---
title: "Build a Q&A chatbot that use your self hosted LLM for renaming invoices files"
date: 2025-02-15
tags: ["LLM", "LangChain", "Ollama", "automation"]
category: professional
source: medium
source_url: "https://medium.com/@damupi/build-a-q-a-chatbot-that-use-your-self-hosted-llm-for-renaming-invoices-files-6a1133d2c4f9"
---

![](/img/2025-02-15-build-a-qa-chatbot-self-hosted-llm-renaming-invoices-1.png)

chatbot assitantI’ve always wanted to understand better all this AI hype, but there’s too much info that it overwhelms me. Besides, I’m the kind of people that likes to learn by practicing, so no matters how much I learn about the topic that, until I don’t put in practices, I don’t completely get it.

This time I had the perfect excuse: my wife is accountant manager, but sometimes has to put her hands in the mud and do some financial assistant tasks. One of these tasks, is to open an invoice file, a .PDF and based on the information the file, rename it in an specific format:

_YYYY.MM.DD_[NAME OF THE SUPPLIER]_[INVOICE ID]_

- YYYY : Year in 4 digits- MM : Month in 2 digits.- DD: Day in 2 digits.For this project I’ve used LangChain, in Python, and using self-hosted models (Deepseek and Llama3.1) served by Ollama. I’ve used also the help, not only from the [LangChain documentation](https://python.langchain.com/api_reference/), but also the tutorials of [how to build a semantic search engine](https://python.langchain.com/docs/tutorials/retrievers/), or how to load a folder of [documents in LangChain](https://quilltez.com/blog/how-load-folder-documents-langchain) from [Quilltez](https://quilltez.com/about-us).

**Requirements:**  
- You need to install [Ollama](https://ollama.com/).  
- Load the Models you are going to answer the questions of your .PDFs, in my case I used [Deepseek R1 7b](https://ollama.com/library/deepseek-r1:7b) and [Llama 3.1 8b](https://ollama.com/library/llama3.1:8b)  
- Python … of course.

As I didn’t want to ask to my wife the invoices because they are for the company she works for, neither use free models that could read those documents, I opted for runing everything locally.

So I created a folder called “invoices”, in the same place where the python file runs, and inside of it, added a few dummy invoices there. The results were optimistic: There were 4 invoices, it did 2 perfect, 1 parcially and 1 wrong.

And without further due, the code:

```python
import os, re, json
from langchain_ollama import OllamaLLM
from langchain_community.document_loaders import PyPDFDirectoryLoader
from langchain_core.prompts import PromptTemplate
from langchain_core.output_parsers import StrOutputParser
from typing import Dict, List

# Load PDF documents from a folder
def load_pdfs(folder_path: str):
    print(f"The folder path is: {folder_path}")
    loader = PyPDFDirectoryLoader(
        path=folder_path,
        glob="**/[!.]*.pdf",
        silent_errors=False,
        load_hidden=False,
        recursive=True,
        extract_images=False,
        password=None,
        mode="single",
        headers=None,
        extraction_mode="plain",
    )
    docs = loader.load()

    # Print the total number of documents loaded from the folder directory.
    print(f"Loaded successfully {len(docs)} documents from the folder {folder_path} directory.")
    return docs

# Split a document into smaller chunks
def split_document(doc):
    text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
    return text_splitter.split_documents([doc])

# Embed and vectorize documents using Ollama embeddings
def vector_docs(model: str, docs: List, search_prompt: str):
    embeddings = OllamaEmbeddings(model=model)
    embeddings.embed_query(search_prompt)
    print("Documents have been vectorized.")
    return docs  # Vector store not needed in this implementation

def extract_invoice_details(llm: OllamaLLM, doc) -> Dict[str, str]:
    content = doc.page_content
    prompt_template = PromptTemplate(
        input_variables=["content"],
        template="""
        Extract the following details from the invoice document:

        - "year": Year of the invoice date in YYYY format (e.g., "2018").
        - "month": Month of the invoice date in MM format (e.g., "03" for March).
        - "day": Day of the invoice date in DD format (e.g., "02" for March 2nd).
        - "supplier": The name of the supplier issuing the invoice.
        - "invoice_id": The unique invoice ID.

        Return the data as a **valid JSON object** with this format:
        {{
            "year": "YYYY",
            "month": "MM",
            "day": "DD",
            "supplier": "Supplier Name",
            "invoice_id": "123456"
        }}

        If any value is missing, return `"None"` for that field.

        Document Content:
        {content}
        """
    )
    
    extract_details_chain = prompt_template | llm | StrOutputParser()
    try:
        response = extract_details_chain.invoke({"content": content}).strip()
        
        # Step 1: Clean the response by removing unwanted tags or metadata
        response_clean = re.sub(r"<think>.*?</think>|```json|```", "", response, flags=re.DOTALL).strip()
        # Print the clean response for debugging
        # print(f"Response Clean:\n{response_clean}")
        
        # Step 2: Extract JSON using a regular expression
        json_pattern = re.compile(r"\{.*\}", re.DOTALL)  # Matches a JSON object, including multi-line
        json_match = json_pattern.search(response_clean)
        
        if json_match:
            json_str = json_match.group(0)  # Extract the matched JSON string
            # print(f"Extracted JSON:\n{json_str}")
            
            # Step 3: Parse the extracted JSON
            details = json.loads(json_str)
            print(f"Parsed JSON of {doc.metadata['source']}:\n{details}")
            
            # Ensure all fields are present
            details.setdefault("year", "None")
            details.setdefault("month", "None")
            details.setdefault("day", "None")
            details.setdefault("supplier", "None")
            details.setdefault("invoice_id", "None")
            
            # Step 4: Check if ALL fields are "None"
            if all(value == "None" for value in details.values()):
                details['filename'] = "Invalid_Invoice"
            else:
                # Format the filename (include all fields, even if some are "None")
                formatted_filename = f"{details['year']}.{details['month']}.{details['day']}_{details['supplier']}_{details['invoice_id']}"
                details['filename'] = formatted_filename
                details['source'] = doc.metadata['source']
                # print(f"Formatted Filename: {formatted_filename}")
                print(f"Source of the document: {doc.metadata['source']}")
            
            return details
        else:
            print("No JSON object found in the response.")
            return {'year': "None", 'month': "None", 'day': "None", 'supplier': "None", 'invoice_id': "None", 'filename': "Invalid_Invoice"}
    except json.JSONDecodeError as e:
        print(f"Error decoding JSON: {e}")
        return {'year': "None", 'month': "None", 'day': "None", 'supplier': "None", 'invoice_id': "None", 'filename': "Invalid_Invoice"}
    except Exception as e:
        print(f"Error extracting invoice details: {e}")
        return {'year': "None", 'month': "None", 'day': "None", 'supplier': "None", 'invoice_id': "None", 'filename': "Invalid_Invoice"}

def sanitize_filename(filename):
    # Replace spaces and special characters with underscores
    return re.sub(r'[\\/*?:"<>|]', "-", filename)

def rename_file(source, new_file_name):
    # Construct full paths for old and new file paths
    old_file_name = source.split("/")[-1]
    folder_path = "/".join(source.split("/")[:-1])
    
    # Sanitize the new file name
    new_file_name = sanitize_filename(new_file_name)
    
    new_full_path = f"./{folder_path}/{new_file_name}.pdf"
    old_full_path = "./" + source
    
    # Debug prints
    print(f"Old file path: {old_full_path}")
    print(f"New file path: {new_full_path}")
    
    # Check if the new file already exists
    if os.path.exists(new_full_path):
        print(f"File '{new_file_name}.pdf' already exists. Skipping rename.")
    else:
        try:
            if os.path.exists(old_full_path):
                print(f"Renaming file: {old_full_path} to {new_full_path}")
                os.rename(old_full_path, new_full_path)
                print(f"Renamed file: {old_file_name} to {new_file_name}.pdf")
            else:
                print(f"The original file '{old_file_name}' does not exist. Skipping rename.")
        except Exception as e:
            print(f"Error renaming file: {e}")

# Main function to process documents
def main():
    model = "deepseek-r1:7b"
    # model = "llama3.1:8b"
    llm = OllamaLLM(model=model)
    document_path = "./invoices/"

    invoices = load_pdfs(document_path)
    print("Processing documents...")

    results = [extract_invoice_details(llm, doc) for doc in invoices]
    
    for result in results:
        filename = result.get('filename', "Invalid_Invoice")
        if filename != "Invalid_Invoice":
            rename_file( result['source'], filename)
if __name__ == "__main__":
    main()
```
Now I’d like to know how to package it and install it in my wife’s computer, without going to the process of the requirement section so, if anyone can help me with that I’ll appreciate it.

Also, I’d love to hear your feedback on how to improve it, for sure there are a lot of room here for improvement.

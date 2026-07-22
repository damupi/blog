---
title: "Beyond MCP: 'gafour' the GA4 CLI for Efficient Reporting"
date: 2026-03-30
tags: ["GA4", "MCP", "CLI", "Google Analytics"]
category: professional
source: medium
source_url: "https://medium.com/@damupi/beyond-mcp-gafour-the-ga4-cli-for-efficient-reporting-0eae6c3f7737"
---

![](/img/2026-03-30-beyond-mcp-gafour-the-ga4-cli-for-efficient-reporting-1.png)

Image created by copilot
### Beyond MCP: Why I Built ‘gafour’ for Efficient GA4 Reporting
A few weeks ago, I came across a headline that stopped me in my tracks: the CEO of Perplexity mentioned they were [moving away from MCP (Model Context Protocol) to focus more on CLIs and APIs](https://byteiota.com/perplexity-ditches-mcp-72-context-waste-kills-protocol/).

As someone deeply passionate about MCPs — having [built several myself](https://github.com/damupi?tab=repositories&q=mcp&type=public&language=&sort=) and written articles praising their potential ([1](https://www.linkedin.com/pulse/mcp-promise-vs-reality-why-googles-ga4-server-misses-mark-david-m-p--d55uf), [2](https://www.linkedin.com/pulse/mcp-server-authorization-auth-20-david-m-p--cbobf)) — this felt like a shock. However, after digging into the technical reasoning, the message was clear: “_Amigo, you’re wasting tokens._”

#### The Token Efficiency Dilemma
Does this mean we should abandon MCPs? Not at all. As a protocol, they are incredibly useful for defining standardized workflows. But it’s true that when you’re dealing with complex data retrieval, a dedicated CLI or direct API integration can save a significant amount of context window and tokens.

Driven by this need for efficiency, I developed gafour, a specialized CLI for Google Analytics 4 reporting.

#### Introducing gafour: Enhanced GA4 Reporting
I didn’t start from scratch. I want to give full credit to Suliman Benhalim for his excellent work on [ga-cli](https://github.com/sulimanbenhalim/ga-cli). While his tool is great, I found it lacked certain advanced features I needed for my daily workflow — specifically, more robust filtering in reports.

With gafour, I’ve introduced several key improvements:

- **Advanced Filtering:** Tailored for complex reporting needs that require granular data subsets.- **Streamlined Authentication:** Refined the auth flow for a smoother user experience.- **Metadata Integration:** Added the [getMetadata](https://developers.google.com/analytics/devguides/reporting/data/v1/rest/v1beta/properties/getMetadata) method. Instead of hunting for dimensions and metrics manually, you can now retrieve the full capabilities of a property in one go.- **Compatibility Checks:** Integrated [checkCompatibility](https://developers.google.com/analytics/devguides/reporting/data/v1/rest/v1beta/properties/checkCompatibility) to verify if your chosen dimensions and metrics are valid together before running the report.- **Batch Processing:** Added [batchRunReports](https://developers.google.com/analytics/devguides/reporting/data/v1/rest/v1beta/properties/batchRunReports) support. This allows you to group multiple requests into one, helping stay within the Google Analytics Data API limits and reducing latency.
#### Focused on Reporting
While gafour includes some administrative calls (via Google Analytics Admin API) to list accounts, properties, or custom dimensions, its core purpose is reporting. It is not designed to create accounts, properties, or custom dimensions — it’s built to extract and analyze data efficiently.

#### Bringing it to AI: The GA4 Agent
To bridge the gap between this CLI and the LLM world, I’ve also created a specialized Agent.

Most GA4 agents I encountered were limited to managing a single property. In my professional life, I manage dozens. This agent — delivered as a comprehensive .md file—is specifically designed to handle multiple properties using gafour as its engine. While it's currently a "pure prompt" version without custom hooks, the structure is ready for further automation.

#### Get Started
If you want to save tokens and take control of your GA4 data, you can find the installation instructions and documentation in the repository.

**GitHub Repository:** [**gafour**](https://github.com/damupi/gafour/)

If you find any bugs or have suggestions for new features, feel free to [open an issue](https://github.com/damupi/gafour/issues). Let’s make GA4 reporting faster and cheaper together!

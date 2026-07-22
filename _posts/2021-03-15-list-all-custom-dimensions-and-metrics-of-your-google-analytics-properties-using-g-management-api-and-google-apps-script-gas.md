---
title: "List all custom dimensions and metrics of your Google Analytics properties using G Management API and Google Apps Script (GAS)"
date: 2021-03-15 09:20:00.006
tags: ["Google Analytics", "Google Apps Script", "GAS", "GA", "API"]
category: professional
---

Well for once in many time, actually I think the first one I’m not gonna talk about GTM. This time I’m gonna dedicate this article to GAS, Google Apps Script. Some people will call it scripting other ones will say it can’t be considered as that, well readers, judge by yourselves and make an opinion in this [overall view](https://developers.google.com/apps-script/).

In this article I’m going to provide you a google spreadsheet where you can get a list of all your properties of Google Analytics and also extract all the custom dimensions and properties from a Google Analytics property.

The only thing that is required here is to have Google Analytics properties with custom dimensions or custom properties and google drive for creating a Google Spreadsheet.

I have created a customised menu where all the functions are allocated there. The menu es called “GA - Lists” and is loaded when you open the Google Spreadsheet.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhgu12BWFektRFFDT8ujE9EcmUpCryRya_uaoDfZ4a9l9vqxb6jnToZiu4S2WsGCOil5ucxlb99SGrkyl9agegq3gCwQmuQo4xSYlL6bWFvPDc8f-mM0R3iIuNzTzYJuU9NCf3R/w400-h131/Picture+1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhgu12BWFektRFFDT8ujE9EcmUpCryRya_uaoDfZ4a9l9vqxb6jnToZiu4S2WsGCOil5ucxlb99SGrkyl9agegq3gCwQmuQo4xSYlL6bWFvPDc8f-mM0R3iIuNzTzYJuU9NCf3R/s941/Picture+1.png)

For checking all the code behind the scenes, you only have to go to “Tools”. And the script editor.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgEB4gIX6vX-tik0eekAxVmT9lYGrsehw5YHMvCz06GxbkKjvT0On6nRCwb1woNP9GkSwcZMMScT3PYzOu4Rwu4_ycbc6VWCGtppj1ctMePLloLbTlcUy18EVWIxyvmOf6VTEfu/w400-h155/Picture+2.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgEB4gIX6vX-tik0eekAxVmT9lYGrsehw5YHMvCz06GxbkKjvT0On6nRCwb1woNP9GkSwcZMMScT3PYzOu4Rwu4_ycbc6VWCGtppj1ctMePLloLbTlcUy18EVWIxyvmOf6VTEfu/s941/Picture+2.png)

  

It will open a new tab with all the code behind. I know I should have cleaned the code because it contains more lines that the ones we need for get a list of all our Google Analytics views, and the custom dimensions and properties of each property.

The reason behind is that I created before a script to iterate with all the properties and extract all CDs and CMs but in my company we have a bunch of properties and each of it contains another bunch of CDs and CMs. When I tried to run this script, it happened that I exceeded the time for it and I got a time out. For sure there’s someone outside there smarter than me (actually that’s not hard to find) and have another idea of how to retrieve them without a timeout error.

I have created a tab called README that is self-explanatory, explaining what every part of the menu does.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiR5aOL1bOc-HZ31xLf0-K0jC4Adap_Iz8dQrVAhuKySFaRPGI3ANGxp0L_4CY4FR65xhwTmoG7cm8otl2T_EFfUgSI0aKuSxL7kSrkxjWyq7nzK2yI6z6HPhcIa3uM92EdhjmL/w400-h153/Picture+3.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiR5aOL1bOc-HZ31xLf0-K0jC4Adap_Iz8dQrVAhuKySFaRPGI3ANGxp0L_4CY4FR65xhwTmoG7cm8otl2T_EFfUgSI0aKuSxL7kSrkxjWyq7nzK2yI6z6HPhcIa3uM92EdhjmL/s941/Picture+3.png)

  

So first thing: List of views that you have access to Google Analytics (GA). Chop chop, click on “GA – Lists” --> “List Views” --> “List all Views”.

It will empty or replace the tab “ALL VIEWS” and will write down all the views you have access to GA.

You can check line 97 of the code in the function getGAViews().

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgMHRYTNXrjQJP_IjM9CaZil1Qjf7vRhMYvmFW1l3wzF1TAH9KuL6PPGDT-XDjKmEORuk1bLsrQBoscdYLujUkrE69VQcypGAeqQhzReTCXkvsjhFEptMQOg5jr6jRv2SC3c3pt/w400-h219/Picture+4.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgMHRYTNXrjQJP_IjM9CaZil1Qjf7vRhMYvmFW1l3wzF1TAH9KuL6PPGDT-XDjKmEORuk1bLsrQBoscdYLujUkrE69VQcypGAeqQhzReTCXkvsjhFEptMQOg5jr6jRv2SC3c3pt/s779/Picture+4.png)

  

If you want to know all the properties that you have access of GA, same old same old: “GA - Lists” --> “List Properties” --> List all Properties. You can find that part of the code in the line 121 in the function name getGAProperties().

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgda-39Cr29GYFaxeiyfLkiswlSC3Jcg8ojsWSax7XvbPAn3eo-rUQAk8SSozZVwtjprIXcbTmhVip3FW6dfAiMzhsoLhXRsn05sgB85LPVHNuds4VahpR_Xg1Lnf6KxB54R2lO/w400-h203/Picture+5.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgda-39Cr29GYFaxeiyfLkiswlSC3Jcg8ojsWSax7XvbPAn3eo-rUQAk8SSozZVwtjprIXcbTmhVip3FW6dfAiMzhsoLhXRsn05sgB85LPVHNuds4VahpR_Xg1Lnf6KxB54R2lO/s791/Picture+5.png)

  

Now it comes the tricky one, if you want to extract all the CDs and all the CMs of a GA property, you first you need to know the property ID “UA-123456-1”, this is why I created the “List all properties”.

So, the only thing you need to do is copy the UA property ID and then when you run the menu it will prompt you for the property ID. Don’t panic this prompt is pretty and despite of my shitty English you will perfectly understand what I mean about.

Then “GA - Lists” --> “List CD & CM” --> “List all CD and CM for a property ID”

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjbL3i5JwUaNItf4Fs3KQ5Kk5QL2gGjAdY7R4nEjw8_XhLKk4t5jPZTT0jnmQapGFLogdL4oCbWGY1jRPK6H9nHVNaUQ8V-MY7DfAcnHIsmy_AxXTDRg3Eirn8Pq9yMxWh48_lc/s320/Picture+6.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjbL3i5JwUaNItf4Fs3KQ5Kk5QL2gGjAdY7R4nEjw8_XhLKk4t5jPZTT0jnmQapGFLogdL4oCbWGY1jRPK6H9nHVNaUQ8V-MY7DfAcnHIsmy_AxXTDRg3Eirn8Pq9yMxWh48_lc/s941/Picture+6.png)

  

As you have already copied the property ID, for example “UA-123456-1”, when you got this popup, paste it on the text box and click OK

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjmFMokbD21l-dznxA9mulhix3c42x8lAZTzKicbcRoA3VUr4JxDzOaEQWbwXE3LpjP4uzcQWRZ0i4vCyYsTcL2V5bGuyMPn0HnV8mwVJBSIhJd7Gj1ti1DfDjpVXPGf2ANSQsi/s320/Picture+7.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjmFMokbD21l-dznxA9mulhix3c42x8lAZTzKicbcRoA3VUr4JxDzOaEQWbwXE3LpjP4uzcQWRZ0i4vCyYsTcL2V5bGuyMPn0HnV8mwVJBSIhJd7Gj1ti1DfDjpVXPGf2ANSQsi/s720/Picture+7.png)

It will create a new tab with the name if the ID and all the custom dimensions and metrics under that property

More info in the code on line 223, function name ListCDAndCMPerProperty()

And now, the time you’ve been waiting for: [here you can have view access to the document](https://docs.google.com/spreadsheets/d/1Pwumrr5Ag6nEEKorWjoTB5prED290oLA2c9XMIt_GBE/edit?usp=sharing). Feel free to copy to your G Drive and don’t forget to mention how much you love damupi for making your life easier.

Live long and prosper.

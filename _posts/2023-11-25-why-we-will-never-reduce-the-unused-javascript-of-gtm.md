---
title: "Why we will never reduce the unused JavaScript of GTM"
date: 2023-11-25
tags: ["GTM", "Google Tag Manager", "performance", "web analytics"]
category: professional
source: medium
source_url: "https://medium.com/@damupi/why-we-will-never-reduce-the-unused-javascript-of-gtm-2f2afc2520d6"
---

![](/img/2023-11-25-why-we-will-never-reduce-the-unused-javascript-of-gtm-1.png)

Throughout my almost 10 years of experience as Web Analyst, in each of the companies that I’ve worked for, there’s always someone that ask you if we can reduce the size of the Google Tag Manager (GTM) script.

We always want to perform our pages, to be the best on speed, usability, google searches’ ranking, …. My colleagues, SEO experts, Front End developers, Clients for the Agency I’ve worked for says to me that they have checked Pagespeed and indicates that the size of the file of GTM can be reduced, thus the overall page will be lighter, faster, thus UX will improve, and we will always be in the top 3 of search of Google and our users will be happy to visit our page without any delays, glitch, blahblahblah, …..

Well, is true, … you can always improve your GTM file size, but is also true that you will never reduce it as much as [PageSpeed Insights](https://pagespeed.web.dev/) would like to. This is what this article is about, enlight you that you won’t get rid of the Unused Javascript in the GTM file. So let’s get to the point.

If you analyze an URL with PageSpeed that contains a GTM file, you will see an _opportunity_ to improve the performance that says _Reduce Unused Javascript_.

Firstly, let me read what PageSpeed says: “**_These suggestions_**_ can help your page load faster. They _**_don’t _**[**_directly affect_**](https://developer.chrome.com/docs/lighthouse/performance/performance-scoring/?utm_source=lighthouse&utm_medium=lr)**_ the Performance score_**_._” In other words, even in the case you reduce that unused JavaScript of GTM, you cannot assure that the performance score is going to change.

Secondly, if we continue reading [the documentation](https://developer.chrome.com/docs/lighthouse/performance/performance-scoring/#what-can-developers-do-to-improve-their-performance-score) we find “_In the Lighthouse report, the Opportunities section has detailed suggestions and documentation on how to implement them._”. Thus again, is a suggestion, nothing else, nothing more, than that.

Regardless that, after reading the above, we must agree that reducing the unused JavaScript is not going to assure as that the performance score is going to improve, … But let’s suppose it does. How can we remove that “unused” JavaScript?.

If we check [the documentation](https://developer.chrome.com/docs/lighthouse/performance/unused-javascript) it says “[_Lighthouse_](https://developer.chrome.com/docs/lighthouse/overview/)_ flags every JavaScript file with more than 20 kilobytes of unused code_”. So, if the file is bigger than 20kb, is susceptible of being flagged as unused. But how Lighthouse knows what is used in the code and what is not?. If you want to know it, download [the repository](https://github.com/GoogleChrome/lighthouse/blob/main/core/audits/byte-efficiency/unused-javascript.js), and have a look. From now, it’d say is enough if I know how to detect each line of code that “I’m not using”.

For that, [it explains you](https://developer.chrome.com/docs/lighthouse/performance/unused-javascript/#detect-unused-javascript) to use the “Coverage tab” in the Chrome Dev tools, that you can select that file and it [will indicate you](https://developer.chrome.com/docs/devtools/coverage/#analyze), in the last column, which bytes are unused, in red, and which bytes are used, in green.

So let’s continue supposing that I want to reduce the unused Javascript code of the GTM js file. I go to [https://www.gamblin](https://www.gamblin)g.com , open the Chrome console, and see the Coverage Tab:

![](/img/2023-11-25-why-we-will-never-reduce-the-unused-javascript-of-gtm-2.png)

In the 4th position we have the GTM js file, that, in this occasion, indicates as that there’s a 26% of code that is not being used, according to GoogleSpeed guidelines.

If we go farther, we can see that the GTM js file has a 74% of Coverage.

![](/img/2023-11-25-why-we-will-never-reduce-the-unused-javascript-of-gtm-3.png)

Because in the column below of Unused Bytes, it says that we are not using a 26% of code of that file.

So, I will scroll down in the top right with the lines of the code, we should see some lines in red. First one I see is this:

![](/img/2023-11-25-why-we-will-never-reduce-the-unused-javascript-of-gtm-4.png)

This part belongs to a JavaScript JSON or dictionary that starts to create all the macros:

![](/img/2023-11-25-why-we-will-never-reduce-the-unused-javascript-of-gtm-5.png)

Let me explain you what this “unused javascript” does: It creates a [User-defined type of variable](https://support.google.com/tagmanager/answer/7683362?hl=en#aev) called “Auto Event Variable” (__aev) available for the user in the GTM interface.

![](/img/2023-11-25-why-we-will-never-reduce-the-unused-javascript-of-gtm-6.png)

As we are not using that type of variable in our container, maybe Lighthouse has detected that we don’t need that part of code.

**And now is why I write in the beginning it’s impossible to remove unused JavaScript, because that lines of code is part of GTM itself. In this example, despite you are not using that variable, it must be available in the platform.**

Bottom line, can you reduce the the size of your GTM file?. Yes, improve your GTM container. Can you reduce all that you wish from the file? Nope.

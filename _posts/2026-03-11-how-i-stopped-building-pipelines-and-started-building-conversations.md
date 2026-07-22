---
title: "How I stopped building pipelines and started building conversations"
date: 2026-03-11
tags: ["Analytics", "DataEngineering", "AI", "Automation", "MCP", "PaidMedia", "MarTech", "AgenticAI", "GrowthMarketing", "BI"]
category: professional
source: linkedin
source_url: "https://www.linkedin.com/pulse/how-i-saved-hours-manual-reporting-every-week-ai-automation-m-p--kxz5c"
---

![](/img/2026-03-11-reporting-automation-1.png)

Another day at work, another process improved. I sometimes joke that I was born lazy—my brain is constantly hunting for shortcuts or ways to make machines do the work, especially for repetitive tasks.

Years ago, a manager gave me what I call a "monkey task." Our company used fingerprint logs for office access, and HR was spending 2 hours manually checking absences and punctuality. I built a macro that did it in 5 minutes. The principle this time was the same: Stop doing the repetitive stuff.

Working in a Growth team, we need constant updates on Paid Media. We need data from Meta, Snapchat, and our Data warehouse. Even with existing reports, they never quite covered everything. We ended up in a cycle of "copy and paste" between documents to create one merged view.

My first move was standard automation: Python scripts pulling from APIs into a unified format. It worked technically, but it was organizationally fragile. The specs were a moving target. New filters, new dimensions, new edge cases every week. I was stuck in a loop: gather specs → build → specs change → rebuild.

![Created with draw.io](/img/2026-03-11-reporting-automation-2.png)

Until I came with this in my head: What if I create something that gathers everything, and then the user decides which data to pull?

I shifted to an AI-native architecture using the Model Context Protocol (MCP). This allowed me to expose each data source as a "tool" that an AI agent could query dynamically. I built different MCPs from different starting points:

Meta Ads (Security-First): Rebuilt the connector internally so API credentials stayed in our environment.
Snapchat Ads (Custom): Built from scratch to expose the Marketing API (campaigns, performance, creative stats) to the AI.
Data warehouse: Integrated OAuth authentication, exposing targeted SQL queries for our specific needs.

![](/img/2026-03-11-reporting-automation-3.png)

The reporting workflow is now a conversation: "show me last week's performance across Meta and Snapchapt, filtered by Data warehouse conversions." The result is a consolidated answer in minutes. No manual API calls. No spreadsheet wrangling.

But something happened that I didn't expect. Once the team started using it, it evolved. Colleagues started asking for more than just reports. They are now able to create campaigns directly, generate creative briefs, and pull audience data on demand.

The data flow has blended. It's no longer a rigid pipeline with fixed columns; it's an open interface. As I was telling a colleague: when specifications are a moving target, rigid pipelines will always lag. 

A flexible AI-native interface that can answer any question about your data is far more durable than a script that answers one question perfectly.

#Analytics #DataEngineering #AI #Automation #MCP #PaidMedia #MarTech #ArtificialIntelligence #Claude #AgenticAI #GrowthMarketing #BI

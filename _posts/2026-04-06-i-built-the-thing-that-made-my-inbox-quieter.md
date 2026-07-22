---
title: "I built the thing that made my inbox quieter"
date: 2026-04-06
tags: ["AI", "Automation", "Analytics", "MCP"]
category: professional
source: substack
source_url: "https://damupi256603.substack.com/p/i-built-the-thing-that-made-my-inbox"
---

Not long ago, a colleague pinged me. His role is commercial — the kind of work that sits between strategy and execution, not in the data stack. His message was not what I expected.

By the end of the conversation, he had a fully automated weekly report — traffic, engagement, and conversion signals across multiple properties, scheduled, formatted, delivered to Slack without anyone touching it. He’d also optimised the API calls himself, cutting requests by two-thirds once he understood the batch pattern.

In ten years of analytics, that conversation didn’t used to exist. For most of that time, the pattern was predictable: someone needed numbers, I had the access, I delivered a table or a chart or a slide. Repeat.

Dashboards were supposed to fix this — build a self-serve interface, let people pull their own data, stop being the bottleneck. It never quite worked. The tool existed. The habit never formed. According to Luzmo’s 2025 research, 40% of users don’t believe dashboards consistently help them make better decisions. Adoption hovers around 20% despite years of BI investment. So the requests kept coming. Different wrapper, same content: “can you pull this for me.”

What I’ve spent the last few years doing is building something different: a layer that puts the data directly inside the AI. MCP — Model Context Protocol — is an open standard for connecting AI assistants to live data systems. The protocol is scaling fast — from roughly 100,000 downloads in November 2024 to eight million by April 2025. It’s becoming infrastructure. You build a server that exposes a data source as something the AI can query in natural language, in real time. No dashboard. No export. No middleman. I’ve built more than ten of these — for web analytics, search console data, data warehouses, ad platforms. The goal was always the same: remove the gap between the stakeholder and the data.

And that brings me back to my colleague. His message wasn’t “can you build me a report.” It was a technical question — he’d connected Claude and n8n to a GA4 MCP server, hit an authentication error, properties returning empty, and needed help unlocking the door. He’d already designed the architecture. He’d already chosen his metrics, his dimensions, his delivery format. He just needed someone who knew the access layer.

The conversation wasn’t “can you get me this.” It was “I’ve built this, help me understand why it’s broken and what it should tell me.”

That’s a completely different kind of inbox message. The old one is about access. The new one is about interpretation. The old one I could theoretically automate. The new one requires someone who’s been close enough to the data — and close enough to the business — to say: “that number will go up next month regardless, here’s why” or “this metric looks fine but it’s hiding a problem in this segment.”

I want to be honest about the limits. Only about 6% of companies have scaled generative AI to production. McKinsey finds that 92% of adoption obstacles are people and process, not technology. And there’s a less comfortable truth: when the AI answers questions directly, bad data doesn’t produce a misleading chart a careful reader might catch. It produces a confident-sounding wrong answer. The dbt Labs 2024 survey found 57% of practitioners cite poor data quality as their primary issue, up from 41% two years earlier. That number rising alongside direct AI data access is not reassuring.

Someone has to own what the AI is allowed to answer. Someone has to know where the data is trustworthy and where it isn’t. The analyst’s role doesn’t disappear — it moves upstream, into the infrastructure layer, into the judgment calls the AI can’t make.

The inbox is quieter. What’s left in it is better. And my colleague, who I’d have described six months ago as a data consumer, is now building automated reporting pipelines and optimising API batch calls on his own.

I’m still figuring out what to call that.

If you’re an analyst watching this shift happen — what kind of requests are disappearing for you? And what’s replacing them?

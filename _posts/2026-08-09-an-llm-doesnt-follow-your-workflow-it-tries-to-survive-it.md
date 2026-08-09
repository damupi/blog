---
title: "An LLM doesn't follow your workflow. It tries to survive it."
date: 2026-08-09
tags: ["AI agents", "MCP", "workflows", "LLMOps", "multi-agent systems"]
category: professional
---

![](/img/2026-08-09-an-llm-doesnt-follow-your-workflow-1.png)

I wrote a simple rule into my subagent's prompt:
*After 3 failed attempts or blocks, give up and return control to the coordinator.*

Simple. Explicit. Unambiguous.

I ran the system. The subagent hit a block on the first try, then another, then another.

It didn't stop at 3. It kept going. The entire pipeline stalled while the agent quietly kept trying to force its way through.

Since I have logging in place (monitoring and workflow design—two birds with one stone), I pulled the logs to see what was happening. The subagent had tried well past the limit, getting blocked every single time.

WTF. I wrote the rule. It ignored the rule.

That's when it clicked: **an LLM isn't following your workflow; it's trying to reach its target.**

If giving up conflicts with "complete the task," reaching the target wins—unless you explicitly make giving up part of the target itself.

I added a hard guardrail section spelling out *exactly* when to stop and hand back control. That fixed it. But it completely reframed how I view agentic architecture:

> A workflow isn't documentation for the agent. It's a constraint you have to actively fight to enforce.

This lesson landed right as I started scoping v2.0 of an MCP I built for connecting LLMs to marketing data sources. V2 is a different beast—multiple agents, data pipelines, Python scripts, a backend, and a frontend.

My initial instinct was to give the agents guidelines and let them negotiate the flow between themselves.

I've dropped that instinct.

A system with that many moving parts needs rigid engineering to hold its structure together—not AI judgment. The AI's job is to help build that engineering and fix it when something breaks.

Guardrails and observability aren't nice-to-haves bolted onto an agentic system. They are the difference between a real workflow and a light suggestion.

**I'm curious where others land on this:**

For complex multi-agent systems, are you still letting agents negotiate their own flow, or have you moved to hard-coded architecture where the AI is the mechanic instead of the driver?

*These are my personal opinions, not my employer's. I've been wrong before—feel free to tell me I'm wrong again.*

#AIagents #MCP #AIworkflows #LLMOps #MultiAgentSystems #AIEngineering #Observability

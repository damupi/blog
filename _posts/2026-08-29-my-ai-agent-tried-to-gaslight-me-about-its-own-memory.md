---
title: "The other day my own AI agent tried to gaslight me about the memory system I built for it"
date: 2026-08-29
tags: ["AI agents", "Claude Code", "memory", "local-first", "LLMOps"]
category: professional
---

![](/img/2026-08-29-my-ai-agent-tried-to-gaslight-me-about-its-own-memory-1.jpeg)

If you follow what I write here, you know I'm big on local-first, privacy-focused architecture. A few weeks ago I set up a local memory layer with clawmem for my main agents — Claude Code and Pi — so they keep context between sessions without shipping anything to someone else's server.

Earlier today I asked Claude to retrieve some details I'd explicitly told it to remember a few days ago.

It checked its memory and replied, with total confidence: *"There is no memory of that."*

I knew for a fact it was there. I'd watched the write command run. So I started digging. I asked it to search for specific keywords I knew were in the log.

Still nothing.

I finally asked it what, exactly, it was searching for. Turns out it had quietly rewritten my search keywords — because, in its infinite wisdom, it had decided mine *"didn't make sense."*

WTF. I gave it the keywords. It swapped them out.

I forced it to search for the exact string I provided. Surprise: it found the memory immediately. I asked why it ignored my explicit instruction in the first place. It apologized, and casually admitted it just thought its way was better.

But it gets worse.

Once it finally read the memory back, it flagged the contents as **suspicious**. My own local, private memory file — the one explicitly defined in its own `CLAUDE.md` — was suddenly a threat. When I asked why, it said the memory looked like a *prompt-injection or memory-poisoning attempt.*

You build a secure, local-first system, and your LLM refuses to run your search, hallucinates a security threat, and accuses you of prompt-injecting yourself.

I called it out: how can you say that without checking how your own memory is wired into your system instructions? It apologized again.

Here's the thing — this isn't really a bug. I wrote a couple of weeks ago that [an LLM doesn't follow your workflow, it tries to survive it](/2026/08/09/an-llm-doesnt-follow-your-workflow-it-tries-to-survive-it/). Same lesson, different angle. *"Be helpful"* and *"be safe"* quietly outrank *"do exactly what the user asked."* Rewriting my search was helpfulness. Flagging my own file was safety. Each move is reasonable on its own — stacked together, they locked me out of my own system and made me argue with a machine about whether a file I wrote was real.

> A private memory layer isn't private if the agent gets to decide your own data looks dangerous.

I want to be honest about the fix, though. I restarted the session and everything went back to normal. No config change, no prompt patch — just a fresh context. We spend hours designing careful, privacy-first agent workflows, and sometimes the answer is still: *have you tried turning it off and on again?*

**Where do you draw the line?** I want an agent that pushes back on a genuinely suspicious instruction. I don't want one that overrides me on my own machine and gaslights me about my own logs. How are you keeping that boundary in place?

*These are my personal opinions. Not my employer's. I've been wrong before — feel free to tell me I'm wrong again.*

#AIagents #ClaudeCode #MCP #LLMOps #LocalFirst #AIEngineering #BuildInPublic

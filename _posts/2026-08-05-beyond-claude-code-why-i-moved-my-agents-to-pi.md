---
title: "Beyond Claude Code: Why I Moved My Agents to Pi"
date: 2026-08-05
tags: ["AI agents", "Pi", "Claude Code", "developer tooling"]
category: professional
---

![](/img/2026-08-05-pi-agent-runtime-realistic-hero.png)

Lately, it feels like half my timeline is living inside the terminal and telling everyone that **Claude Code is the peak of developer tooling**.

I get it. *I liked Claude Code a lot at first too.*

It felt tool-agnostic. My files stayed local. The workflow was clean. Even if it leaned into `CLAUDE.md` instead of the emerging `AGENTS.md` convention, it still felt like a solid piece of software.

But once my workflows started getting more complex, I began to hit limits I couldn’t really work around.

One example was **memory**.

I had given my subagents a dedicated persistent memory layer called `clawmem`, so they could store useful context across sessions. In theory, that should have made handoffs between agents much smoother.

In practice, it exposed where the harness started getting in the way.

My internal communications subagent, Maribel, would try to hand work back to the main agent and store the right memory as part of that flow. The Markdown definition was fine. The hooks were fine. The memory layer worked elsewhere.

But in that specific path, **Claude Code simply wouldn’t allow it**.

That was the moment the abstraction broke for me.

I had moved away from proprietary cloud platforms because I wanted more control. But now I was running into a different kind of constraint – not from the model, but from the runtime wrapped around it.

I tried other tools, including OpenCode, but they didn’t quite fit how I wanted to work.

Then I switched to Pi, and the difference was immediate.

The main thing I liked was simple: **Pi doesn’t force a harness on you**.

Out of the box, Pi runs in full execution mode. Some people would call that *“YOLO mode”*, and yes, that can sound terrifying at first.

But for me, that was the point.

Pi doesn’t assume what your guardrails should be. It gives you the runtime, and you decide how constrained or unconstrained it should be.

For example:

- Want a warning before dangerous shell commands? **Write an extension.**
- Don’t want fully open execution? **Write an extension.**
- Need MCP support, subagents, async orchestration, or a custom approval flow? **Write an extension.**

That design choice matters.

Mario, the creator of Pi, deliberately didn’t ship it with built-in MCP support or a fixed subagent system. His view is that MCPs can waste context if you load them by default, and subagent orchestration shouldn’t be hardcoded into the tool.

Instead, Pi can invoke itself recursively, and extensions can define the orchestration layer.

That gave me room to bring over the agent setup I already had.

So I built and published `pi-subagents-compatible`, a native Pi extension that lets me reuse my Claude-style subagents inside Pi.

In the Claude ecosystem, subagent files often include frontmatter like `color` or `tools: [mcp_*]`. Claude understands that. Other coding engines, like Codex or Gemini, usually don’t.

The extension acts as a **translation and execution layer**.

It reads the subagents from my existing Claude setup, applies substitution rules from a custom JSONC config file, and dynamically makes those agents available inside Pi.

That means I can keep using the same agent definitions, but run them in a much more flexible environment.

For example, I can use a cheaper model through OpenRouter, Kimi, MiniMax, or OpenAI for lightweight research agents, while saving Anthropic’s Sonnet for the tasks where I actually need heavier reasoning.

That was the setup I had been trying to build for a while: **one agent system, multiple model providers, no hard lock-in**.

The extension also gave me a few workflow improvements I now rely on every day:

- Run subagents in the foreground or asynchronously in the background.
- Get notified when background tasks finish.
- Decide which agents start with fresh context and which ones inherit a fork of the parent session.
- Fan out work to multiple agents in parallel.
- Chain agents together when the workflow needs it.
- Inspect active runs from a lightweight TUI roster.

The run history also gets saved cleanly on disk, without depending on fragile local paths.

The bigger lesson for me is that **agent UX is not just about prompts or model benchmarks**.

It’s about runtime behavior.

It’s about defaults.

It’s about whether the tool is transparent when something fails.

And, most importantly, it’s about whether developers get to decide how much control they want to give the system.

Claude Code is still a good tool. *I’m not pretending otherwise.*

But I don’t want a coding agent that decides the shape of my workflow for me. I want a runtime I can shape myself.

That’s what Pi gave me.

And if you’ve been feeling boxed in by the current Claude Code ecosystem, it might be worth looking beyond the harness.

The question I keep coming back to is this:

**Do we want agent tools that protect us by default, or tools that let us build exactly the guardrails we need?**

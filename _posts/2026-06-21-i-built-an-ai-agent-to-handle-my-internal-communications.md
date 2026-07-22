---
title: "I Built an AI Agent to Handle My Internal Communications. Here's What That Actually Looks Like."
date: 2026-06-21
tags: ["AI agents", "Claude Code", "productivity"]
category: professional
source: medium
source_url: "https://medium.com/@damupi/i-built-an-ai-agent-to-handle-my-internal-communications-heres-what-that-actually-looks-like-5f902dd5161f"
---

![](/img/2026-06-21-i-built-an-ai-agent-to-handle-my-internal-communications-1.png)

Her name is **Maribel**. Named after the best office manager I ever worked with, back at my first real job. She had this uncanny ability to know exactly how to talk to everyone — the partners, the assistants, the interns — and match the moment perfectly every time.

My Maribel is a Claude Code subagent. She lives in a markdown file. She handles my Slack and Outlook end-to-end: reads incoming messages, drafts replies calibrated to the relationship, gets my approval, and sends. She doesn’t do it for fun — she does it because I was writing too many bad messages at the wrong time.

Let me show you what that looks like under the hood.

### The Problem She Solves
I work fully remote, in English, which is not my first language. That’s already two strikes. Add that sometimes a message lands at 4pm on a Friday when a deadline slipped and someone is asking about a process I’ve documented three times already. The reply I want to write is not the reply I should send.

The insight wasn’t mine — I half-stole it from watching how YouTube creators think about platform tone. LinkedIn is not Twitter. A thread is not a newsletter. The same idea needs different packaging depending on where it lands and who reads it.

I applied that to internal communication. Messaging my manager is not the same as messaging a peer on my team. Messaging a VP who doesn’t know me is not the same as messaging a teammate I’ve worked with for two years. I already knew this intuitively. I just wasn’t always executing on it well.

So I built a system that does.

### What the Agent File Actually Is
A Claude Code agent is a markdown file with a YAML frontmatter block and a system prompt. That’s it. No Python, no API wrappers, no framework. The file looks like this at the top:

```
---
name: maribel
description: "Maribel is the user's internal communications strategist..."
tools: Bash, mcp__claude_ai_slack__*, mcp__claude_ai_MS_Outlook__*
model: sonnet
color: purple
---
```
What follows is a full operating procedure — not a vague “be helpful” prompt, but a step-by-step workflow with decision trees, failure modes, and explicit guardrails. The file is the persona. It defines not just what Maribel does, but _how she decides what to do_.

Her responsibilities, in order: read the incoming context, look up the recipient in the org chart, select the right channel (always mirror the channel I used to invoke her — if I mention a Slack message, the reply goes on Slack), pull tone from the appropriate file, check memory for prior context on this person, draft, show me the draft, wait for my approval, then send.

The tone table looks like this:

- Relationship Tone file Manager : skip-level upward.md- Senior leadership : C-level senior-guide.md- Peer, same team casual.md- Peer with technical context technical-peer.md- Cross-team :direct report collaborative.mdShe reads the full tone file before drafting — not a summary, the whole thing. And before that, she reads damupi-voice.md, which is a file I maintain manually describing how I actually write: my cadence, the phrases I overuse, the ones I avoid, the level of directness I use with different people. The tone file sets the register. The voice file sets the fingerprint. Both apply together.

The org chart is a static JSON file I downloaded from our HR system once and keep locally — just the schema, names, reporting lines. Maribel reads it directly and searches it for whoever she needs. If someone is missing because they joined last week, she falls back to the same file and flags the gap to me rather than guessing.

The one rule I never wanted to bend: **she never sends anything without my explicit approval**. That constraint isn’t a safety theater checkbox. It’s the whole point. The draft loop is:

draft → show → ask “Send this via Slack? (yes / modify / cancel)” → wait.

If I say modify, she doesn’t re-run the research steps — she already has all the context, she just applies the change. That distinction matters a lot for speed.

A typical invocation: _“Maribel, my manager is asking why the Q2 tagging audit isn’t done yet — tell him we hit a schema change in the event layer and we’re back on track for Thursday.”_

She looks up the relationship (manager → upward.md), checks if she has recent context on him from memory, drafts something professional and non-defensive, and shows it to me.

### The Memory Layer
This is where it gets more interesting than a system prompt alone.

Maribel has persistent memory. Every time she interacts with someone, she’s supposed to learn something about them — how they write, how they respond, what channel they prefer, what didn’t land last time. That memory gets used in the _next_ session, automatically, without me asking.

The infrastructure is [ClawMem](https://github.com/yoloshii/clawmem) — a Node.js tool I run as both an MCP server and a CLI. It stores everything in SQLite with vector embeddings, living in a local folder on my machine at ~/Documents/memory/.

Maribel has her own collection inside it, separate from the main assistant collection. She writes entries like this:

```
clawmem diary write "My manager prefers bullet points over paragraphs in async updates" -a maribel -t comms
clawmem diary write "Slack DM is better than email for him — responds within the hour" -a maribel -t comms
```
The -a maribel flag routes this to the maribel collection specifically. You can add as many collections as subagents you want — each agent keeps its own memory silo, isolated from the others.

But here’s the thing that tripped me up and might trip you up too.

### Why Subagent Memory Needs Its Own Silo
When Maribel runs, she runs as a subagent — Claude Code spawns her in a separate context. The parent session doesn’t see what she does inside her context. This matters because Claude Code has hooks that run automatically at the end of every session and extract decisions, patterns, and handoffs from the transcript.

Those hooks don’t see Maribel’s work.

So if Maribel observed that someone writes in short bursts and signs off quickly, that observation would disappear — unless she writes it to ClawMem herself, explicitly, before returning control to the parent session.

The original design I was running had a simpler memory system: flat markdown files under ~/.claude/projects/*/memory/, typed as user, feedback, project, reference. That system is gone now. I removed it. The problem with flat files for this use case is that they load wholesale or not at all — there's no retrieval, no ranking, no "here's the three most relevant fragments from everything I know about this person." ClawMem gives me vector search. The context-surfacing hook runs on every prompt and injects the most relevant fragments automatically.

This means Maribel gets relationship context injected _before she drafts_, without me telling her to check. The hook handles it.

### The Hooks That Wire It Together
The memory system is not magic. It’s five hooks in ~/.claude/settings.json:

```
"hooks": {
  "UserPromptSubmit": [{
    "hooks": [{
      "type": "command",
      "command": "clawmem hook context-surfacing",
      "timeout": 8
    }]
  }],
  "Stop": [{
    "hooks": [
      { "command": "clawmem hook decision-extractor", "timeout": 30 },
      { "command": "clawmem hook handoff-generator", "timeout": 30 },
      { "command": "clawmem hook feedback-loop", "timeout": 30 }
    ]
  }],
  "SessionEnd": [{
    "hooks": [{ "command": "clawmem update --embed", "timeout": 60 }]
  }],
  "SessionStart": [{
    "hooks": [
      { "command": "clawmem hook postcompact-inject", "timeout": 5 },
      { "command": "clawmem hook curator-nudge", "timeout": 5 }
    ]
  }],
  "PreCompact": [{
    "hooks": [{ "command": "clawmem hook precompact-extract", "timeout": 5 }]
  }]
}
```
UserPromptSubmit fires before every prompt and injects relevant memory as a <vault-context> block. Stop fires when Claude finishes responding — three hooks extract decisions, generate a handoff summary, and process feedback from the conversation. SessionEnd embeds everything into the vector index. PreCompact extracts content before the context window gets compressed.

I never run these manually. They’re just there, running in the background of every session.

For Maribel specifically: the context-surfacing hook fires when I invoke her, surfaces relevant fragments from her collection, and she drafts with that context already loaded. After she finishes, she’s supposed to write her own diary entries before returning — because the Stop hooks from the parent session run in the parent context, not hers.

### What Voice Means When You Encode It
I said Maribel reads damupi-voice.md. That file is a description of how I communicate — patterns I've noticed in my own writing, things I've been told, preferences I'm aware of. I maintain it manually. It's not generated by the system; I edit it when I notice something has drifted.

The question I keep landing on is the same one I ended my Substack post with: is it less authentic if it sounds more like you at your best rather than you in the moment?

My working answer is that the writing is still mine. The voice file is a self-description, not a ghost. Maribel doesn’t invent positions or fabricate relationships — she takes my intent (“tell him no, because Y”) and expresses it in a register appropriate to the relationship. That’s not different in kind from me taking a breath before hitting send. It’s just automated.

The harder question is whether encoding your communication patterns into a system prompt does something to how you think about those patterns. It might. I haven’t been running this long enough to know.

What I do know is that I’ve sent fewer messages I regretted in the last two months. That’s enough to keep iterating.

**Credits**

ClawMem is built by [yoloshii](https://github.com/yoloshii/clawmem) — the tool that makes the memory layer in this piece possible. Worth reading the repo if any of this sparked interest.

The thinking behind why flat markdown files aren’t memory came from Jonathan’s piece [_The Obsidian Memory Delusion: Why Markdown Files Can’t Replace Databases_](https://limitededitionjonathan.substack.com/p/stop-calling-it-memory-the-problem). It’s what pushed me to replace my own file-based memory system with a proper vector store. If you want the argument made more rigorously than I did here, read that first.

_These are my personal opinions. Not my employer’s. I’ve been wrong before — feel free to tell me I’m wrong again._

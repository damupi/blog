---
title: "Know All The Ways The Plumbing Can Be Laid"
date: 2026-08-19
tags: ["AI agents", "Claude Code", "MCP", "automation", "AI engineering"]
category: professional
---

![](/img/2026-08-19-know-all-the-ways-the-plumbing-can-be-laid-1.jpeg)

About a month ago, a friend who's job hunting asked me for advice on building a Job Tracker to keep tabs on his applications.

What's interesting is how differently four people would solve that exact same need: a GitHub repo in TypeScript/React, a Markdown file managed from the terminal with Claude Code, a local agent that designs its own SQLite schema when you just ask it to, or a self-hosted instance on your own server.

All four are "correct." But that's not really the point I want to make.

Working in this field, the real flex isn't picking the right tool for a job tracker. It's understanding enough of the ecosystem that you can shape the tool to whatever the constraints actually are. Two things I built this month show what I mean better than any job tracker would.

The first: a coordinator agent managing two subagents. One is a researcher — queries Exa, scrapes pages, pulls content back for synthesis. The other is an SEO specialist with access to an SEO tool's data, through a CLI extension I built myself that calls that tool's API directly.

The second, built today: a coordinator with three subagents, one per marketing platform. This time, instead of wiring up an MCP connector or installing a CLI on the machine, I put the CLI inside the skill itself — sitting in the script folder. A teammate doesn't configure anything. They just run `claude --agent coordinator` from the folder where the plugin lives, and it works. The whole thing is packaged as a plugin, ready to hand to the team.

Same underlying idea both times — coordinator plus specialized subagents — but the *implementation* decision is different every time: SDK internals, CLI extensions, MCP versus skill-embedded scripts, how you package it so someone else can pick it up without a setup guide.

And that decision isn't only about what's technically possible. It's also about what happens after you ship it. How do you monitor an agent that's calling three different APIs on its own judgment? How do you debug a run when the failure is a bad decision three steps upstream, not a stack trace? And what does this actually cost to run — per call, per teammate, per month — versus the version that's slower but nearly free? Those questions change the answer as much as the architecture does.

None of that is about the fanciest option. It's about knowing all the ways the plumbing can be laid — and what each way costs you in visibility, in debugging time, in actual dollars — so you choose the one that fits the resources you have and the person who has to use it.

That's the real skill in this field. Not "I know how to use an AI agent." It's knowing the systems, the workflows, the tools, and the frameworks well enough that you can deliver the right product with whatever you've got — not the product a tutorial told you to build.

If you build with AI agents: what's the implementation decision you get asked about most — the one people assume is obvious but actually took you the longest to get right?

*These are my personal opinions. Not my employer's. I've been wrong before — feel free to tell me I'm wrong again.*

#AIagents #ClaudeCode #MCP #Automation #AIengineering #BuildInPublic #AItooling

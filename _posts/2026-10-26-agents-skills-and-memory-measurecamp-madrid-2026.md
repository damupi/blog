---
title: "Agents, Skills and Memory: My Daily AI System at MeasureCamp Madrid 2026"
date: 2026-10-26
tags: ["AI agents", "skills", "automation", "Google Tag Manager", "MeasureCamp"]
category: professional
---

![First-time speakers at MeasureCamp Madrid 2026](/img/2026-10-26-measurecamp-madrid-agents-skills-memory-1.jpg)

This weekend I gave my first session at [**MeasureCamp Spain – Madrid 2026**](https://spain.measurecamp.org/).

The session, *Agents, Skills and Memory: My Daily AI System*, received the **Best Session – From Data to Business Impact** award, sponsored by [JAKALA](https://www.jakala.com/).

The award was a lovely surprise. What stayed with me even more was that the conversation continued after the scheduled end. People wanted to understand what was behind the demonstration and how the system worked outside a conference room.

So I am sharing the slides and the supporting materials.

## The task, not the architecture

I did not want to spend thirty minutes giving people a tour of agent definitions and configuration files.

Instead, the session followed one anonymised analytics task from beginning to end:

1. An ambiguous tracking ticket arrives.
2. A triage agent turns it into verifiable acceptance criteria.
3. A specialised GTM agent prepares the implementation in an isolated workspace.
4. A human reviews the diff and remains responsible for publishing.
5. New evidence arrives, and the system investigates it before proposing a response.

Only after completing the task did I reveal the layers behind it:

- **Agents** define who is responsible for each part of the work.
- **Skills** contain the procedures, conventions and safety rules.
- **Tools** provide controlled access to the systems where work happens.
- **Memory** preserves reusable decisions and findings between sessions.
- **The human** approves sensitive actions and remains responsible for the outcome.

The point was not that an agent could edit Google Tag Manager.

The point was that it knew its responsibility, had the procedure and tools required to complete it, and knew where it had to stop.

> **Automate the work. Do not delegate responsibility.**

## The slides

The presentation is in Spanish, as delivered at MeasureCamp Madrid.

<div class="slide-deck">
  <iframe src="/assets/slides/measurecamp-madrid-2026-agents-skills-memory.pdf#view=FitH" title="Agents, Skills and Memory – MeasureCamp Madrid 2026 slides" loading="lazy"></iframe>
</div>

[Download the slides as PDF](/assets/slides/measurecamp-madrid-2026-agents-skills-memory.pdf) · [Download the original PowerPoint](/assets/slides/measurecamp-madrid-2026-agents-skills-memory.pptx)

## The materials behind the session

I have also published a companion repository containing:

- the slides;
- an offline replay of the demonstration;
- the fictional ticket and agent outputs;
- the simplified system canvas;
- the local dataLayer simulator used to explain the final debugging step.

[Explore the MeasureCamp Madrid 2026 companion repository](https://github.com/damupi/measurecamp-madrid-2026-ai-setup).

Everything in the public package has been reviewed and sanitised. It contains no company identifiers, production data, credentials or links to private systems.

## The CLIs underneath the agents

The agents are useful because they operate through tools with clear, inspectable interfaces. Three of the main command-line projects behind my daily setup are:

- [`gafour`](https://github.com/damupi/gafour) for GA4 reporting and metadata inspection.
- [`gtm-cli`](https://github.com/damupi/gtm-cli) for Google Tag Manager administration and controlled workspace workflows.
- [`gaad-cli`](https://github.com/damupi/gaad-cli) for administering GA4 properties through the Admin API.

A CLI alone is not an agent. The agent adds responsibility and limits. The skill adds the procedure. Memory adds reusable context. The CLI provides the deterministic interface through which the work is inspected and performed.

That separation is what makes the system useful to me every day. It is also what makes it possible to stop before publication, ask for human approval and explain exactly what changed.

You do not need twenty-seven agents to begin. Start with one repetitive task and define six things:

- What result is the specialist responsible for?
- What procedure does it need?
- Which tools can it access?
- What must it never do?
- When must it return control to a human?
- What is worth remembering for next time?

**What repetitive part of your work would you turn into a specialist first?**

---

*These are my personal opinions. Not my employer's. I've been wrong before – feel free to tell me I'm wrong again.*

---
title: "A Markdown File Is Not an AI Workflow"
date: 2026-10-13
tags: ["AI workflows", "AI agents", "LLMOps", "multi-agent systems"]
category: professional
---

![](/img/2026-10-13-markdown-file-not-ai-workflow.png)

**A Markdown file is not an AI workflow.**

I learned that after trying to coordinate a project shared by several humans and their AI agents.

Until then, most of my agentic systems had been built from beginning to end by me. I could write the instructions, watch the agents work and intervene when they misunderstood something. An `AGENTS.md` pointing to a detailed architecture document was usually enough.

Then I joined a project where I was no longer the only person making decisions, and my agents were not the only agents touching the repository.

**The system fell apart.**

Not because the documentation was missing. The documentation was there. The problem was that **documents can describe an architecture, but they cannot enforce one.**

The project was a form of data self-service. That is all the product context you need.

The interesting part was how it had to be built.

I had built MCP servers, skills and plugins for other people to use. This time, several people and their agents were changing the solution itself.

Other people had different requirements, different constraints and an equal say in the decisions. Once the infrastructure was in place, I captured the architecture in a Markdown document.

One document. Everything explained. What could possibly go wrong?

That had worked when I was building alone.

When one person supervises the process, documentation can feel like a control system. You notice the implementation drifting, stop the agent and correct it. Fine. *No problemo.* Here's your reviewer agent.

In a shared repository, that invisible supervision disappears.

One person asks an agent to optimise for speed. Another requests a new feature. A third agent extends a pattern that was supposed to be temporary. Every change looks reasonable in isolation. Put them together and suddenly you are looking at a completely different system.

The agents did what LLMs do: they interpreted the available context and tried to complete the task in front of them.

*The humans helped them make a mess of it.*

Around that time, I found [an article by Aina Lluna](https://ainalluna.substack.com/p/crear-sistema-diseno-codex-agentes-ia) about building a design system with Codex. Her combination of persistent documentation, reusable components, skills and specialised agents changed how I saw the problem.

Until then, I had treated LLM interpretation and deterministic code as mutually exclusive choices.

*Either tell the LLM to run a script, or let the LLM do the work.*

*Either build a flexible agent, or build a rigid workflow.*

**That was the wrong distinction.**

The useful boundary is between **the parts of a system that benefit from interpretation and the parts that should never be left open to it.**

An LLM is useful when the input is ambiguous, context has to be gathered or the next step requires judgement. A script is useful when an operation should behave the same way every time. A schema defines valid data. A test checks whether the implementation still honours the contract.

**They are not competing approaches. They are different layers of the same workflow.**

Obvious in hindsight. It was not obvious to me while I was trying to choose one.

The architecture is now expressed through more than prose:

- **Documents** explain the intent and provide context.
- **JSON schemas** define the structures the system accepts.
- **Python scripts** handle deterministic operations.
- **Tests** verify behaviour before changes are accepted.
- **Skills** package those elements into repeatable workflows.
- **Specialised subagents** execute only the workflows that belong to their role.

The Markdown files still matter. Context matters more than ever when agents are involved. But **context alone is not enforcement.**

A document can ask an agent to follow a structure. A schema can reject an invalid output. Documentation can request checks; a workflow can make them unavoidable. Instructions can describe an agent's limits; the harness can enforce them.

As I wrote before, [an LLM does not follow your workflow just because you documented it](https://blog.damupi.com/2026/08/09/an-llm-doesnt-follow-your-workflow-it-tries-to-survive-it/). If skipping a step appears to be the easiest route to its objective, it may skip it.

The solution is not to remove the LLM. **It is to stop asking the LLM to be the entire system.** Poor thing already has enough to do.

I had been thinking in exclusions: the model or the script, flexibility or control, autonomy or guardrails.

The answer was **the model, the script, the schema, the test and the human** – each doing the part they are actually good at.

That human part still matters. Building with AI in a team means accounting for different requirements, abilities and ways of working. Every person brings their own judgement and, increasingly, their own agents. A good workflow has to coordinate all of them without assuming they will interpret the same document in the same way.

> **Documentation describes intent. A workflow turns that intent into paths, contracts, checks and boundaries.**

So when you design an AI workflow, **which parts genuinely need the model's judgement – and which parts should never depend on interpretation at all?**

---

*These are my personal opinions. Not my employer's. I've been wrong before – feel free to tell me I'm wrong again.*

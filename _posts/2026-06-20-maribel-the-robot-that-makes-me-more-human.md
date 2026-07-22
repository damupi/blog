---
title: "Maribel - The Robot That Makes Me More Human"
date: 2026-06-20
tags: ["AI", "Claude", "RAG", "TechCulture"]
category: professional
source: substack
source_url: "https://damupi256603.substack.com/p/maribel-the-robot-that-makes-me-more"
---

![](/img/2026-06-20-maribel-1.jpg)

What if the most authentic version of a message is the one you meant to write? What if a robot helps you get there? Is it less authentic?

I’ve been living that question for the past few months. Her name is Maribel.

Maribel is my internal communications agent—a Claude subagent I built to help me write better messages to my colleagues. The name comes from the best office manager I ever worked with, years ago. She had this rare ability to make anyone feel heard, even when delivering bad news. I always thought it was an innate gift. Turns out, with the right prompting, it’s also a skill you can borrow.

The idea came from an unlikely place. I was watching videos about writing for social media—how LinkedIn needs a different voice than Twitter, how Substack needs a different register than both—and something clicked. If you can tune your voice to a platform, why not to a person? Or more precisely, to their position in the organization?

So, I exported our company org chart into a static Markdown file—who reports to whom, which team, what level—and built five tone registers on top of it. Each one is a prompt file, calibrated to a specific reporting relationship:

Upward: For writing to a manager or skip-level.

Senior Leadership: Shorter, no warmup, one clear ask.

Peers: Same-team colleagues.

Technical: For engineering peers who share context and don’t need a preamble.

Cross-team: Conversations that require broader framing.

Maribel reads the org chart, identifies the relationship, loads the right file, and only then starts drafting. She doesn’t improvise the tone; she reads it from a file I control. That distinction matters to me. If I want to adjust how she writes to my manager, I change the file, not the core prompt.

But tone alone isn’t enough. Before drafting, she also reads the last ten or fifteen messages in that thread to understand where things actually stand. Not to summarize them, just to calibrate. If the thread has been going for a while and the other person has already dropped the formalities, Maribel picks up on that. She won’t open with a rigid greeting mid-conversation. She won’t reset the register when the conversation has already found its own rhythm.

But here is the problem I didn’t fully anticipate: she started every session knowing nothing about the human on the other end.

You could ask her to draft a message to someone you’ve been working with for two years, and she would have no idea who they are. No idea how they write, whether they prefer Slack or email, or that the last time you sent them something overly formal it landed badly. Every session, back to zero.

That’s where ClawMem comes in.

It’s a RAG (Retrieval-Augmented Generation) system, which is a fancy way of saying: a memory layer that stores what it learns and pulls back the most relevant pieces when they’re needed. After every meaningful interaction, Maribel writes a short diary entry—a concrete observation about the person she just helped me message.

She captures nuances like: this person prefers bullet points over paragraphs in async updates, or responds within the hour on Slack but tends to ignore longer emails, or writes in short bursts and uses a thumbs-up to close threads—silence means it’s fine. These aren’t notes I write. Maribel writes them herself before ending the session.

At the start of every new conversation, the most relevant fragments from past sessions are automatically injected into her context. So when I tell her “reply to my manager’s message about a Q2 deliverable,” she doesn’t start from scratch. She already knows their communication style, the channel they prefer, and the history of our working relationship.

The org chart tells her the structure. The tone files tell her the register. The memory tells her the person.

To be honest, the system still falls short in places. She doesn’t proactively propose replies; I still have to initiate every interaction. And the memory is only as good as what she’s observed so far; for people I don’t message often, she’s still writing cold.

But something unexpected happened once the memory started accumulating. The drafts got noticeably better. Less generic. More situationally aware. Occasionally, she’d catch something I would have missed—a subtle shift in tone from someone’s recent messages, a detail suggesting this wasn’t a great moment to push on a deadline.

Which brings me back to the authenticity question I keep sitting with. Is a message less authentic if an AI wrote it, but it sounds more like you at your best, rather than you in a stressed, rushed moment?

That question gets heavier when you add memory. It’s one thing to say the AI wrote a message that sounds like me. It’s another to say the AI wrote it having accumulated months of context on this specific relationship.

I’m still figuring out what to call that. But “less authentic” doesn’t feel right.

Do you want to know more details? The system prompt? The hooks in the harness system? Please leave a comment and I’d be happy to share.

Clawmem github repository

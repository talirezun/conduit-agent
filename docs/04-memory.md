# Memory

A Conduit agent has two kinds of memory, and they do different jobs. Together they're the difference between an assistant that forgets everything between chats and one that *compounds* — getting more useful the longer you work with it.

| | Short-term | Long-term |
|---|---|---|
| **What** | `memory.md` — one small file | [The Curator](05-the-curator.md) — a local knowledge app |
| **Scope** | This one agent / project | Everything you've ever added, across projects |
| **Holds** | "Where did we leave off?" | Documents, notes, research, a knowledge graph |
| **Updated** | Automatically, every session | When you ingest sources |
| **Required?** | Yes (created automatically) | Recommended |

---

## Short-term memory — `memory.md`

This is a small file in your agent's folder. The agent **reads it at the start of every session** and **updates it at the end**. It's how the agent remembers what happened last time.

A typical `memory.md`:

```
Last session: 2026-06-21
Last action: Drafted the new onboarding checklist
Current status: Day 14 of 30 — 3 tasks done, 1 awaiting review
Unresolved items: Waiting on reviewer feedback for the checklist
Notable events: Reviewer asked for a simpler format
Next planned action: Revise checklist, then measure onboarding time
```

You never write this by hand. The [Setup prompt](../templates/prompts/setup.md) creates it, and the agent maintains it. The rule it follows: **append history, never delete it.** Your `memory.md` is an honest log of the project.

> A starter template is at [templates/memory.md](../templates/memory.md), but the agent creates this for you during setup.

---

## Long-term memory — The Curator

Short-term memory remembers *this project*. Long-term memory remembers *everything you know*.

[The Curator](https://github.com/talirezun/the-curator) is an open-source app that runs on your computer. You feed it documents, notes, and research; it organizes them into a **knowledge graph** — a web of connected ideas — that your agent can query at any time. Knowledge is split into **domains** (e.g. one for your personal knowledge, one for your company), and it **compounds**: the more you add, the more context your agent has to ground its work in *what you actually know*, instead of guessing.

This is what turns a generic assistant into *your* agent.

→ Full guide, including the prompt-based install: **[The Curator →](05-the-curator.md)**

---

## When the agent uses each

- **Start of session:** read `memory.md` → "here's where we are."
- **Before a new phase or a research question:** query The Curator → "here's what we already know about this."
- **End of session:** update `memory.md` → "here's where we're leaving off."

You don't manage any of this. You just work, and the agent keeps both memories current.

---

Next: [The Curator →](05-the-curator.md)

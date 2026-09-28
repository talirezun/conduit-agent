# Use case: Atlas — Professional Assistant Agent

*A general-purpose execution assistant for professionals — Conduit Use Case #2*

This is **Use Case #2** for the [Conduit framework](../../README.md): a configuration any working professional — CEO, founder, manager, consultant, or individual contributor — can use to build a personal AI **execution assistant**. Atlas captures and tracks tasks, turns meetings into action items, researches on demand, reads documents, keeps a compounding second brain, and prepares reports and drafts — all inside a clear mandate, and all driven by copy-paste prompts (no code, no terminal).

**It is also a coach.** Atlas explains each step as it builds itself, so a non-technical professional finishes the setup understanding what an agent harness, an MCP, a second brain and context management actually are — because they watched their own agent build each one. That makes Atlas a practical, hands-on exercise for executive education as much as a daily tool.

> New to Conduit? Read [docs/00-start-here.md](../../docs/00-start-here.md) and [the concepts](../../docs/01-concepts.md) first. This page assumes the basics and focuses on what's *specific* to Atlas.

---

## What's in this folder

| File | What it is |
|---|---|
| **[AGENTS.md](AGENTS.md)** | The Atlas config — copy this into your project folder. |
| **[Atlas_Professional_Assistant_Guide.md](Atlas_Professional_Assistant_Guide.md)** | The complete, prompt-driven setup & operations guide. |
| **[scenarios/](scenarios/)** | Ready-to-run scenario prompts (Basic + Advanced) if you don't have your own workflow in mind yet. |

---

## What makes this use case distinct

Where [Use Case #1 (Cotrugli Business School)](../cotrugli-business-school/) is an opinionated, governed **30-day project agent** with a required ledger and a reviewer, Atlas is the opposite end of the spectrum: a **flexible, agnostic assistant** for everyday professional work.

- **No lifecycle, no ledger, no reviewer.** Atlas isn't running a graded project — it's helping you get through your week.
- **opencode-only, prompt-driven.** Everything installs by prompt; the agent builds itself. Free and open-source, with local-model (Ollama) support for maximum privacy.
- **Coach-led.** Coach Mode (on by default) explains every step in plain language, does all the technical work itself, and hands back only the clicks that are genuinely yours — following a five-level learning path.
- **A modular tool stack** you turn on as needed: a context layer / second brain (The Curator), documents (Word, PowerPoint, Excel, PDF), web search, spreadsheets, a read-only calendar, local meeting transcription, and email — every one free, and none needing an account to start except the ones that reach your own mail or calendar.
- **Privacy-first by construction.** Files and your second brain stay on your machine; document and search tools need no account; meeting transcription is 100 % on-device; anything that leaves your machine is named, opt-in, and confirmed.

---

## The stack

| Layer | Tool | Status |
|---|---|---|
| 🗂️ Tasks | `tasks.md` in your folder | Always on |
| 🧠 Memory & Context | `memory.md` (working memory) + [The Curator](https://github.com/talirezun/the-curator) (context layer): Mac app or browser app, `my-curator` MCP + its two official skills | Always on |
| 📄 Documents | [MarkItDown](https://github.com/microsoft/markitdown) MCP — Word, PowerPoint, Excel, PDF | Recommended |
| 🔎 Research | Web search MCP (no key) + opencode's built-in page reader | Recommended |
| 📊 Data | Excel MCP (`haris-musa/excel-mcp-server`) | Recommended |
| 📅 Calendar | Apple Calendar · Microsoft 365 (read-only) · private iCal feed | Optional |
| 🎙️ Meetings | [OpenWhispr](https://github.com/OpenWhispr/openwhispr) (local transcription) | Optional |
| 📧 Communication | Atomic Mail (agent mailbox) · Gmail/Outlook (your real inbox) | Optional |

---

## The learning path

| Level | You learn | By doing |
|---|---|---|
| 1 · Your agent | What an agent harness is; the config as mandate | Install opencode, run the Setup and Profile prompts |
| 2 · Tools (MCPs) | What an MCP is | Give Atlas documents, web search and spreadsheets |
| 3 · Your second brain | How your own context grounds — and compounds — an agent's work | Install The Curator, ingest, connect it |
| 4 · A shared brain *(optional)* | How a team builds one brain together | Join a Shared Brain |
| 5 · Context management *(optional)* | How any agent can continue your work | Connect Atlas to a Curator project |

---

## How to run it

The full walkthrough is in the [guide](Atlas_Professional_Assistant_Guide.md). In short:

1. **Install opencode** (free) and pick a model.
2. **Make a folder**, drop in [AGENTS.md](AGENTS.md).
3. **Setup prompt** → Atlas builds its folders and memory, and starts coaching you.
4. **Fill My Profile prompt** → makes Atlas yours (role, focus, mandate).
5. **Level 2 — tools by prompt**: Documents, Web search, Excel.
6. **Level 3 — The Curator**: Mac app (download) or browser app (by prompt) → first run in the app → one prompt connects the MCP and installs the skills.
7. **Levels 4–5 (optional)**: join a Shared Brain; connect Atlas to a Curator project.
8. **Optional modules**: calendar, meeting transcription, email.
9. **Run it** with the [scenario prompts](scenarios/README.md). Lost at any point? Paste **`coach me`**.

Already running Atlas v1? The guide has a one-prompt [upgrade](Atlas_Professional_Assistant_Guide.md#75-already-running-atlas-v1-upgrade).

---

## After you build it

Atlas stays with you — it's a tool for your work, not a demo. Keep using it, add MCPs with the generic pattern, and grow your Curator domains. When you no longer need the explanations, say *"coach mode off"*. If you build a great configuration for a specific profession or workflow, consider contributing it back as a new [use case](../README.md).

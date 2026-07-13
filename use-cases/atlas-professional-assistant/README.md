# Use case: Atlas — Professional Assistant Agent

*A general-purpose execution assistant for professionals — Conduit Use Case #2*

This is **Use Case #2** for the [Conduit framework](../../README.md): a configuration any working professional — CEO, founder, manager, consultant, or individual contributor — can use to build a personal AI **execution assistant**. Atlas captures and tracks tasks, turns meetings into action items, researches on demand, reads documents, keeps a compounding second brain, and prepares reports and drafts — all inside a clear mandate, and all driven by copy-paste prompts (no code, no terminal).

> New to Conduit? Read [docs/00-start-here.md](../../docs/00-start-here.md) and [the concepts](../../docs/01-concepts.md) first. This page assumes the basics and focuses on what's *specific* to Atlas.

---

## What's in this folder

| File | What it is |
|---|---|
| **[AGENTS.md](AGENTS.md)** | The Atlas config — copy this into your project folder. |
| **[Atlas_Professional_Assistant_Guide.md](Atlas_Professional_Assistant_Guide.md)** | The complete, prompt-driven setup & operations guide. |
| **[curator-skill/](curator-skill/)** | opencode-adapted copy of The Curator's usage skill — the Curator install prompt downloads this into your project. |
| **[scenarios/](scenarios/)** | Ready-to-run scenario prompts (Basic + Advanced) if you don't have your own workflow in mind yet. |

---

## What makes this use case distinct

Where [Use Case #1 (Cotrugli Business School)](../cotrugli-business-school/) is an opinionated, governed **30-day project agent** with a required ledger and a reviewer, Atlas is the opposite end of the spectrum: a **flexible, agnostic assistant** for everyday professional work.

- **No lifecycle, no ledger, no reviewer.** Atlas isn't running a graded project — it's helping you get through your week.
- **opencode-only, prompt-driven.** Everything installs by prompt; the agent builds itself. Free and open-source, with local-model (Ollama) support for maximum privacy.
- **A modular tool stack** you turn on as needed: a second brain (Curator), spreadsheets (Excel), web research, PDF reading, local meeting transcription, and email.
- **Privacy-first by construction.** Files stay local; web/PDF tools are no-auth; meeting transcription is 100 % on-device; anything that leaves your machine is opt-in and confirmed.

---

## The stack

| Layer | Tool | Status |
|---|---|---|
| 🗂️ Tasks | `tasks.md` in your folder | Always on |
| 🧠 Memory | `memory.md` + [My Curator](https://github.com/talirezun/the-curator) MCP | Always on |
| 📊 Data | Excel MCP (`haris-musa/excel-mcp-server`) | Recommended |
| 🔎 Research | Web Fetch MCP + PDF MCP (both no-auth) | Recommended |
| 🎙️ Meetings | [OpenWhispr](https://github.com/OpenWhispr/openwhispr) (local transcription) | Optional |
| 📧 Communication | Atomic Mail (agent mailbox) · Gmail/Outlook (your real inbox) | Optional |

---

## How to run it

The full walkthrough is in the [guide](Atlas_Professional_Assistant_Guide.md). In short:

1. **Install opencode** (free) and pick a free or local (Ollama) model.
2. **Make a folder**, drop in [AGENTS.md](AGENTS.md).
3. **Setup prompt** → builds folders, memory, tasks.
4. **Fill My Profile prompt** → makes Atlas yours (role, focus, mandate).
5. **Install tools by prompt** — Curator (two-step), Excel, Web Research, PDF.
6. **Optional modules** — meeting transcription, email.
7. **Run it** with the [scenario prompts](scenarios/README.md): capture tasks, process meetings, brief before calls, read documents, weekly digests.

---

## After you build it

Atlas stays with you — it's a tool for your work, not a demo. Keep using it, add MCPs with the generic pattern, and grow your Curator domains. If you build a great configuration for a specific profession or workflow, consider contributing it back as a new [use case](../README.md).

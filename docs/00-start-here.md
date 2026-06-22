# Start here

This page gets you from *nothing* to *a working agent* in about 15 minutes, without writing code. If a word is unfamiliar, check the [glossary](glossary.md) — but you can follow this without knowing any of it.

> **The one rule of Conduit:** you never type terminal commands. You paste **prompts**, and your agent does the technical work for you.

---

## Step 1 — Pick how you'll run it

Your agent needs a *harness* — an app that can read files and do setup on your behalf. Pick one:

| If you want… | Use | Cost |
|---|---|---|
| To start free, no accounts | **opencode** + a built-in free model | Free |
| More model choice, still free | **opencode** + an [OpenRouter](https://openrouter.ai) free model | Free (free account) |
| Fully private, offline | **opencode** + a **local model** | Free |
| The best quality | **Claude Cowork** or **Claude Code** | ~$20/mo |

Full walkthrough with install links: **[Choose your runtime →](02-choose-your-runtime.md)**

Install your chosen app, then come back.

---

## Step 2 — Make a folder for your agent

Create one folder anywhere on your computer. Name it for the agent, e.g. `my-agent/` or `sales-agent/`. One folder = one agent. (You can make more later.)

Copy the template config into it:

- Download [`templates/AGENTS.md`](../templates/AGENTS.md) and place it in your folder as `AGENTS.md`.
- *Using Claude Cowork or Claude Code?* Rename it to `CLAUDE.md`.

You don't create any other files or subfolders — the agent does that in the next step.

---

## Step 3 — Open the folder in your agent and run setup

Open your folder in opencode (or point Claude Cowork at it). Then paste the **Setup prompt**:

➡️ **[templates/prompts/setup.md](../templates/prompts/setup.md)**

The agent will create its folders, create its memory file, check for and install any tools it needs, and confirm what it did. It explains every step in plain language.

---

## Step 4 — Tell the agent who it is

Paste the **Fill-my-config prompt**:

➡️ **[templates/prompts/fill-config.md](../templates/prompts/fill-config.md)**

The agent interviews you one question at a time — what's your goal, what may it do, what must it never do — and writes your answers into `AGENTS.md`. Confirm when it shows you the result.

**You now have a working agent.** Talk to it in plain language.

---

## Step 5 (optional) — Give it superpowers

Add these only if you want them, each by pasting one prompt:

| Capability | What it adds | Guide |
|---|---|---|
| 🧠 **The Curator** | Long-term memory — a compounding second brain | [docs/05](05-the-curator.md) |
| 📊 **Spreadsheets** | Read/write Excel (opencode only; Claude does it natively) | [docs/06](06-mcps.md) |
| 📧 **Email** | An agent-owned mailbox | [docs/06](06-mcps.md) |
| 🧾 **Audit ledger** | A tamper-evident record of what the agent did | [docs/07](07-governance-ledger.md) |

---

## What next

- Understand the model you just built → [The concepts](01-concepts.md)
- See a complete real example → [Cotrugli Business School use case](../use-cases/cotrugli-business-school/)
- Adapt it to your own workflow → [Build your own use case](../use-cases/README.md)

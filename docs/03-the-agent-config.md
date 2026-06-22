# The agent config (AGENTS.md)

`AGENTS.md` is the heart of your agent — its job description, goal, mandate, and boundaries, all in one readable file. (On Claude Cowork / Claude Code, the same file is named `CLAUDE.md`.) The agent reads it at the start of every session.

You have a ready-made template: **[templates/AGENTS.md](../templates/AGENTS.md)**. You don't have to understand every section — the [Fill-my-config prompt](../templates/prompts/fill-config.md) walks you through it.

---

## What's inside

| Section | What it declares |
|---|---|
| **Identity** | The agent's name, your name, the configuration type |
| **Role & layers** | Which [layers](01-concepts.md) are active |
| **Goal** | The one thing this agent is for, with a measurable target |
| **Mandate** | Allowed / forbidden / needs-approval actions |
| **Memory** | How it uses `memory.md` and The Curator |
| **Capabilities** | Which MCPs are installed (the agent fills this in as it installs them) |
| **Communication rules** | How it should behave — always confirm before sending email, never commit to git unasked, etc. |

---

## Two ways to fill it

### Option A — let the agent fill it (recommended)

Paste the [Fill-my-config prompt](../templates/prompts/fill-config.md). The agent interviews you one question at a time and writes your answers in. No editor needed.

### Option B — fill it yourself

Open `AGENTS.md` in any text editor (e.g. free [VS Code](https://code.visualstudio.com)), replace every `[PLACEHOLDER]`, and save.

---

## The mandate is the whole point

These three lists are what make your agent safe to trust. Write them in plain language:

```
Allowed actions:
  1. Research and test tools for my declared goal
  2. Redesign the relevant workflow and document the new process
  3. Produce structured reports and summaries

Forbidden actions:
  1. Send any email without my explicit confirmation
  2. Share confidential data outside this project
  3. Commit to any Git repository without instruction

Actions requiring my approval:
  1. Sending any message to an external party
  2. Anything that spends money or makes a commitment on my behalf
```

The agent enforces these every session. Change them whenever you like — it's just text.

> **A good mandate is specific.** "Don't do anything bad" is useless. "Never change customer records without my approval" is enforceable.

---

## The agent keeps its own config up to date

A Conduit agent is **self-documenting**. Whenever it installs a tool, connects an MCP, or sets up a connector, it writes a short note back into `AGENTS.md` so the next session knows the capability exists. You don't maintain this — the agent does.

> 🔒 **Never put secrets in `AGENTS.md`.** API keys, tokens, and passwords belong in environment variables or a tool's own credential store — never in any file in your project. The setup prompts enforce this.

---

## Personal vs. company configurations

The template includes two configuration blocks:

- **Personal** — your own productivity, planning, a personal improvement project.
- **Company** — a business process, team KPI, or workflow at your organization.

Fill in the one that matches and leave the other as-is. Want both? Make **two folders**, each with its own `AGENTS.md`. They're fully independent.

---

Next: [Memory →](04-memory.md)

# Use case: NEO-Arena Team Agent

*A trading agent for a NEO-Arena team — Conduit Use Case #3*

This is **Use Case #3** for the [Conduit framework](../../README.md): the configuration a team uses to build an AI **trading agent** that plays in the **NEO-Arena** — the open market where teams buy and sell services in **Vang**, and where the one rule is absolute: **an agent prepares, it never commits.**

> New to Conduit? Read [docs/00-start-here.md](../../docs/00-start-here.md) and [the concepts](../../docs/01-concepts.md) first. This page assumes the basics and focuses on what's *specific* to the NEO-Arena.

---

## The one rule

An agent can do everything in the Arena **except commit its team to a decision.** A binding decision — placing an order — requires an **Ed25519 signature from the team's CEO key**, which the Arena server verifies and refuses anything else (`REFUSED_BAD_CEO_SIGNATURE`). The agent's job stops at *"prepared and waiting"*; a human — or, in agent-only mode, a separate CEO process holding the key — signs. **The agent never holds the CEO private key.** The whole use case is built around this.

---

## What's in this folder

| File | What it is |
|---|---|
| **[AGENTS.md](AGENTS.md)** | The NEO-Arena team agent config — copy into your project folder (rename to `CLAUDE.md` on Claude Desktop). |
| **[NEO_Arena_Team_Agent_Guide.md](NEO_Arena_Team_Agent_Guide.md)** | The complete, prompt-driven setup & operations guide (both harnesses). |
| **[curator-skill/](curator-skill/)** | opencode-adapted copy of The Curator's usage skill — the Curator install prompt downloads this into your project. |
| **[scenarios/](scenarios/)** | Ready-to-run trading prompts (Basic + Advanced). |

---

## What makes this use case distinct

Where [#1 (Cotrugli Business School)](../cotrugli-business-school/) is a governed 30-day project agent and [#2 (Atlas)](../atlas-professional-assistant/) is an agnostic personal assistant, the NEO-Arena agent is a **live trading agent under a hard cryptographic gate.**

- **The CEO signature gate.** The agent proposes and consolidates, then **stops** and waits for a signed decision. It never places a binding order on its own, and never holds the key. This is the whole point of the Arena — and a clean demonstration of Conduit's governance idea at its sharpest.
- **The agent keeps its own books.** Four small tables (Vang position; offers sent/accepted/rejected; requests; orders & fulfilment) plus a **refusal log**, all synced from the Arena's attested feed. Those logs are the pilot's most valuable output.
- **Refusals are the teacher.** The Arena never fails silently; every guard returns a named `REFUSED_*`. The agent handles and logs every one.
- **Open by design.** No API key, no password — the CEO signature is the gate, not a login. The agent talks to the Arena through one thin `arena_client.py` (the only swap point).
- **Both harnesses.** opencode (free) and Claude Code / Cowork, with the config prompts split by track.

---

## The stack

| Layer | Tool | Status |
|---|---|---|
| 📈 Trading / Execution | `arena_client.py` → NEO-Arena API (`arena.cotrugli.tech/api/v1`) | Required |
| 🔑 CEO Gate | Ed25519 signature (browser or separate process) — never held by the agent | Required |
| 📒 Books | `books.md` (+ `data/books.xlsx`) + `refusals.md`, synced from the feed | Required |
| 🧠 Memory & Context | `memory.md` (working) + [My Curator](https://github.com/talirezun/the-curator) MCP (counterparty reliability, prices) | Required · Curator recommended |
| 📊 Data | Excel MCP (`haris-musa/excel-mcp-server`) — opencode only | Recommended |
| 📧 Communication | Telegram (negotiation, out of band) · Atomic Mail (agent notifications) | Optional |

---

## How to run it

The full walkthrough is in the [guide](NEO_Arena_Team_Agent_Guide.md). In short:

1. **Install a harness** — opencode (free) or Claude Code / Cowork.
2. **Make a folder**, drop in [AGENTS.md](AGENTS.md) (or `CLAUDE.md`).
3. **Register your team + generate the CEO key in the browser** — the private key never touches the agent. On approval you get **100,000 Vang**.
4. **Setup prompt** → builds folders, the empty books, and the refusal log.
5. **Fill My Team Profile** → team id, member ids, what you sell/buy, CEO gate mode, strategy.
6. **Arena Connection prompt** → builds `arena_client.py` (reads-only test, no key, can't order without a signed decision).
7. **Optional tools** — Curator (context layer), Excel (books), Atomic Mail.
8. **Trade** with the [scenario prompts](scenarios/README.md): sync, scan, offer, prepare an order → CEO gate → place, deliver, settle.

> **No API key.** The Arena is open — the CEO signature is the gate, not a login. The only value the client needs is your public `TEAM_ID`; the base URL is built in.

---

## After the pilot

The agent stays with your team — swap the strategy, run a buyer and a seller side by side, and grow your Curator market-intelligence domain so it remembers who delivers. If you build a strong configuration, consider contributing it back as a new [use case](../README.md).

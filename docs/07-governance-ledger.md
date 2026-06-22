# Governance & the audit ledger

This is what makes a Conduit agent *accountable* — not just capable. There are two parts: **governance** (boundaries the agent enforces) and the **ledger** (an optional tamper-evident record of what it did).

> This is the layer that matters most in regulated or high-trust settings. It's **optional** in the universal framework — but a use case can make it required. The [Cotrugli Business School use case](../use-cases/cotrugli-business-school/) does exactly that.

---

## Part 1 — Governance: the config is the mandate

You already met this in [the agent config](03-the-agent-config.md). The three lists in `AGENTS.md` —

- **Allowed actions**
- **Forbidden actions**
- **Actions requiring approval**

— *are* the agent's governance rules. The agent reads them every session and stays inside them. When you ask for something forbidden, it declines and records the attempt rather than quietly doing it.

This maps cleanly onto formal governance frameworks:

| In your `AGENTS.md` | Governance equivalent |
|---|---|
| Allowed actions | Mandate scope |
| Forbidden actions | Hard policy blocks |
| Actions requiring approval | Human-in-the-loop / co-sign |
| Reviewer | Co-signer / accountability reference |

The point: **your boundaries are written in plain language, in one file, and you control them.**

---

## Part 2 — The ledger: a tamper-evident record

A ledger records **fingerprints (hashes)** of what your agent did — proof that an event happened and hasn't been altered — *without storing the content itself.*

### What gets sent

Only a **hash** of each event — never the raw content. A hash is a one-way fingerprint: it proves "this exact thing happened" but can't be reversed to reveal what the thing was. **No business data leaves your computer in readable form.** This is privacy-correct by design.

### Why you'd want it

- **Trust:** an outside party can verify your agent's trail is genuine and unaltered.
- **Compliance:** an auditable record aligns with emerging AI governance expectations (e.g. the EU AI Act).
- **Honesty:** the record is append-only — nothing is quietly deleted or rewritten.

---

## The connector pattern — one thin file

All communication with a ledger goes through a **single file** with **one function** — for example `ledger_connector.py` exposing `send_agent_event()`. This is the only thing that ever talks to the backend.

```
┌─────────────────────┐     calls      ┌──────────────────────┐    posts    ┌──────────────┐
│     Your agent      │ ─────────────▶ │  ledger_connector.py │ ──────────▶ │   A ledger   │
│  (never changes)    │                │  send_agent_event()  │             │   backend    │
│  builds hashes only │                │  the ONLY swap point │             │  (swappable) │
└─────────────────────┘                └──────────────────────┘             └──────────────┘
```

Why this matters: **the agent never knows about the backend.** If you switch ledgers, you change *one file* — the agent is untouched. The agent builds this connector for you during setup, from a prompt.

### Three rules a good connector respects

1. **One swap point.** Keep all backend logic in the single connector file.
2. **Routing ≠ identity.** Infrastructure routing (which tenant/endpoint) stays *out* of the hashed content. *Semantic* identity (who the principal is) goes *into* it. Don't mix them.
3. **The mandate is the governance mandate.** Forbidden and needs-approval actions map to the approval/co-sign level. An action that needed approval is only marked "human-approved" *after you confirm.*

> 🔒 **Never send raw content to a ledger, and never confirm a submission silently.** The connector sends hashes only, and the agent always asks before submitting — these records are immutable.

---

## A concrete example

The [Cotrugli Business School use case](../use-cases/cotrugli-business-school/) implements this fully: a 30-day project lifecycle (commitment → fulfillment → acceptance → dispute → correction → resolution), each stage hashed and sent to the **Cotrugli Ledger** via exactly this connector pattern. See that use case for the working prompts, record templates, and endpoint details.

---

## Do you need a ledger?

| If your agent… | Ledger? |
|---|---|
| Is a personal productivity helper | Probably not |
| Operates in a regulated or audited context | Yes |
| Acts on behalf of a company or clients | Strongly recommended |
| Is part of the Cotrugli lab | Required (use case rule) |

You can always add it later — it's one prompt.

---

Back to: [Concepts](01-concepts.md) · [Start here](00-start-here.md)

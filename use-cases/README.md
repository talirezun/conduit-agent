# Use cases

A **use case** is a ready-made Conduit configuration for a specific context — a filled-in `AGENTS.md`, a guide, any extra prompts or connectors, and example scenarios. The framework is universal; use cases are where it meets the real world.

This is the heart of Conduit's promise: **one core, many uses.** A business school, a sales team, a research lab, and a solo consultant can all run Conduit — each with its own use case.

---

## Available use cases

| Use case | For | Highlights |
|---|---|---|
| 🎓 **[Cotrugli Business School](cotrugli-business-school/)** | COTRUGLI / Vanguard MBA students | 30-day execution lifecycle, reviewer flow, required Cotrugli Ledger connection |
| 🧭 **[Atlas — Professional Assistant](atlas-professional-assistant/)** | Any working professional (CEO, founder, manager, consultant) | Agnostic execution assistant · meetings → action items, on-demand research, PDF reading, second brain · opencode-only, prompt-driven, privacy-first |

*More coming — yours could be next.*

---

## Build your own use case

Adapting Conduit to your workflow and sharing it back is the best way to contribute. A use case is a folder under `use-cases/` containing:

```
use-cases/your-use-case/
├── README.md          ← what it's for, who it's for, how to start
├── AGENTS.md          ← a filled-in (or specialized) agent config
├── guide.md           ← optional: a setup/operations guide
├── scenarios/         ← optional: example scenarios people can adopt
├── prompts/           ← optional: any use-case-specific prompts
└── images/            ← optional: diagrams
```

### How to create one

1. **Start from the template.** Copy [`templates/AGENTS.md`](../templates/AGENTS.md) and fill it for your context — decide which [layers](../docs/01-concepts.md) you need (as few as Execution + Memory, or all of them).
2. **Decide your rules.** What's the goal, the mandate, the evidence? Make the forbidden/approval lists specific to your domain.
3. **Add only what's special.** Most use cases need *nothing* beyond a filled `AGENTS.md`. Add a guide, scenarios, or a connector only if your context calls for it.
4. **Write a short README** so others can adopt it in minutes.
5. **Contribute it back.** See [CONTRIBUTING.md](../CONTRIBUTING.md).

### Ideas for use cases

- A weekly **sales/operations reporting** agent
- A **customer onboarding** tracker
- A **research assistant** grounded in a Curator domain
- A **content/marketing** pipeline agent
- A **compliance** agent with a required ledger
- A **personal productivity** / habit agent

> A good use case is opinionated: it makes the decisions (layers, mandate, evidence) so the next person can start in five minutes instead of from a blank page.

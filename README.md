# Conduit

**Build your own governed AI agent — by prompt, on your own computer.**

Conduit is an open framework for setting up a personal **execution agent**: an AI assistant that lives in a folder on your machine, knows its job, stays inside boundaries you set, remembers what it learns, and keeps an honest record of what it did. You set it up by *pasting prompts* — not by typing terminal commands — so you don't need to be a developer to use it.

> A conduit carries something faithfully from one place to another without changing it. That's the idea here: your agent is the conduit between **what you want** and **the tools that do it**, between **today's memory** and **everything it has learned**, and between **your private work** and an **auditable trail** — carried faithfully, with nothing leaked.

---

## Why Conduit

- **🔒 Private by default.** The agent runs on *your* computer. Your files stay with you. Short-term memory is a local file; long-term memory is a local app. Nothing is sent to a server unless you choose a tool that does so.
- **💸 Free to run.** Start with zero cost and zero accounts. Conduit works with free models, your own API key, *or* models running fully offline on your machine. (See the [Free Ladder](docs/02-choose-your-runtime.md).)
- **⌨️ No code, no terminal.** You describe what you want in plain language and paste setup prompts. The agent installs its own tools, writes its own config, and explains each step.
- **🧠 It gets smarter over time.** Through [The Curator](docs/05-the-curator.md), your agent builds a compounding "second brain" — a knowledge graph it can query across sessions.
- **🧾 Accountable.** Every agent is governed by a single config file (its *mandate*) and can keep a tamper-evident record of what it did. Good for trust, good for compliance.
- **🌍 Universal.** One core framework, many [use cases](use-cases/). The first is a business school lab; the next could be yours.

---

## Who this is for

| You are… | Start here |
|---|---|
| 🎓 A **COTRUGLI / Vanguard MBA student** | [use-cases/cotrugli-business-school](use-cases/cotrugli-business-school/) |
| 🧭 **New** and want to understand the idea first | [docs/01-concepts.md](docs/01-concepts.md) |
| 🚀 Ready to **build your own agent** now | [docs/00-start-here.md](docs/00-start-here.md) |
| 🛠️ A **practitioner** adapting Conduit to a workflow | [use-cases/README.md](use-cases/README.md) (build your own use case) |
| 🔬 A **researcher / writer** | [research/](research/) |

---

## How it works in one picture

![How a Conduit agent works](images/conduit_how_it_works.svg)

You give prompts. The agent does the technical work — installs tools, writes config, builds connectors, and records what it did back into its own config file so the next session knows.

You only ever need **one tool** to begin: an *agent harness* that can read files and run setup. Conduit supports two, and you pick based on cost and privacy:

- **[opencode](https://opencode.ai)** — open-source, free, runs free or local models. *Recommended starting point.*
- **Claude Cowork / Claude Code** — best quality, requires a Claude subscription.

Everything else — long-term memory, spreadsheets, email, an audit ledger — is **optional** and installed later, by prompt, only if you want it.

→ See [Choose your runtime — the Free Ladder](docs/02-choose-your-runtime.md).

---

## The five-minute path

1. **Pick a runtime** and install it → [docs/02](docs/02-choose-your-runtime.md)
2. **Make a folder** for your agent and drop in the [`AGENTS.md` template](templates/AGENTS.md)
3. **Paste the Setup prompt** → [templates/prompts/setup.md](templates/prompts/setup.md)
4. **Paste the "Fill my config" prompt** and answer a few questions → [templates/prompts/fill-config.md](templates/prompts/fill-config.md)
5. (Optional) **Add memory, spreadsheets, email** → [docs/05](docs/05-the-curator.md), [docs/06](docs/06-mcps.md)

That's it. You now have a working agent.

---

## What's in this repository

```
conduit-agent/
├── docs/            The framework, explained — concepts, runtimes, memory, governance
├── templates/       Copy-paste starting points: AGENTS.md, memory.md, and a prompt library
├── use-cases/       Real, ready-to-use configurations (Cotrugli Business School is #1)
├── research/        Articles and research on agent frameworks, memory, and governance
└── images/          Diagrams
```

- **New to all of this?** → [docs/00-start-here.md](docs/00-start-here.md)
- **Want the concepts?** → [docs/01-concepts.md](docs/01-concepts.md)
- **Confused by a word?** → [docs/glossary.md](docs/glossary.md)
- **Have a question?** → [docs/faq.md](docs/faq.md)

---

## Related projects

- **[The Curator](https://github.com/talirezun/the-curator)** — the open-source local "second brain" that gives Conduit agents long-term memory. ([Why it matters](docs/05-the-curator.md).)
- **[opencode](https://opencode.ai)** — the free, open-source agent harness Conduit recommends.

---

## Contributing

Conduit grows through **use cases** and **research**. If you've adapted it to a real workflow, contribute it back so others can learn from it. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

Apache License 2.0 — see [LICENSE](LICENSE). Open source today; you're free to use, adapt, and build on it.

---

*Conduit is an independent open framework. "Vanguard Agent Lab" and the Cotrugli Ledger belong to the COTRUGLI Business School use case and are one example of what Conduit can express, not the framework itself.*

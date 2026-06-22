# Contributing to Conduit

Thanks for helping build Conduit. The framework grows in two main ways — **use cases** and **research** — and we also welcome improvements to the docs and templates.

> You don't need to be a developer to contribute. A well-written use case or article is just as valuable as code.

---

## Ways to contribute

### 1. Add a use case (most valuable)

If you've adapted Conduit to a real workflow, share it. A use case is a folder under [`use-cases/`](use-cases/) with at least a `README.md` and a filled-in `AGENTS.md`. See the **[Build your own use case](use-cases/README.md#build-your-own-use-case)** guide for the structure.

A good use case:
- Targets a clear audience and goal.
- Makes the decisions (which layers, what mandate, what evidence) so others can start in minutes.
- Keeps secrets out of every file (use environment variables).
- Includes a short README that gets someone running quickly.

### 2. Write research

Articles on agents, memory, governance, privacy, or teaching with Conduit. Drop a markdown file in [`research/articles/`](research/articles/) and add it to the index. See [research/README.md](research/README.md).

### 3. Improve docs, templates, or prompts

Spotted something confusing, outdated, or missing? Fixes to [`docs/`](docs/), [`templates/`](templates/), and the prompt library are very welcome — especially anything that makes Conduit clearer for **non-technical** users.

---

## Principles to keep in mind

These are the values that make Conduit what it is. Please honor them in contributions:

1. **Prompt-first, not terminal-first.** Setup steps should be prompts a non-technical user pastes — not commands they type. If a step needs the terminal, wrap it in a prompt the agent runs.
2. **Plain language.** Write for someone who has never heard of an MCP. Define jargon or link to the [glossary](docs/glossary.md).
3. **Privacy by default.** Prefer local and free options. Never put secrets in any file — use environment variables or the tool's own store.
4. **Universal core, specific use cases.** Keep the framework (`docs/`, `templates/`) neutral. Anything tied to a particular org or context belongs in a use case.
5. **Self-documenting agents.** Prompts should tell the agent to record what it set up back into `AGENTS.md`.

---

## How to submit

1. Fork the repository and create a branch.
2. Make your change (add your use case / article / fix).
3. Update any relevant index (the use-case table, the research table).
4. Open a pull request describing what you added and why.

For larger ideas, open an issue first to discuss.

---

## License

By contributing, you agree your contributions are licensed under the project's [Apache License 2.0](LICENSE).

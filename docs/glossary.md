# Glossary

Plain-language definitions. You don't need to memorize any of this — paste the prompts and your agent handles the technical parts. This is here for when you're curious.

---

**Agent** — An AI assistant that can *do things*, not just chat: read and write your files, run setup, use tools. A Conduit agent lives in a folder on your computer and is governed by `AGENTS.md`.

**Harness** — The app your agent runs inside. It connects a language model to your files and tools. Conduit supports **opencode** (free, open-source) and **Claude Cowork / Claude Code**. → [Choose your runtime](02-choose-your-runtime.md)

**Model** — The "brain" — the large language model that actually generates the agent's responses (e.g. a free model, an OpenRouter model, a local model, or Claude). You pick one; you can change it.

**AGENTS.md** — Your agent's main config file: its job description, goal, and mandate. Named `CLAUDE.md` on Claude Cowork / Code. → [The agent config](03-the-agent-config.md)

**Mandate** — The rules in `AGENTS.md` that define what the agent may do, must never do, and may do only with your approval. The agent enforces these every session.

**memory.md** — A small file holding short-term memory: where the agent left off last session. Created and updated automatically. → [Memory](04-memory.md)

**The Curator** — A local app that gives your agent long-term memory: a knowledge graph of your documents and notes, queryable across sessions. → [The Curator](05-the-curator.md)

**Knowledge graph** — A web of connected ideas (rather than a flat pile of files). The Curator builds one from what you feed it, so the agent can find related knowledge, not just keyword matches.

**Domain** — A separate "brain" inside The Curator. You might keep one domain for personal knowledge and another for your company.

**MCP (Model Context Protocol)** — A standard way to plug new abilities into your agent. An MCP **server** is one such plug-in (Excel, email, The Curator…). Installed by prompt. → [MCPs](06-mcps.md)

**opencode** — A free, open-source agent harness. Conduit's recommended starting point. [opencode.ai](https://opencode.ai)

**Claude Cowork** — A tab inside Claude Desktop that runs locally with file and shell access, so it can do agent work. (Different from plain Claude Chat, which *can't* touch your files.)

**Claude Code** — Anthropic's command-line agent. An alternative to Cowork; also works for Conduit setup.

**OpenRouter** — A gateway that gives you access to many models through one account/key, including free ones. [openrouter.ai](https://openrouter.ai)

**Local model** — A language model running entirely on your own computer (via tools like [Ollama](https://ollama.com) or [LM Studio](https://lmstudio.ai)) — fully private, works offline.

**Ledger** — A system that records tamper-evident **hashes** of your agent's actions, for an auditable trail. → [Governance & the ledger](07-governance-ledger.md)

**Hash** — A one-way "fingerprint" of some data. It proves *that* something happened without revealing *what* it was. Hashes can't be reversed back into the original content.

**Connector** — The single thin file (e.g. `ledger_connector.py`) that talks to a ledger backend. The only part that changes if you switch backends; the agent never changes.

**Tamper-evident** — Any change to a recorded event would be detectable, because the fingerprint wouldn't match. Makes a record trustworthy.

**Environment variable** — A secure place outside your project files to store secrets like API keys. Conduit prompts always store secrets here, never in `AGENTS.md`.

**Use case** — A ready-made Conduit configuration for a specific context (e.g. a business school lab, a sales workflow). Lives in [use-cases/](../use-cases/). You can build your own.

**Layer** — One functional part of an agent: Execution, Memory, Data, Intelligence, Communication, Governance. You turn on the ones you need. → [Concepts](01-concepts.md)

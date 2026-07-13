# FAQ

### Do I need to know how to code?

No. Conduit is **prompt-driven**: you describe what you want in plain language and paste setup prompts. The agent installs tools, writes config, and explains each step. The only thing you create by hand is one folder and one file (`AGENTS.md`).

### Do I have to type terminal commands?

No. The agent runs anything technical for you. On Claude Desktop there's exactly **one** manual step: restarting the app after an MCP is added.

### Is it really free?

Yes, to start. opencode is free and ships with free models. You can also use free OpenRouter models, or run a model fully offline on your own machine. A paid Claude subscription (~$20/mo) gives the best quality but is optional. → [The Free Ladder](02-choose-your-runtime.md)

### Is my data private?

Yes. The agent runs on your computer; your files stay there. Working memory is a local file, your context layer ([The Curator](05-the-curator.md)) is a local app, and a ledger only ever receives **hashes**, never content. With a local model (Rung 2), nothing leaves your machine at all.

### What's the difference between Claude Cowork and plain Claude Chat?

Cowork (and Claude Code) can read your files, install software, and do setup — it's a real agent. Plain Claude Chat cannot touch your files. **Always use Cowork or Claude Code for Conduit**, never the normal chat window.

### Which should I choose — opencode or Claude?

Start with **opencode** if you want free and private, or if you don't have a Claude subscription. Choose **Claude Cowork / Code** if you have a subscription and want the smoothest, highest-quality experience (and native Excel). You can switch later. → [Choose your runtime](02-choose-your-runtime.md)

### What does The Curator actually give my agent?

A context layer — a compounding second brain. Your documents and notes become a knowledge graph the agent queries across sessions, organized into domains. The more you add, the more context it has — which makes it far better at real tasks. → [The Curator](05-the-curator.md)

### Does `memory.md` get created manually?

No. The [Setup prompt](../templates/prompts/setup.md) creates it, and the agent updates it automatically at the end of every session.

### Do I need a separate `SKILL.md` or other files?

No. Everything lives in `AGENTS.md` (or `CLAUDE.md`). One file, on purpose, to keep things simple.

### Can I build both a personal and a company agent?

Yes. Make two folders, each with its own `AGENTS.md`. They're completely independent — separate memory, reports, and config.

### What if my goal changes mid-project?

Tell the agent: *"My original goal needs to change. The new target is [X]."* It records the change without deleting the original.

### Do I have to connect a ledger?

Not in the universal framework — it's optional. Some **use cases require it** (the Cotrugli lab does). Add it any time with one prompt. → [Governance & the ledger](07-governance-ledger.md)

### What if a tool or MCP fails during a session?

The agent notes the failure clearly and keeps working with the tools it has. Fix it afterward with a prompt and it'll be available next time.

### Can I add MCPs that aren't listed?

Yes. Ask your agent to install any MCP using the standard pattern in [docs/06](06-mcps.md). It handles installation, config, and recording it in `AGENTS.md`.

### How do I keep secrets safe?

Never put API keys or passwords in `AGENTS.md` or any project file. The prompts always store secrets in environment variables or a tool's own credential store. → [The agent config](03-the-agent-config.md)

### Is "Conduit" the same as "Vanguard"?

No. **Conduit** is the universal framework. **Vanguard Agent Lab** is one *use case* built on Conduit — the COTRUGLI Business School configuration. → [use-cases/](../use-cases/)

### How do I contribute?

Share a use case or a research article. See [CONTRIBUTING.md](../CONTRIBUTING.md).

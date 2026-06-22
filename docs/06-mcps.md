# MCPs — giving your agent more abilities

Out of the box, your agent can read and write files in its folder. To do more — write spreadsheets, send email, query your second brain — it connects to **MCP servers**.

**MCP** stands for *Model Context Protocol*. Think of an MCP as a **plug-in** that gives your agent one new ability. You install them the same way you do everything in Conduit: **by pasting a prompt.** You never paste JSON into a config file by hand — the agent does it, and records what it added back into `AGENTS.md`.

> **Where MCP config lives** (the agent handles this — just know it exists):
> - **opencode** → `opencode.jsonc` in your project folder
> - **Claude Cowork / Code** → `claude_desktop_config.json`, then **restart Claude Desktop**
>
> Never mix them up. Every install prompt tells the agent which file to use.

---

## 📊 Excel — spreadsheets

For project trackers, summaries, and logs.

- **Claude Cowork / Code:** reads and writes Excel **natively** — nothing to install.
- **opencode:** install the Excel MCP (`haris-musa/excel-mcp-server`).

➡️ opencode users: paste the **[Install Excel prompt](../templates/prompts/install-excel.md)**.

---

## 📧 Atomic Mail — an agent-owned mailbox

Gives your agent its **own** mailbox. There's no human webmail login — the agent registers the account, holds the credentials, and is the only one who reads or sends from it.

➡️ Paste the **[Install Atomic Mail prompt](../templates/prompts/install-atomic-mail.md)**.

> ⚠️ **The email rule, always:** the agent **never sends an email without your explicit "confirm send."** It always shows you the draft first. This belongs in your mandate ([the agent config](03-the-agent-config.md)).

- Docs: the `atomicmail` MCP speaks JMAP under the hood; you don't need to know that to use it.

---

## 🧠 The Curator — long-term memory

The most important MCP. Covered in its own guide because it's a multi-level install.

➡️ See **[The Curator](05-the-curator.md)**.

---

## Adding any other MCP

Conduit isn't limited to these. There are MCP servers for calendars, GitHub, databases, web scraping, and much more. To add one, paste a prompt like:

```
Please install the [NAME] MCP server for this project so the agent can [WHAT IT DOES].
Steps:
1. Check for and install any runtime it needs (e.g. Node.js or a Python runner).
2. Register it in the config file for the harness you are running in:
   - opencode -> opencode.jsonc in this project folder
   - Claude Cowork / Code -> claude_desktop_config.json (then remind me to restart)
   Never edit the other harness's config file.
3. Verify it works with a small test.
4. Record in AGENTS.md that this MCP is installed and how to reach it.
Never write any secret (API key, token) into AGENTS.md — use environment variables
or the tool's own credential store. Explain each step in plain language.
```

The agent figures out the rest.

> 💡 Looking for an MCP? Browse public MCP registries, or just ask your agent: *"Is there an MCP that can do [X]? If so, install it using the standard Conduit pattern."*

---

## If an MCP isn't available during a session

The agent notes it clearly and continues with the tools it has — your session is never blocked. Fix it afterward with a prompt and it'll be there next time.

---

Next: [Governance & the ledger →](07-governance-ledger.md)

# The prompt library

Every Conduit setup step is a **prompt** you copy and paste into your agent (opencode, Claude Cowork, or Claude Code). No terminal commands. Pick the prompt you need, copy the code block, paste it, and let the agent work.

| Prompt | What it does | When |
|---|---|---|
| [setup.md](setup.md) | Creates folders, memory file, installs needed runtimes | First, always |
| [fill-config.md](fill-config.md) | Interviews you and fills `AGENTS.md` | Right after setup |
| [install-curator.md](install-curator.md) | Installs The Curator (context layer) + connects the MCP | When you want a second brain / compounding context |
| [install-excel.md](install-excel.md) | Adds Excel read/write (opencode only) | When you need spreadsheets |
| [install-atomic-mail.md](install-atomic-mail.md) | Sets up an agent-owned mailbox | When you want email |
| [add-any-mcp.md](add-any-mcp.md) | Generic pattern for installing any other MCP | Anytime |

> **Rule of thumb:** if a prompt seems to do nothing, check you're in a real agent (opencode / Claude Cowork / Claude Code) and not plain Claude Chat — plain chat can't touch files.

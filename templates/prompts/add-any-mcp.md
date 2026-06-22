# Add any MCP prompt (generic pattern)

Conduit isn't limited to Excel, email, and The Curator. Use this pattern to add **any** MCP server — calendars, GitHub, databases, web search, and more. Replace `[NAME]` and `[WHAT IT DOES]`.

```
Please install the [NAME] MCP server for this project so the agent can
[WHAT IT DOES]. Follow the standard Conduit pattern and explain each step:

1. Check for and install any runtime it needs (e.g. Node.js or a Python runner).
2. Register it in the config file for the harness you are running in:
   - opencode -> opencode.jsonc in this project folder
   - Claude Cowork / Code -> claude_desktop_config.json, then remind me to
     fully restart Claude Desktop.
   Never edit the other harness's configuration file.
3. Verify it works with a small, safe test.
4. Record in AGENTS.md (Capabilities table) that this MCP is installed and how to
   reach it.

Never write any secret (API key, token, password) into AGENTS.md or any project
file — store secrets in environment variables or the tool's own credential store.
```

> 💡 Don't know if an MCP exists for what you want? Just ask: *"Is there an MCP that can do [X]? If so, install it using the standard Conduit pattern and tell me what it can do."*

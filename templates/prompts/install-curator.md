# Install The Curator prompt

Gives your agent long-term memory — a compounding second brain. This prompt handles **Level 1** (install the app) and **Level 3** (connect the MCP). **Level 2** (the onboarding wizard: API key + first domain) happens in the app's browser window when it opens. → [Why The Curator matters](../../docs/05-the-curator.md)

```
Please install "The Curator" on this machine for me.
Project:    https://github.com/talirezun/the-curator
User Guide: https://github.com/talirezun/the-curator/blob/main/docs/user-guide.md

Steps:
1. Verify Node.js 18+ is installed; if not, install it (Homebrew on macOS,
   nodejs.org installer on Windows, system package manager on Linux).
2. Clone https://github.com/talirezun/the-curator.git into my home directory.
3. cd the-curator && npm install
4. On macOS: run bash scripts/build-app.sh to build "The Curator.app", copy it to
   /Applications, and remind me to drag it from Finder into my Dock.
   On Linux/Windows: skip the .app build; explain how to start the server
   (`node src/server.js`, with CURATOR_NO_OPEN=1 on Windows) and remind me to
   open http://localhost:3333.
5. Open http://localhost:3333 once the server is running so I can complete the
   onboarding wizard (API key + my first domain). Do NOT ask me for my API key —
   the in-app wizard handles it.
6. Register the my-curator MCP in the config file for the harness you are in:
   - opencode -> opencode.jsonc in this project folder
   - Claude Cowork / Code -> claude_desktop_config.json, then remind me to fully
     restart Claude Desktop.
7. Tell me how to enable GitHub sync if I want it (point me to docs/sync.md).

Do not edit any files outside ~/the-curator and the correct MCP config file. Do
not commit anything to my git config. After install, summarise what you did in 5
bullet points and add a note to AGENTS.md (Memory + Capabilities) recording that
the my-curator MCP is installed and how to reach it.
```

> 💡 After install, create one domain for your **personal** knowledge and one for your **company**. Ingest a few real documents early — the agent's usefulness compounds as the graph grows.

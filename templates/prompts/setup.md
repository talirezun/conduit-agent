# Setup prompt

Paste this into your agent (opencode / Claude Cowork / Claude Code) after placing `AGENTS.md` in your folder. It builds the agent's structure and installs anything missing.

```
Read the AGENTS.md file in this project folder carefully. Then do the following,
and explain each step in plain language as you go:

1. Create this folder structure inside the current project folder:
   reports/   tasks/   evidence/   data/   notes/

2. Create memory.md in the project root with this starting content:
   Last session: [today's date]
   Last action: Setup complete
   Current status: Day 0 — not yet started
   Unresolved items: None
   Notable events: Agent configured and ready
   Next planned action: Define the first task

3. Check whether the tools you need are installed (for example a Python runner
   such as uv, and Node.js). If any required tool is missing, install it for me
   and tell me what you installed. Do not ask me to run terminal commands —
   you handle it.

4. If you create or edit an MCP configuration file, edit the file that belongs
   to the harness you are running in:
   - opencode -> opencode.jsonc in THIS project folder
   - Claude Cowork / Claude Code -> claude_desktop_config.json (system location)
   Never edit the other harness's configuration file. If you are in Claude,
   remind me that I must fully restart Claude Desktop after the file changes.

5. Whenever you install or configure anything that future sessions will need to
   know about, append a short note to the relevant section of AGENTS.md so the
   next session is aware of it. Never write secrets (API keys, tokens, passwords)
   into AGENTS.md — those belong in environment variables or the tool's own
   credential store.

6. Confirm what was installed and created, then read memory.md and give me a
   short status summary as described in AGENTS.md.
```

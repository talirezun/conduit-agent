# Install The Curator prompt

Gives your agent long-term memory — a compounding second brain. → [Why The Curator matters](../../docs/05-the-curator.md)

The Curator install is **two steps**, and only Step 2 differs by harness:

- **Step 1 — install the app** (same on both tracks). Clones, builds, and launches the local Curator app so you can create your first domain (Level 1 + the in-app onboarding wizard, Level 2). It does **not** connect your agent yet.
- **Step 2 — connect the MCP + usage skill** (Level 3, split by track). Registers the `my-curator` MCP so your agent can query the wiki, and installs the Curator **usage skill** so it uses those tools well (no broken links / duplicate pages).

The MCP config **format** differs by harness — see [MCPs — the two config formats](../../docs/06-mcps.md). Use the Step 2 prompt for your harness.

---

## Step 1 — install the app (both tracks)

```
Please install "The Curator" app on this machine for me.
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
6. Tell me how to enable GitHub sync if I want it (point me to docs/sync.md).

Do not edit any files outside ~/the-curator. Do not commit anything to my git
config. After install, summarise what you did in 5 bullet points and add a note to
AGENTS.md that the Curator app is installed at ~/the-curator, that it must be
running for the second brain to work, and that the my-curator MCP will be connected
next (Step 2). Do NOT register the MCP yet — that is Step 2.
```

> 💡 After install, create one domain for your **personal** knowledge and one for your **company**. Ingest a few real documents early — the agent's usefulness compounds as the graph grows. Keep the Curator app **running** whenever you want your agent to use the second brain.

---

## Step 2 — connect the MCP + skill

The Curator MCP server is the file `~/the-curator/mcp/server.js`, launched with `node`. The Curator app also generates a ready-made snippet for you (**Settings → "My Curator" → "Copy snippet"**) — but that snippet is always in Claude's `mcpServers` format, so on opencode your agent must translate it.

### Track A — opencode

```
The Curator app is installed and running. Connect it to THIS opencode project.
Work only in the project-level opencode.jsonc in this folder (not the global
~/.config/opencode config) — and never touch any Claude config file.

1. Find the MCP server file at ~/the-curator/mcp/server.js (expand ~ to an absolute
   path). If I give you the app's "Copy snippet", note it is in Claude's format —
   TRANSLATE it, don't paste it.
2. Register the my-curator MCP in opencode.jsonc using opencode's format EXACTLY:
       {
         "mcp": {
           "my-curator": {
             "type": "local",
             "command": ["node", "<ABSOLUTE path to ~/the-curator/mcp/server.js>"],
             "enabled": true
           }
         }
       }
   If the snippet had an env block (e.g. a DOMAINS_PATH), add it as an
   "environment": { ... } object on the same entry (opencode uses "environment",
   NOT "env"). Merge into any existing "mcp" block rather than overwriting.
3. Install the Curator usage skill for opencode (opencode has no ~/.claude/skills):
   a. Download this opencode-adapted skill into a "curator-skill/" folder in THIS
      project (save as curator-skill/my-curator.md):
          https://raw.githubusercontent.com/talirezun/conduit-agent/main/use-cases/cotrugli-business-school/curator-skill/my-curator.md
   b. Register it so opencode always reads it — add to opencode.jsonc (merge):
          "instructions": ["curator-skill/my-curator.md"]
4. Reload opencode / restart the session, then verify: call the my-curator
   "list_domains" tool and show me my domains. If it fails, check the Curator app
   is running.
5. Record in AGENTS.md (Memory / long-term memory) that my-curator is connected, the
   path curator-skill/my-curator.md, and that the Curator app must be running.
After setup, summarise what you did in 5 bullet points.
```

### Track B — Claude (Cowork / Claude Code)

```
The Curator app is installed and running. Connect it to Claude.
Work only in claude_desktop_config.json (system location) — never touch opencode.jsonc.

1. Get the MCP snippet: Curator app -> Settings -> "My Curator" -> "Copy snippet".
   It is already in Claude's format. Paste it to me. (If you can't, the server file
   is ~/the-curator/mcp/server.js — build the entry yourself.)
2. Merge that snippet under the "mcpServers" key in claude_desktop_config.json,
   WITHOUT deleting any MCP already there:
       {
         "mcpServers": {
           "my-curator": {
             "command": "node",
             "args": ["<ABSOLUTE path to ~/the-curator/mcp/server.js>"]
           }
         }
       }
   If the snippet had an env block (e.g. DOMAINS_PATH), keep it as an "env": { ... }
   object on the same entry (Claude uses "env", NOT "environment").
3. Install the Curator usage skill:
   - Claude Code:
         mkdir -p ~/.claude/skills/my-curator
         curl -L https://raw.githubusercontent.com/talirezun/the-curator/main/claude-skills/my-curator/SKILL.md -o ~/.claude/skills/my-curator/SKILL.md
         curl -L https://raw.githubusercontent.com/talirezun/the-curator/main/claude-skills/my-curator/examples.md -o ~/.claude/skills/my-curator/examples.md
   - Claude Cowork / Desktop: download those two files and add them to my project's
     Knowledge (the + in the Files section), or paste SKILL.md (minus its
     frontmatter) into the project's custom instructions.
4. Remind me to fully quit and restart Claude Desktop so the MCP loads.
5. After I restart, verify: call the my-curator "list_domains" tool and show me my
   domains. If it fails, confirm the Curator app is running.
6. Record in AGENTS.md (Memory / long-term memory) that my-curator is connected,
   where the skill files live, and that the Curator app must be running.
After setup, summarise what you did in 5 bullet points.
```

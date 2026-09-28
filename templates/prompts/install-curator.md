# Install The Curator prompt

Gives your agent a **context layer** — a compounding second brain (compounding context across sessions). → [Why The Curator matters](../../docs/05-the-curator.md)

The Curator install has **three steps**:

- **Step 1 — get the app.** Two options: the **Mac app** (a download — macOS only, no Node.js needed) or the **browser app** (installed by prompt — Windows, Linux, or Mac). Both are the same program; pick one.
- **Step 2 — first run, in the app.** The Curator's *Getting started* panel walks you through adding an AI key and creating your first domain. You do this yourself, in the app — your agent never sees your key.
- **Step 3 — connect the MCP + skills** (split by harness). Registers the `my-curator` MCP so your agent can use your second brain, and installs The Curator's two official **skills** — `my-curator` (how to read and write the wiki cleanly) and `curator-continuity` (how to carry work between sessions).

> **Good to know:** the MCP bridge reads your knowledge folder directly, so it **works with The Curator app closed**. You only need the app open to ingest documents, chat, or change settings.

The MCP config **format** differs by harness — see [MCPs — the two config formats](../../docs/06-mcps.md). Use the Step 3 prompt for your harness.

---

## Step 1 — get the app

### Option 1 — the Mac app (recommended on a Mac)

This one is a download you do yourself — it takes two minutes, and it is the one install your agent should not do for you, because macOS asks *you* to approve it.

1. Open the **[Releases page](https://github.com/talirezun/the-curator/releases)** and download the `.dmg` for your Mac from the newest release:
   - Apple Silicon (M1 and later): `TheCurator-<version>-arm64-AppleSilicon.dmg`
   - Intel: `TheCurator-<version>-x64-Intel.dmg`
   *(Not sure which Mac you have? Apple menu → About This Mac → "Chip".)*
2. Open the `.dmg` and drag **The Curator** onto **Applications**.
3. **First launch only:** macOS blocks it, because the app is not yet notarized by Apple (enrolment is in progress). Double-click the app, dismiss the warning, then go to **System Settings → Privacy & Security**, scroll to **Security**, and click **Open Anyway**. Open the app again — macOS remembers your choice.

The Mac app updates itself from then on (**The Curator → Check for Updates…**). Details: [The Mac app guide](https://github.com/talirezun/the-curator/blob/main/docs/mac-app.md).

### Option 2 — the browser app (Windows, Linux, or Mac) — by prompt

```
Please install "The Curator" (browser app) on this machine for me, and explain
each step in plain language as you go.
Project:    https://github.com/talirezun/the-curator
User Guide: https://github.com/talirezun/the-curator/blob/main/docs/user-guide.md

Steps:
1. Check that Node.js 18 or newer is installed; if not, install it (Homebrew on
   macOS, the nodejs.org installer on Windows, the system package manager on
   Linux). Tell me what you installed.
2. Clone https://github.com/talirezun/the-curator.git into my home folder.
3. In that folder, run: npm install
4. Start the server: node src/server.js
   (on Windows PowerShell: $env:CURATOR_NO_OPEN=1; node src\server.js)
5. Open http://localhost:3333 in my browser so I can do the in-app first run.
6. Tell me how to start it again next time (and, on macOS, that
   bash scripts/build-app.sh can build me a Dock launcher if I want one).

Do not edit any files outside the-curator folder. Do not ask me for an API key —
I add it in the app myself. After install, summarise what you did in 5 bullets
and note in AGENTS.md that The Curator (browser app) is installed at
~/the-curator. Do NOT register the MCP yet — that is Step 3.
```

---

## Step 2 — first run, in the app (you do this)

When The Curator opens for the first time, a **Getting started** panel asks what you want to set up first. Choose **Build a second brain** and follow its three steps:

1. **Add an AI key** — the app uses it to turn your documents into a knowledge graph. A free **Google Gemini** key is the easiest start: [aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey). (Anthropic and OpenRouter keys also work.) Paste it into the app — **never into your agent chat.**
2. **Create your first domain** — e.g. `work` or `personal`.
3. **Ingest your first source** — drop in a real document (a report, an article, your notes).

> The free Gemini tier is fine for trying it out; for regular use, enabling billing in Google AI Studio typically costs a few euros a month. See [the API key guide](https://github.com/talirezun/the-curator/blob/main/docs/user-guide.md#4-get-your-api-key-gemini-claude-or-openrouter).

---

## Step 3 — connect the MCP + skills

### Track A — opencode

```
The Curator is installed and I have created my first domain. Connect it to THIS
opencode project, and explain each step in plain language as you go. Work only in
the project-level opencode.jsonc in this folder (not the global ~/.config/opencode
config) — and never touch any Claude config file.

1. Work out which install I have:
   - Mac app: the file
       ~/Library/Application Support/The Curator/bin/my-curator-mcp
     exists (expand ~ to my full home path). The app writes it on every launch.
   - Browser app: ~/the-curator/mcp/server.js exists.
   If neither exists, stop and tell me (for the Mac app: open The Curator once,
   then try again).
2. Register the my-curator MCP in opencode.jsonc, using opencode's format EXACTLY
   and ABSOLUTE paths. Merge into any existing "mcp" block — never overwrite it.
   - Mac app:
       "my-curator": {
         "type": "local",
         "command": ["<ABSOLUTE path to .../The Curator/bin/my-curator-mcp>"],
         "enabled": true
       }
   - Browser app:
       "my-curator": {
         "type": "local",
         "command": ["node", "<ABSOLUTE path to ~/the-curator/mcp/server.js>"],
         "enabled": true
       }
   (If I paste you a snippet from The Curator's Settings → MCP bridge, it is in
   Claude's "mcpServers" format — TRANSLATE it: command + args become ONE
   "command" array, and any "env" becomes "environment".)
3. Install The Curator's two official skills into THIS project. Download every
   file below into .agents/skills/ (create the folders), keeping the file names:
     Base: https://raw.githubusercontent.com/talirezun/the-curator/main/skills
     .agents/skills/my-curator/         SKILL.md  examples.md  maintenance.md  shared-brain.md
     .agents/skills/curator-continuity/ SKILL.md  examples.md  brief-authority.md
   Each file is at <Base>/<skill-name>/<file-name>. Check each download is a
   real markdown file (not an error page). opencode discovers skills in
   .agents/skills/ automatically — no config entry is needed.
4. Tell me to quit and reopen opencode (or start a new session) so it loads the
   new MCP and skills. After I do, verify: call the my-curator "list_domains"
   tool and show me my domains, and confirm both skills are available.
5. Record in AGENTS.md (Memory & Context / context layer): that my-curator is
   connected, which install I have (Mac app or browser app), that the skills live
   in .agents/skills/, and that the MCP works even when the Curator app is closed.
After setup, summarise what you did in 5 bullets.
```

### Track B — Claude (Cowork / Claude Code)

The easiest route on Claude Desktop is The Curator's own wizard: open The Curator → **Settings → MCP bridge → Set up Claude Desktop**, follow it, then fully quit and reopen Claude Desktop. The prompt below does the same thing through your agent, and installs the skills.

```
The Curator is installed and I have created my first domain. Connect it to Claude,
and explain each step in plain language as you go. Work only in
claude_desktop_config.json (system location) — never touch opencode.jsonc.

1. Work out which install I have:
   - Mac app: ~/Library/Application Support/The Curator/bin/my-curator-mcp exists
     (expand ~ to my full home path).
   - Browser app: ~/the-curator/mcp/server.js exists.
2. Merge a my-curator entry under "mcpServers" in claude_desktop_config.json,
   WITHOUT deleting any MCP already there. Use ABSOLUTE paths.
   - Mac app:
       "my-curator": {
         "command": "<ABSOLUTE path to .../The Curator/bin/my-curator-mcp>",
         "args": []
       }
   - Browser app:
       "my-curator": {
         "command": "node",
         "args": ["<ABSOLUTE path to ~/the-curator/mcp/server.js>"]
       }
   (Claude uses "env" for environment variables, NOT "environment".)
3. Install The Curator's two official skills. Files are at
   https://raw.githubusercontent.com/talirezun/the-curator/main/skills/<skill-name>/<file-name>:
     my-curator:         SKILL.md  examples.md  maintenance.md  shared-brain.md
     curator-continuity: SKILL.md  examples.md  brief-authority.md
   - Claude Code: download them into ~/.claude/skills/my-curator/ and
     ~/.claude/skills/curator-continuity/.
   - Claude Cowork / Desktop: download them into a folder I can find, then walk me
     through adding them to my project's knowledge (or, on the Mac app, point me to
     The Curator → Settings → MCP bridge → Tools on this Mac, which offers both
     skills as ready-made .zip files).
4. Remind me to fully quit (Cmd+Q) and reopen Claude Desktop so the MCP loads.
5. After I restart, verify: call the my-curator "list_domains" tool and show me my
   domains.
6. Record in AGENTS.md (Memory & Context / context layer): that my-curator is
   connected, which install I have, where the skill files live, and that the MCP
   works even when the Curator app is closed.
After setup, summarise what you did in 5 bullets.
```

---

## Keep going

- **Feed it.** Ingest a few real documents early — the agent's usefulness compounds as the graph grows. One domain for **personal** knowledge and one for **company** knowledge keeps the graph clean.
- **Share it.** A team or class can build a **Shared Brain** together → [Shared Brain user guide](https://github.com/talirezun/the-curator/blob/main/docs/shared-brain-user-guide.md).
- **Manage context across sessions and tools** with Curator projects → [Working state](https://github.com/talirezun/the-curator/blob/main/docs/working-state.md).
- **Update the skills** any time by re-running Step 3's download — the files are overwritten with the current version.

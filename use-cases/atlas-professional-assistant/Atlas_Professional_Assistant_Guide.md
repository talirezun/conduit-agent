# Atlas — Professional Assistant Agent · Setup & Operations Guide

*Conduit Use Case #2 · Guide v1.0*
*A general-purpose execution assistant for professionals — built on [Conduit](../../README.md), running on opencode (free & open-source).*

> **New to Conduit?** Read [docs/00-start-here.md](../../docs/00-start-here.md) and [the concepts](../../docs/01-concepts.md) first. This guide assumes you understand the basics and focuses on building **Atlas**.

---

## How this guide works (read this first)

Atlas is **prompt-driven**. You don't edit config files, run terminal commands, or wire up tools by hand. You **copy a prompt, paste it into your agent, and the agent builds itself** — folders, memory, and every tool (MCP) installs by prompt. That is the whole point of the Conduit framework: you describe what you want in plain language, and the agent does the engineering.

Every grey box below is a **copy-paste prompt**. Work top to bottom:

1. **Section 2–3** — install opencode, make a folder, drop in `AGENTS.md`.
2. **Section 4** — paste the **Setup Prompt**. The agent builds the structure.
3. **Section 5** — paste the **Fill My Profile** prompt. The agent makes Atlas *yours*.
4. **Section 6** — paste install prompts for the tools you want (Curator, Excel, Web, PDF).
5. **Section 7** — optional modules (meeting notes, email).
6. **Section 8–9** — run Atlas with the ready-made scenario prompts.

You can stop after Section 6 and have a genuinely useful assistant. The optional modules add meeting transcription and email when you want them.

---

# 1. What You Are Building

**Atlas** is an execution assistant for a working professional — a CEO, founder, manager, consultant, or individual contributor. It is **agnostic**: not a personal-life bot, not a single-purpose business bot. It captures and tracks your tasks, turns meetings into action items, researches on demand, reads documents, keeps a compounding second brain, and prepares reports and drafts — always inside a clear mandate, always keeping you in control of anything that leaves your machine.

### The layers

| Layer | What it does | Status |
|---|---|---|
| 🗂️ **Tasks** | Capture and follow up on your work (`tasks.md`) | Always on |
| 🧠 **Memory** | `memory.md` (short-term) + My Curator (long-term second brain) | Always on |
| 📊 **Data** | Excel workbooks for logs and reports | Recommended |
| 🔎 **Research** | No-auth web fetch + PDF reading | Recommended |
| 🎙️ **Meetings** | Local, private transcription → minutes + action items | Optional |
| 📧 **Communication** | Agent-owned mailbox + optional real inbox | Optional |

### What Atlas does day to day
- Turns a meeting recording into **minutes + action items** with owners and due dates
- Prepares a **research brief** before your next call ("brief me on Company X")
- Reads a **contract or board deck** and pulls the key terms/figures
- Keeps your **tasks and follow-ups** so nothing slips
- Compiles a **weekly digest** of what got done, what's open, what's next
- Drafts **emails and reports** for your approval

---

# 2. Install opencode (the only thing you install by hand)

Everything else, Atlas installs for you later through prompts.

**opencode** is a free, open-source AI coding agent that works as a powerful agent harness. It reads your `AGENTS.md`, has access to your file system, connects to MCP servers, installs software, and writes config files for you.

### Steps
1. Go to **opencode.ai** and download the app for your OS (macOS, Windows, Linux).
2. Install it (about two minutes).
3. On first launch, **pick a free model**. opencode always has free models available — no subscription or API key required to start.

| **Free & private options** | opencode is completely free and open-source. It ships with free hosted models, and it also works with **local models via [Ollama](https://ollama.com)** — so you can run Atlas fully on your own machine if you prefer maximum privacy. Point opencode at an Ollama model in its model picker. |
| --- | --- |

> You do **not** install Node.js, `uv`, or any MCP by hand. When a tool is needed, the install prompt tells Atlas to check for it and install it if it's missing.

---

# 3. Create Your Project Folder

Each agent lives in its own folder. You create **one folder** by hand and drop `AGENTS.md` inside it — the Setup Prompt builds the rest.

### Steps
1. Make a folder with a clear name, e.g. `atlas-assistant/` (or `petra-assistant/`).
2. Copy the **[AGENTS.md](AGENTS.md)** from this use case into the root of that folder.
3. Do **not** create subfolders yourself — the Setup Prompt does that.
4. In opencode, **File → Open Folder** (or drag the folder in). You should see it as your working directory.

> **A note on MCP config (opencode).** opencode registers MCP servers in a **project-level** `opencode.jsonc` inside your folder — not the global `~/.config/opencode` config. This keeps each agent's tools scoped to its own folder, so different agents don't share tools. Every prompt in this guide already targets the project file; if Atlas ever offers to edit the global config, tell it to use the project `opencode.jsonc` in this folder instead.

---

# 4. The Setup Prompt — Let Atlas Build Everything

Open your folder in opencode and paste this. Atlas reads `AGENTS.md` and builds its structure.

**SETUP PROMPT — copy & paste:**
```
Read the AGENTS.md file in this project folder carefully. Then do the following,
and explain each step in plain language as you go:

1. Create this folder structure inside the current project folder:
   reports/weekly/   meetings/   briefs/   documents/   data/

2. Create tasks.md in the project root with this starting content:
   # Tasks
   (no open tasks yet — add with "new task")

3. Create memory.md in the project root with this starting content:
   Last session: [today's date]
   Last action: Setup complete
   Open tasks: 0
   Unresolved items: None
   Notable events: Atlas configured and ready
   Next planned action: Fill my Professional Profile

4. Check whether the tools you need are installed (Node.js, and a Python runner
   such as uv). If any are missing, install them for me and tell me what you
   installed. Do not ask me to run terminal commands — you handle it.

5. If you create or edit an MCP configuration file, edit the PROJECT-LEVEL
   opencode.jsonc in THIS folder (never the global ~/.config/opencode config),
   using opencode's "mcp" format.

6. Whenever you install or configure anything future sessions will need to know
   about, append a short note to the relevant section of AGENTS.md. Never write
   secrets (API keys, tokens, passwords) into AGENTS.md — those belong in
   environment variables or the tool's own credential store.

7. Confirm what was created, then read memory.md and give me the Daily Status
   Check described in AGENTS.md.
```

---

# 5. Configure Atlas — Make It Yours

Atlas needs to know who you are and what you do. Two ways: let it interview you (recommended), or edit `AGENTS.md` by hand.

### Option A — Let Atlas fill your profile (recommended)

**FILL MY PROFILE PROMPT — copy & paste:**
```
Open the AGENTS.md file in this project folder. You are going to help me fill in
the Professional Profile and tune the Mandate. Interview me ONE question at a
time — do not flood me with questions.

Collect:
  - My full name, my role/title, and my organisation (or "Independent")
  - My 2–4 focus areas (what I spend working time on)
  - My recurring responsibilities Atlas should help with
  - The tools/systems I use (optional)
  - My working-style preferences (brief vs detailed, tone, timezone, when my
    week resets) — optional
  - 2–3 extra actions Atlas IS allowed to do for me
  - 1–2 actions Atlas must NEVER do
  - Any action that should require my explicit approval

Then write my answers into the Professional Profile block and the Mandate lists
in AGENTS.md, replacing the placeholders. Show me the result and ask me to
confirm before saving. Do not invent details — if something is unclear, ask.
```

### Option B — Fill it by hand
Open `AGENTS.md`, replace every `[PLACEHOLDER]` in the **Professional Profile**, and add your own items to the **Mandate** lists. Save.

> No business problem in mind yet? Use one of the [scenarios](scenarios/README.md) to fill your profile and try Atlas out.

---

# 6. Install the Tools — By Prompt

Install what you want, in any order. Each is a copy-paste prompt; Atlas does the rest and records what it did in `AGENTS.md`. All snippets are already in **opencode format**.

> **The generic pattern.** Any MCP not covered here can be added the same way — just ask: *"Is there an MCP that can do [X]? If so, install it for this opencode project using the standard Conduit pattern (project opencode.jsonc, opencode's `mcp` format), verify it, and tell me what it can do."*

---

## 6.1 My Curator — Long-Term Memory (the second brain)

The Curator gives Atlas a compounding knowledge graph it can query and grow across sessions. → [Why The Curator matters](../../docs/05-the-curator.md). Install is **two steps**.

### Step 1 — install the app

**CURATOR — STEP 1 PROMPT:**
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
   (node src/server.js, with CURATOR_NO_OPEN=1 on Windows) and remind me to open
   http://localhost:3333.
5. Open http://localhost:3333 once the server is running so I can complete the
   onboarding wizard (API key + my first domain). Do NOT ask me for my API key —
   the in-app wizard handles it.
6. Tell me how to enable GitHub sync if I want it (point me to docs/sync.md).

Do not edit any files outside ~/the-curator. After install, summarise what you did
in 5 bullets and note in AGENTS.md that the Curator app is installed at
~/the-curator, that it must be running for the second brain to work, and that the
my-curator MCP will be connected next (Step 2). Do NOT register the MCP yet.
```

> 💡 After install, create one domain for your **personal** knowledge and one for your **company/work**. Ingest a few real documents early — Atlas gets more useful as the graph grows. Keep the Curator app **running** whenever you want Atlas to use the second brain.

### Step 2 — connect the MCP + usage skill

**CURATOR — STEP 2 PROMPT (opencode):**
```
The Curator app is installed and running. Connect it to THIS opencode project.
Work only in the project-level opencode.jsonc in this folder (not the global
~/.config/opencode config).

1. Find the MCP server file at ~/the-curator/mcp/server.js (expand ~ to an
   absolute path). If I give you the app's "Copy snippet", note it is in Claude's
   format — TRANSLATE it, don't paste it.
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
   "environment": { ... } object on the same entry. Merge into any existing "mcp"
   block rather than overwriting.
3. Install the Curator usage skill for opencode:
   a. Download this opencode-adapted skill into a "curator-skill/" folder in THIS
      project (save as curator-skill/my-curator.md):
          https://raw.githubusercontent.com/talirezun/conduit-agent/main/use-cases/atlas-professional-assistant/curator-skill/my-curator.md
   b. Register it so opencode always reads it — add to opencode.jsonc (merge):
          "instructions": ["curator-skill/my-curator.md"]
4. Reload opencode / restart the session, then verify: call the my-curator
   "list_domains" tool and show me my domains. If it fails, check the Curator app
   is running.
5. Record in AGENTS.md (Memory / long-term memory) that my-curator is connected,
   the path curator-skill/my-curator.md, and that the Curator app must be running.
After setup, summarise what you did in 5 bullets.
```

---

## 6.2 Excel — Spreadsheets

opencode doesn't write Excel natively; this adds it.

**EXCEL MCP PROMPT:**
```
Install the Excel MCP server (haris-musa/excel-mcp-server) for this opencode
project so Atlas can read, create, and update .xlsx files.

1. Make sure a Python runner (uv / uvx) is installed; if not, install it for me.
2. Register the Excel MCP in the project-level opencode.jsonc in THIS folder,
   using opencode's format EXACTLY:
       {
         "mcp": {
           "excel": {
             "type": "local",
             "command": ["uvx", "excel-mcp-server", "stdio"],
             "enabled": true
           }
         }
       }
   Merge into any existing "mcp" block rather than overwriting.
3. Confirm the MCP is available, then create data/task-log.xlsx with a header row
   (Date, Task, Owner, Due, Status, Source) to prove it works.
4. Record in AGENTS.md (Dependencies) that the Excel MCP is installed.
After setup, summarise what you did in 3 bullets.
```

---

## 6.3 Web Research — No account, no key

Lets Atlas fetch and read public pages for briefings and monitoring. The reference **Fetch** MCP needs no authentication.

**WEB RESEARCH MCP PROMPT:**
```
Install a no-auth web fetch MCP for this opencode project so Atlas can retrieve
and read public web pages for research and monitoring. Use the official reference
Fetch server (modelcontextprotocol/fetch, package "mcp-server-fetch", run via
uvx). If that package name has changed, find the current reference Fetch MCP and
use that — tell me which one you chose.

1. Make sure a Python runner (uv / uvx) is installed; if not, install it.
2. Register it in the project-level opencode.jsonc in THIS folder, opencode format:
       {
         "mcp": {
           "fetch": {
             "type": "local",
             "command": ["uvx", "mcp-server-fetch"],
             "enabled": true
           }
         }
       }
   Merge into any existing "mcp" block.
3. Verify it works: fetch a simple public page (e.g. https://example.com) and show
   me a one-line summary.
4. Record in AGENTS.md (Research Layer + Dependencies) that the web fetch MCP is
   installed, with its tool name(s).
After setup, summarise what you did in 3 bullets.
```

> Want richer web search (not just fetch-a-URL)? Ask Atlas: *"Is there a free, no-account web-search MCP for agents? If so, add it using the standard Conduit pattern."* Several exist; Atlas will pick one and register it the same way.

---

## 6.4 PDF Reading — contracts, decks, reports

Lets Atlas read large PDFs without loading the whole document into context — search by meaning, read only the pages that matter, extract tables.

**PDF MCP PROMPT:**
```
Install a PDF-reading MCP for this opencode project so Atlas can read, search, and
extract from large PDFs (contracts, reports, decks) without loading the whole file
into context. Use jztan/pdf-mcp (https://github.com/jztan/pdf-mcp); read its README
for the exact install/run command, then TRANSLATE that into opencode's project
"mcp" format in the project-level opencode.jsonc in THIS folder. If pdf-mcp is
unavailable, pick another reputable no-auth PDF MCP and tell me which.

Steps:
1. Install any runtime it needs (likely a Python runner — uv/uvx).
2. Register it in opencode.jsonc, opencode format (top-level "mcp", "type":
   "local", "command" as one array, "enabled": true). Merge, don't overwrite.
3. Verify: put any sample PDF in the documents/ folder, then have the MCP read it
   and give me a one-line summary.
4. Record in AGENTS.md (Research Layer + Dependencies) which PDF MCP you installed
   and its tool names.
After setup, summarise what you did in 3 bullets.
```

---

# 7. Optional Modules

Add these when you want them. They are more involved than the Section 6 tools, so they're separated out.

---

## 7.1 Meeting Intelligence (local, private transcription)

**The honest picture:** an MCP does not *listen live* to a call through the protocol. Instead, a **local, on-device** app transcribes your meetings and exposes an MCP so Atlas can pull the transcript **afterward** and turn it into minutes + action items. The recommended app is **[OpenWhispr](https://github.com/OpenWhispr/openwhispr)** — open-source, cross-platform, **100 % local** (audio never leaves your machine), auto-detects Zoom/Teams/Meet calls, and does speaker labelling.

> This module is an **app install + a model download**, so it's heavier than the prompt-only MCPs above. If you'd rather not install it, skip ahead — Atlas still processes any transcript or notes you **paste** or **drop into the `meetings/` folder**. You only lose the automatic capture, not the minutes.

**MEETING INTELLIGENCE PROMPT:**
```
Help me set up local, private meeting transcription and connect it to Atlas.

1. Install OpenWhispr (https://github.com/OpenWhispr/openwhispr) for my OS —
   download/build per its README. Explain each step. It runs 100% locally.
2. Walk me through enabling a local transcription model (Whisper / Parakeet) and
   turning on meeting transcription with speaker diarization, so my audio never
   leaves my machine.
3. OpenWhispr exposes an MCP server (see its Integrations/MCP docs). Get the exact
   MCP launch command from its docs, then register it in the project-level
   opencode.jsonc in THIS folder, TRANSLATED into opencode's "mcp" format
   (top-level "mcp", "type": "local", "command" as one array, "enabled": true).
   Merge, don't overwrite.
4. Verify: list or fetch a recent transcript through the MCP and show me the title.
5. Record in AGENTS.md (Meeting Intelligence Layer) the MCP tool names and where
   transcripts live.

After this, when I say "process the meeting", pull the latest transcript, produce
MEETING MINUTES per AGENTS.md, extract action items with owners and due dates into
tasks.md (and data/action-items.xlsx if Excel is installed), and offer to save a
summary to my Curator.
```

---

## 7.2 Atomic Mail — the agent's own mailbox

Gives Atlas its **own** mailbox — no human webmail login; the agent holds the credentials and is the only one who uses it. This is Atlas's outbound/notification channel (drafts you approve, follow-ups, digests), **separate from your real inbox**.

**ATOMIC MAIL PROMPT:**
```
Set up Atomic Mail for Atlas so it has its own agentic mailbox. This is an
agent-owned mailbox — there is no human inbox login; you (the agent) hold the
credentials and are the only one who accesses it. Work only in the project-level
opencode.jsonc in this folder.

1. Ask me what username I want for the agent's mailbox (e.g. a short handle like
   "petra-agent").
2. Register the mailbox by running:
       npx --package=@atomicmail/agent-skill atomicmail register --username "<the
       username I give you>"
   Capture the account it returns and store any credentials in the tool's own
   credential store / an environment variable — NEVER in AGENTS.md or any file.
3. Register the Atomic Mail MCP in opencode.jsonc, opencode format EXACTLY:
       {
         "mcp": {
           "atomicmail": {
             "type": "local",
             "command": ["npx", "-y", "@atomicmail/mcp"],
             "enabled": true
           }
         }
       }
   Merge into any existing "mcp" block.
4. Verify it works: run a JMAP "Mailbox/get" (or send yourself a short test message
   and read it back).
5. Record in AGENTS.md (Communication Layer + Dependencies): the mailbox handle you
   registered, and that Atomic Mail is installed. Do NOT record credentials.
6. Remind me of the rule: you never send an email without my explicit "confirm
   send", and you always show me the draft first.
After setup, summarise what you did in 5 bullets.
```

> ⚠️ Atlas **never** sends mail without your explicit "confirm send." That rule is in the mandate — keep it there.

---

## 7.3 Connect Your Real Inbox — Gmail / Outlook (Advanced)

To have Atlas triage **your own** inbox and prepare email reports, connect a Gmail or Outlook MCP.

> **Read this before you decide.** This is the highest-friction, highest-privacy-cost step in the guide, so it's optional and advanced:
> - It requires an **OAuth grant** through Google's/Microsoft's own consent screen. **You** complete that in your browser — Atlas never sees or enters your password (a hard rule in the mandate).
> - Start **read-only**. Let Atlas *summarise and draft*; keep *sending/archiving/rules* behind your explicit approval.
> - If you're privacy-sensitive, you may prefer to skip this and keep Atlas on the agent-owned Atomic Mail mailbox only.

**CONNECT REAL INBOX PROMPT:**
```
I want to connect my real email inbox to Atlas, read-only to start. Walk me through
it carefully and flag anything I need to approve myself.

1. Find a reputable, actively-maintained Gmail (or Outlook) MCP server that
   supports read + draft. Tell me which one you recommend and why, and what OAuth
   scopes it requests. Prefer read-only scopes to start.
2. Explain the OAuth step: I will complete the consent in my own browser; you will
   NOT ask for or enter my password. Confirm you understand this before proceeding.
3. Register the MCP in the project-level opencode.jsonc in THIS folder, TRANSLATED
   into opencode's "mcp" format. Put any client secret / token path in an
   "environment" block or the tool's own store — NEVER in AGENTS.md or any file.
4. After I complete the OAuth grant, verify with a safe read (e.g. list the last 5
   subject lines) — do not send, archive, or modify anything.
5. Record in AGENTS.md (Communication Layer): the provider, the scope (read-only?),
   and the tool names. Note that acting on the inbox (send/reply/archive/rules)
   requires my explicit approval each time.

Once connected, when I say "email report", summarise what's in my inbox: what needs
a reply, what's waiting on me, what can wait. Draft replies only for my approval.
```

---

# 8. Running Atlas — Basic Scenarios

Each scenario is a **copy-paste prompt**. Fill the `[BRACKETS]` and paste. These assume the Section 6 tools are installed; each says what it needs.

## Basic 1 — Capture a task
*Needs: nothing extra.*
```
New task: [what needs doing]. Owner: [me / a name]. Due: [date or "none"].
This came from: [a meeting / an email / just my request].
Log it per AGENTS.md and confirm what's now open.
```

## Basic 2 — Turn a meeting into minutes + action items
*Needs: nothing extra (paste the notes) — or the Meeting module for auto-capture.*
```
Here are my raw notes / the transcript from a meeting:
---
[PASTE NOTES OR TRANSCRIPT — or say: "read the latest transcript" if OpenWhispr is on]
---
Produce MEETING MINUTES per AGENTS.md: summary, decisions, action items with owners
and due dates, and open questions. File the action items into tasks.md (and
data/action-items.xlsx if Excel is on). Save the minutes to meetings/.
```

## Basic 3 — Brief me before a call
*Needs: Web Research (6.3); better with Curator (6.1).*
```
Brief me on [company / person / topic] before my meeting at [time].
Check my Curator second brain first, then the public web. Give me a RESEARCH BRIEF
per AGENTS.md: bottom line up top, key points with sources, what my second brain
already knew, and watch-outs. Keep it to one screen.
```

## Basic 4 — Read a document
*Needs: PDF Reading (6.4).*
```
Read the document at documents/[filename].pdf. Give me: a one-paragraph summary,
the 5 things I most need to know, and any dates, numbers, or obligations that
matter. If there's a table worth keeping, extract it into a new sheet in
data/[name].xlsx.
```

## Basic 5 — Weekly digest
*Needs: nothing extra; richer with the modules on.*
```
Produce my WEEKLY DIGEST per AGENTS.md for this week: what got done, meetings and
decisions, what's open or overdue, any research signals, and next week's plan.
Save it to reports/weekly/ and give me the highlights.
```

---

# 9. Advanced Scenarios

These chain modules together — the kind of thing that saves a professional real hours. Fill the `[BRACKETS]` and paste.

## Advanced 1 — Meeting → action items → follow-up drafts → second brain
*Needs: Meeting module (7.1) or pasted transcript · Curator (6.1) · Atomic Mail (7.2) optional.*
```
Process my last meeting end to end:
1. Read the latest transcript (or I'll paste it below).
2. Produce MEETING MINUTES and file action items with owners + due dates.
3. For each action item that needs an email to someone, draft the email — but do
   NOT send anything; show me the drafts for approval.
4. Save a short summary of the meeting's decisions to my [DOMAIN] Curator domain
   following the Curator skill (no broken links, no duplicate pages).
Report what you filed, what's drafted, and what's waiting on my approval.
---
[PASTE TRANSCRIPT HERE if OpenWhispr is not installed]
```

## Advanced 2 — Competitive / market monitor (recurring)
*Needs: Web Research (6.3) · Curator (6.1) · Excel (6.2) optional.*
```
Set up a monitoring routine for [company / market / topic]. Right now:
1. Do a research scan of the public web for anything new in the last [30] days.
2. Cross-check against what my Curator second brain already has, so you only flag
   what's genuinely new.
3. Give me a RESEARCH SCAN per AGENTS.md with a one-line signal I should care about.
4. Log the findings to data/research-log.xlsx and save anything important to my
   [DOMAIN] Curator domain.
Then tell me: what's the cleanest way for me to re-run this weekly? (e.g. I paste
"run the monitor" each Monday.)
```

## Advanced 3 — Document → structured data → briefing
*Needs: PDF Reading (6.4) · Excel (6.2) · Curator (6.1) optional.*
```
I've dropped [a contract / report / deck] at documents/[filename].pdf.
1. Read it with the PDF MCP.
2. Extract the key structured data (terms, dates, figures, parties — whatever fits)
   into a clean sheet in data/[name].xlsx.
3. Write me a one-page RESEARCH BRIEF on what it means for me and any risks or
   deadlines I must not miss.
4. Save the summary to my [DOMAIN] Curator domain.
Flag anything that needs a decision from me.
```

## Advanced 4 — Inbox triage → daily email report → draft replies
*Needs: Real inbox module (7.3).*
```
Give me my email report for today (read-only): what needs a reply, what's waiting
on me, and what can wait. For the [3] most important, draft replies in my voice —
do NOT send anything; show me the drafts. For anything that's really a task, add it
to tasks.md. Nothing leaves my inbox without my explicit "confirm send".
```

## Advanced 5 — Monday morning start-of-week brief
*Needs: whatever modules you have; degrades gracefully.*
```
It's Monday. Give me a start-of-week brief:
1. Run the Daily Status Check (open tasks, follow-ups, overdue).
2. Pull last week's WEEKLY DIGEST for context.
3. If the email module is on, add an inbox summary (read-only).
4. If the research module is on, run a quick scan on [my key topic] for weekend news.
5. End with a prioritised, numbered plan for my week — the 5 things that matter most.
Keep it to one screen. Don't take any external action without my approval.
```

---

# 10. FAQ

**Do I need Claude or a paid subscription?**
No. Atlas runs on opencode, which is free and open-source with free models built in — and works with local Ollama models if you want everything on your machine.

**Do I ever type terminal commands or edit config files?**
No. You paste prompts; Atlas installs tools, writes `opencode.jsonc`, and builds the folders. The one thing you do by hand is install opencode and create the top-level folder.

**Can Atlas really listen to my meetings live?**
Not through the MCP protocol. The Meeting module (7.1) uses a local app that transcribes on your device; Atlas turns the transcript into minutes afterward. Without it, paste or drop in a transcript and Atlas does the same thing.

**Can Atlas read my real Gmail/Outlook?**
Yes, via the optional advanced module (7.3), after **you** complete an OAuth grant in your browser. Atlas never enters your password. Start read-only. If you'd rather not, keep Atlas on its own Atomic Mail mailbox.

**Is my data private?**
Your files stay in your folder. The web and PDF MCPs are no-auth and local. The Curator second brain is a local app. Meeting transcription is 100 % on-device. The only steps that send data outward are the ones you explicitly approve (sending an email) or opt into (a hosted LLM model, or connecting your real inbox) — and you can run local models to minimise even that.

**How do I add a tool that isn't listed?**
Ask Atlas: *"Is there an MCP that can do [X]? If so, install it for this opencode project using the standard Conduit pattern and tell me what it can do."* Same pattern as everything in Section 6.

**Can I run more than one Atlas?**
Yes — one folder per agent. Because MCPs are registered in the project `opencode.jsonc`, each agent keeps its own tools. Build a personal one and a work one if you like.

**What if a tool is unavailable during a session?**
Atlas will say so and continue with the tools it has. Re-run the relevant install prompt to fix it (check the Curator app is running for second-brain features).

---

*Atlas — Professional Assistant Agent · Guide v1.0 · Conduit Use Case #2*
*opencode-native · prompt-driven · privacy-first. Built on the [Conduit framework](../../README.md).*

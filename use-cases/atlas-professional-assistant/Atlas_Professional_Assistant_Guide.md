# Atlas — Professional Assistant Agent · Setup & Operations Guide

*Conduit Use Case #2 · Guide v2.0 · September 2026*
*A general-purpose execution assistant for professionals — built on [Conduit](../../README.md), running on opencode (free & open-source).*

> **New to Conduit?** Read [docs/00-start-here.md](../../docs/00-start-here.md) and [the concepts](../../docs/01-concepts.md) first. This guide assumes you understand the basics and focuses on building **Atlas**.

> **What's new in v2.0.** Atlas is now also your **coach** — it explains each step as it builds itself, and you can say *"coach me"* at any time. The guide follows a five-level **learning path**. The Curator section covers the new **Mac app** as well as the browser app, and installs The Curator's two official **skills** automatically. New tools: **Documents** (Word, PowerPoint, Excel, PDF), free **web search**, and a read-only **Calendar** module. Already running Atlas v1? See [§7.5 Upgrade from Atlas v1](#75-already-running-atlas-v1-upgrade).

---

## How this guide works (read this first)

Atlas is **prompt-driven**. You don't edit config files, run terminal commands, or wire up tools by hand. You **copy a prompt, paste it into your agent, and the agent builds itself** — folders, memory, and every tool (MCP). That is the whole point of the Conduit framework: you describe what you want in plain language, and the agent does the engineering.

**And it teaches you while it works.** Atlas runs in **Coach Mode** by default: before each step it tells you what it is about to do and why, afterwards it shows you how to tell it worked, and it explains each new idea the moment it appears. If you ever feel lost, paste **`coach me`** — Atlas shows where you are and the one next step.

Every grey box below is a **copy-paste prompt**. Fill any `[BRACKETS]`, paste it into opencode, and follow along.

### Who does what

| **You do** (a few clicks) | **Atlas does** (all the technical work) |
|---|---|
| Install opencode and create one folder | Creates every other folder and file |
| Paste prompts and answer Atlas's questions | Installs software, writes `opencode.jsonc`, downloads skills |
| Approve security prompts on your own computer (e.g. macOS "Open Anyway") | Tells you exactly which button to click, and waits |
| Paste API keys **into the app that needs them** — never into the chat | Never asks for a key, password or token |
| Quit and reopen opencode when Atlas asks | Checks everything works afterwards and shows you the result |

### The learning path

Setting up Atlas is also a short course in how AI agents really work. Each level teaches one idea by doing it.

| Level | You learn | Sections | Time |
|---|---|---|---|
| **1 · Your agent** | What an **agent harness** is, and how `AGENTS.md` becomes your agent's **mandate** | [2–5](#2-level-1--install-opencode) | ~25 min |
| **2 · Tools (MCPs)** | What an **MCP** is — by giving Atlas the power to read documents, search the web and write spreadsheets | [6](#6-level-2--tools-your-first-mcps) | ~20 min |
| **3 · Your second brain** | How a **second brain** (The Curator) gives your agent *your* context — and compounds | [7](#7-level-3--your-second-brain-the-curator) | ~30 min |
| **4 · A shared brain** *(optional)* | How a team or class builds **one brain together** | [8](#8-level-4--a-shared-brain-optional) | ~20 min |
| **5 · Context management** *(optional)* | How any agent, on any computer, can **continue your work** instead of starting cold | [9](#9-level-5--context-management-optional) | ~15 min |
| **Modules** *(optional)* | Calendar, meeting transcription, email | [10](#10-optional-modules) | as needed |

You can stop after Level 3 and have a genuinely useful assistant. Levels don't have to be done in one sitting — Atlas remembers where you are.

### Before you start — checklist

- [ ] A Mac, Windows or Linux computer, with permission to install apps
- [ ] About **75 minutes** for Levels 1–3 (splitting it over two sessions is fine)
- [ ] A Google account, for a free **Gemini API key** (Level 3) — or an Anthropic / OpenRouter key if you prefer
- [ ] *Level 4 only:* a free **GitHub** account and an **invite** from whoever runs your Shared Brain (e.g. your instructor)
- [ ] A couple of real work documents to try things on (a report, a deck, meeting notes)

---

# 1. What You Are Building

**Atlas** is an execution assistant for a working professional — a CEO, founder, manager, consultant, or individual contributor. It is **agnostic**: not a personal-life bot, not a single-purpose business bot. It captures and tracks your tasks, turns meetings into action items, researches on demand, reads your documents, keeps a compounding second brain, and prepares reports and drafts — always inside a clear mandate, always keeping you in control of anything that leaves your machine.

### The layers

| Layer | What it does | Status |
|---|---|---|
| 🗂️ **Tasks** | Capture and follow up on your work (`tasks.md`) | Always on |
| 🧠 **Memory & Context** | `memory.md` (working memory) + The Curator (context layer / second brain) | Always on |
| 📄 **Documents** | Read Word, PowerPoint, Excel and PDF files | Recommended |
| 🔎 **Research** | Web search + reading public pages | Recommended |
| 📊 **Data** | Excel workbooks for logs and reports | Recommended |
| 📅 **Calendar** | Read your schedule (read-only) | Optional |
| 🎙️ **Meetings** | Local, private transcription → minutes + action items | Optional |
| 📧 **Communication** | Agent-owned mailbox + optional real inbox | Optional |

### What Atlas does day to day
- Turns a meeting recording or notes into **minutes + action items** with owners and due dates
- Prepares a **research brief** before your next call ("brief me on Company X")
- Reads a **contract, board deck or spreadsheet** and pulls out what matters
- Keeps your **tasks and follow-ups** so nothing slips
- Compiles a **weekly digest** of what got done, what's open, what's next
- Drafts **emails and reports** for your approval

---

# 2. Level 1 — Install opencode

*The only thing you install completely by hand.*

**opencode** is a free, open-source **agent harness**: the program that turns an AI model into an agent that can act on your computer. It reads your `AGENTS.md`, works with the files in your folder, connects to MCP servers, installs software, and writes config files for you. *Think of the model as the engine and opencode as the car built around it.*

### Steps
1. Go to **[opencode.ai](https://opencode.ai)** and download the app for your computer (macOS, Windows, Linux).
2. Install it (about two minutes).
3. On first launch, **pick a model**. opencode always offers free models — no subscription or API key needed to start.

| **Free, private — and honest about it** | opencode is free and open-source. Its free hosted models are great for learning, but check the note next to each one: some providers may use prompts from free models to improve their service, so **don't put confidential company data through a free model.** For confidential work, use a paid model you trust, or a **local model via [Ollama](https://ollama.com)** that runs entirely on your computer (point opencode at it in the model picker). |
| --- | --- |

> You do **not** install Node.js, `uv`, or any MCP by hand. When a tool is needed, the install prompt tells Atlas to check for it and install it if it's missing.

---

# 3. Level 1 — Create Your Project Folder

Each agent lives in its own folder. You create **one folder** by hand and drop `AGENTS.md` inside it — the Setup Prompt builds the rest.

### Steps
1. Make a folder with a clear name, e.g. `atlas-assistant/` (or `petra-assistant/`), somewhere easy to find such as your Documents folder.
2. Download **[AGENTS.md](https://raw.githubusercontent.com/talirezun/conduit-agent/main/use-cases/atlas-professional-assistant/AGENTS.md)** (right-click → *Save Link As…*) into that folder. Keep the name exactly `AGENTS.md`.
3. Do **not** create subfolders yourself — the Setup Prompt does that.
4. In opencode, open the folder (**File → Open Folder**, or drag the folder in). You should see it as your working directory.

> **Why a file called `AGENTS.md`?** It is your agent's job description and rulebook in one: who it works for, what it may do, what it must never do, and what it must ask about first. opencode reads it automatically every session. In Conduit we say: **the config *is* the mandate.**

> **A note on MCP config (opencode).** opencode registers tools (MCP servers) in a **project-level** `opencode.jsonc` inside your folder — not the global `~/.config/opencode` config. This keeps each agent's tools scoped to its own folder, so different agents don't share tools. Every prompt in this guide already targets the project file.

---

# 4. Level 1 — The Setup Prompt: Let Atlas Build Everything

Paste this into opencode. Atlas reads `AGENTS.md`, builds its structure, and starts coaching you.

**SETUP PROMPT — copy & paste:**
```
Read the AGENTS.md file in this project folder carefully — including the Coach Mode
section. I am not technical: coach me through this setup as AGENTS.md describes,
one step at a time, in plain language. Then do the following:

1. Introduce yourself in three sentences: who you are, what we're about to build
   together, and the five-level learning path.

2. Create this folder structure inside the current project folder:
   reports/weekly/   meetings/   briefs/   documents/   data/

3. Create tasks.md in the project root with this starting content:
   # Tasks
   (no open tasks yet — add with "new task")

4. Create memory.md in the project root with this starting content:
   Last session: [today's date]
   Last action: Setup complete
   Open tasks: 0
   Unresolved items: None
   Notable events: Atlas configured and ready
   Learning path: L1 in progress · L2 – · L3 – · L4 – · L5 –
   Coach mode: on
   Next planned action: Fill my Professional Profile

5. Check whether the tools you will need later are installed: Node.js (version 20
   or newer) and the Python runner "uv" (which provides uvx). If any are missing,
   install them for me and tell me what you installed. Do not ask me to run
   terminal commands — you handle it. If my computer asks for my password or an
   approval, tell me what the prompt will look like and why it is safe.

6. If you create or edit an MCP configuration file, edit the PROJECT-LEVEL
   opencode.jsonc in THIS folder (never the global ~/.config/opencode config),
   using opencode's "mcp" format.

7. Whenever you install or configure anything future sessions will need to know
   about, append a short note to the relevant section of AGENTS.md. Never write
   secrets (API keys, tokens, passwords) into AGENTS.md.

8. Explain, in plain language, what an "agent harness" is and what AGENTS.md
   and memory.md do — using what we just did as the example. Then run the Daily
   Status Check described in AGENTS.md and tell me the next step.
```

**You'll know it worked when:** you can see the new folders and files in your project folder, and Atlas has shown you an **ATLAS STATUS** block.

---

# 5. Level 1 — Make It Yours

Atlas needs to know who you are and what you do. Two ways: let it interview you (recommended), or edit `AGENTS.md` by hand.

### Option A — Let Atlas fill your profile (recommended)

**FILL MY PROFILE PROMPT — copy & paste:**
```
Open the AGENTS.md file in this project folder. You are going to help me fill in
the Professional Profile and tune the Mandate. Interview me ONE question at a
time — do not flood me with questions. Before we start, explain in two sentences
why the Mandate matters (what it lets you do, forbids, and makes you ask about).

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
in AGENTS.md, replacing the placeholders, and delete the "HOW TO FILL THIS FILE"
comment block. Show me the result and ask me to confirm before saving. Do not
invent details — if something is unclear, ask. When it's saved, mark Learning
path Level 1 as done in memory.md and tell me what I've learned so far.
```

### Option B — Fill it by hand
Open `AGENTS.md`, replace every `[PLACEHOLDER]` in the **Professional Profile**, and add your own items to the **Mandate** lists. Save.

> No business problem in mind yet? Use one of the [scenarios](scenarios/README.md) to fill your profile and try Atlas out.

🎓 **Level 1 complete.** You've built an agent from a single file, and you've seen that its behaviour is governed by a written mandate you control. Try it: *"Check my mandate: can you send an email to my team for me?"*

---

# 6. Level 2 — Tools: Your First MCPs

An **MCP** (Model Context Protocol) is a standard plug that gives an agent a new ability. *Like apps on a phone: the phone stays the same, each app adds a capability.* Each MCP Atlas installs is registered in your folder's `opencode.jsonc` — the list of plugs this agent is allowed to use.

Install what you want, in any order. Each is a copy-paste prompt; Atlas does the rest, tests it on something real, and records what it did in `AGENTS.md`. All snippets are already in **opencode format**.

> **After each install, Atlas will ask you to quit and reopen opencode** (or start a new session). That is how opencode loads a newly added tool. It's normal.

> **The generic pattern.** Any MCP not covered here can be added the same way — just ask: *"Is there a free, reputable MCP that can do [X]? If so, explain what it does and what data it can reach, then install it for this opencode project using the standard Conduit pattern (project opencode.jsonc, opencode's `mcp` format), verify it, and tell me what it can do."*

---

## 6.1 Documents — Word, PowerPoint, Excel, PDF

Executives live in documents. **[MarkItDown](https://github.com/microsoft/markitdown)** is a free, open-source (MIT) tool from Microsoft that turns Word, PowerPoint, Excel and PDF files — and web pages — into text Atlas can read. It runs on your computer; no account, no key.

**DOCUMENTS MCP PROMPT:**
```
Install Microsoft's MarkItDown MCP for this opencode project so you can read my
Word, PowerPoint, Excel and PDF files. Coach me through it in plain language.

1. Make sure the Python runner "uv" (uvx) is installed; if not, install it.
2. Register it in the project-level opencode.jsonc in THIS folder, in opencode's
   format EXACTLY (merge into any existing "mcp" block, never overwrite):
       "documents": {
         "type": "local",
         "command": ["uvx", "markitdown-mcp"],
         "enabled": true,
         "timeout": 60000
       }
   (The first start downloads its components and can take about a minute; the
   longer timeout covers that. You can also run "uvx markitdown-mcp --help" once
   now to pre-download it.)
3. Tell me to quit and reopen opencode. After I do, verify it: its tool is
   convert_to_markdown, and it needs a file:// URI with the FULL path (e.g.
   file:///Users/me/atlas/documents/report.docx — on Windows
   file:///C:/Users/me/atlas/documents/report.docx). Ask me to drop any real
   document into the documents/ folder, then read it and give me a 5-bullet
   summary.
4. Record in AGENTS.md (Documents Layer + Dependencies) that the Documents MCP
   (MarkItDown) is installed, its tool name, and the file:// rule.
5. Explain what an MCP is, using this one as the example, and what opencode.jsonc
   now contains. Then summarise what you did in 3 bullets.
```

> **Honest limits:** scanned PDFs (images of pages, no real text) come back empty — MarkItDown does not do OCR by default. Very long PDFs are read whole; for those, add the optional [Large PDFs](#64-large-pdfs-optional) tool.

---

## 6.2 Web Search — no account, no key

opencode can already **read** a web page you point it at (its built-in `webfetch` tool). This adds **search**, so Atlas can find the pages worth reading. **[Exa](https://exa.ai)** offers a hosted MCP that needs no installation and no key for light use.

**WEB SEARCH MCP PROMPT:**
```
Add free web search to this opencode project. Coach me through it in plain
language.

1. Register Exa's hosted search MCP in the project-level opencode.jsonc in THIS
   folder, opencode format EXACTLY (merge into any existing "mcp" block):
       "websearch": {
         "type": "remote",
         "url": "https://mcp.exa.ai/mcp",
         "enabled": true,
         "oauth": false
       }
   Nothing needs installing — it runs on Exa's servers.
2. Tell me to quit and reopen opencode. After I do, verify it: search for
   "[a company or topic I care about]" and give me the three most relevant
   results with one line each and their links.
3. Explain the privacy trade-off in one sentence: my search queries go to Exa;
   the documents on my computer do not.
4. If Exa is unavailable or starts refusing requests (free use is rate-limited),
   switch to the local DuckDuckGo fallback instead, same entry name:
       "websearch": {
         "type": "local",
         "command": ["uvx", "--with", "duckduckgo-mcp-server[browser]",
                     "duckduckgo-mcp-server"],
         "enabled": true,
         "timeout": 60000
       }
   and tell me you switched.
5. Record in AGENTS.md (Research Layer + Dependencies) which search MCP is
   installed and its tool names. Summarise what you did in 3 bullets.
```

> **Using opencode's free "Zen" models?** opencode then also has a built-in web search tool (it uses Exa too). The MCP above works with *any* model, which is why the guide installs it.

---

## 6.3 Excel — Spreadsheets

opencode doesn't write Excel natively; this adds it.

**EXCEL MCP PROMPT:**
```
Install the Excel MCP server (haris-musa/excel-mcp-server) for this opencode
project so you can read, create, and update .xlsx files. Coach me through it.

1. Make sure the Python runner "uv" (uvx) is installed; if not, install it for me.
2. Register the Excel MCP in the project-level opencode.jsonc in THIS folder,
   using opencode's format EXACTLY (merge into any existing "mcp" block):
       "excel": {
         "type": "local",
         "command": ["uvx", "excel-mcp-server", "stdio"],
         "enabled": true,
         "timeout": 60000
       }
3. Tell me to quit and reopen opencode. After I do, create data/task-log.xlsx
   with a header row (Date, Task, Owner, Due, Status, Source) to prove it works,
   and tell me where to find it.
4. Record in AGENTS.md (Dependencies) that the Excel MCP is installed.
After setup, summarise what you did in 3 bullets.
```

---

## 6.4 Large PDFs (optional)

MarkItDown reads a PDF whole. For very long documents (a 300-page annual report), a PDF MCP that searches and reads **page by page** keeps Atlas fast and focused.

**LARGE PDFs MCP PROMPT:**
```
Install a PDF-reading MCP for this opencode project so you can search and read
very large PDFs page by page instead of loading the whole file. Use jztan/pdf-mcp
(https://github.com/jztan/pdf-mcp); read its README for the exact install/run
command, then TRANSLATE that into opencode's project "mcp" format in the
project-level opencode.jsonc in THIS folder (top-level "mcp", "type": "local",
"command" as one array, "enabled": true, "timeout": 60000). Merge, don't
overwrite. If pdf-mcp is unavailable, pick another reputable no-account PDF MCP
and tell me which.
Then tell me to quit and reopen opencode, verify it on a PDF in documents/, and
record in AGENTS.md (Documents Layer + Dependencies) which PDF MCP you installed
and its tool names. Summarise in 3 bullets.
```

🎓 **Level 2 complete** once Atlas has used one of these tools on a real task. You now know what an MCP is and how an agent's abilities are added — and limited — by a list in one config file. Try it: *"Read [a document in documents/] and brief me on it in five bullets."*

---

# 7. Level 3 — Your Second Brain: The Curator

This is the heart of Atlas. **[The Curator](https://github.com/talirezun/the-curator)** is its **context layer**: a free, open-source app that turns your documents and notes into a **knowledge graph** — a second brain — that Atlas can search and grow across sessions. A model on its own knows nothing about *you*; The Curator is how you give your agent your context, and it **compounds**: every source you add makes the next task better. → [Why The Curator matters](../../docs/05-the-curator.md)

It takes three steps: **get the app → do its first run → connect it to Atlas.**

## 7.1 Get the app

Pick one:

### Option 1 — The Mac app (recommended on a Mac)

This is a download you do yourself — about two minutes. Atlas should not do it for you, because macOS will ask *you* to approve the app.

1. Open the **[Releases page](https://github.com/talirezun/the-curator/releases)** and, under the newest release, download the file for your Mac:
   - Apple Silicon (M1 or later): `TheCurator-<version>-arm64-AppleSilicon.dmg`
   - Intel: `TheCurator-<version>-x64-Intel.dmg`
   *(Not sure? Apple menu → **About This Mac** → look at "Chip".)*
2. Open the downloaded `.dmg` and drag **The Curator** onto **Applications**.
3. **First launch only:** double-click The Curator in Applications. macOS blocks it, because the app isn't notarized by Apple yet (the developer's enrolment is in progress) — dismiss the message. Then go to **System Settings → Privacy & Security**, scroll down to **Security**, and click **Open Anyway**. Confirm (your Mac password may be needed), and open the app again. macOS remembers your choice.

From then on the app updates itself (**The Curator → Check for Updates…**). → [Mac app guide](https://github.com/talirezun/the-curator/blob/main/docs/mac-app.md)

> 🧭 **Want Atlas to walk you through the clicks?** Paste: *"Coach me through installing The Curator Mac app. I'll do the clicks — give me one step at a time and wait for me to say done."*

### Option 2 — The browser app (Windows, Linux — or a Mac, if you prefer)

Same program, running as a local web page in your browser. Atlas installs it.

**CURATOR — GET THE BROWSER APP PROMPT:**
```
Please install "The Curator" (browser app) on this machine for me. Coach me
through it in plain language.
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
6. Tell me, in one simple instruction, how to start it again next time.

Do not edit any files outside the-curator folder. Do not ask me for an API key —
I add it in the app myself. After install, summarise what you did in 5 bullets
and note in AGENTS.md that The Curator (browser app) is installed at
~/the-curator. Do NOT connect the MCP yet — that comes after the first run.
```

## 7.2 First run — in the app (you do this)

When The Curator opens for the first time, a **Getting started** panel asks what you want to set up first. Choose **Build a second brain** and follow its three steps:

1. **Add an AI key.** The Curator uses an AI model to organise your documents into the graph. The easiest start is a free **Google Gemini** key:
   - Go to **[aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)**, sign in with Google, click **Create API key**, and copy it.
   - Paste it into The Curator where the panel asks. **Never paste it into the opencode chat** — Atlas doesn't need it and must never see it.
   - *(Anthropic and OpenRouter keys also work. The free Gemini tier is fine to learn with; for regular use, turning on billing in Google AI Studio typically costs a few euros a month. → [API keys & cost](https://github.com/talirezun/the-curator/blob/main/docs/user-guide.md#4-get-your-api-key-gemini-claude-or-openrouter))*
2. **Create your first domain.** A domain is one topic area of your brain. Start with `work` (and later perhaps `personal`) — keeping them separate keeps the graph clean.
3. **Ingest your first source.** Drop in a real document — a strategy paper, a report, your notes. Watch it become pages of people, companies and ideas, linked together.

> **What leaves your computer?** Your second brain is stored as plain text files on your own machine — no account, no cloud database. When you **ingest** a document, its text is sent to the AI provider whose key you added, so it can be organised. Searching and reading your brain — including everything Atlas does through the MCP — never calls a model.

## 7.3 Connect The Curator to Atlas — MCP + skills

Now Atlas gets two things: the **my-curator MCP** (the *tools* to search and write your second brain — 24 of them) and The Curator's two official **skills** (the *rules* for using them well: `my-curator` for reading and writing the wiki cleanly, and `curator-continuity` for carrying work between sessions). Atlas works out which install you have and does the rest.

**CURATOR — CONNECT PROMPT:**
```
The Curator is installed and I have created my first domain. Connect it to THIS
opencode project, and coach me through it in plain language. Work only in the
project-level opencode.jsonc in this folder (not the global ~/.config/opencode
config).

1. Work out which install I have:
   - Mac app: the file
       ~/Library/Application Support/The Curator/bin/my-curator-mcp
     exists (expand ~ to my full home path). The app writes it every time it
     starts.
   - Browser app: ~/the-curator/mcp/server.js exists.
   If neither exists, stop and tell me (for the Mac app: open The Curator once,
   then try again).
2. Register the my-curator MCP in opencode.jsonc, in opencode's format EXACTLY,
   with ABSOLUTE paths. Merge into any existing "mcp" block — never overwrite it.
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
   Each file is at <Base>/<skill-name>/<file-name>. Check each download is a real
   markdown file (not an error page). opencode finds skills in .agents/skills/
   automatically — no config entry is needed.
4. Tell me to quit and reopen opencode. After I do, verify: call the my-curator
   list_domains tool, show me my domains, and confirm both skills are available.
5. Record in AGENTS.md (Memory & Context Layer): that my-curator is connected,
   which install I have (Mac app or browser app), that the skills live in
   .agents/skills/, my domain names, and that the MCP works even when the Curator
   app is closed.
6. Explain the difference between the MCP (tools) and a skill (rules), and
   between memory.md and The Curator. Summarise what you did in 5 bullets.
```

> 💡 **Good to know:** the MCP reads your knowledge folder directly, so it **works even when The Curator app is closed**. You only need the app open to ingest documents, chat with your brain, or change settings.

## 7.4 First real use — ask your second brain

**FIRST QUESTION PROMPT:**
```
Using my Curator second brain (follow the my-curator skill): what do I know about
[a topic from the document I ingested]? Show me which pages you used. Then
explain in two sentences how this differs from asking a model that has never
seen my documents.
```

Then close the loop — let Atlas **write** to the brain too:

```
Save the three most useful takeaways from our session today to my [work] Curator
domain, following the my-curator skill (check what already exists first — no
duplicate pages, no broken links). Show me what you'll write before you save it.
```

🎓 **Level 3 complete.** Your agent now works from *your* context, and every document you ingest makes it better. Keep feeding it: after meetings and research, say *"save to Curator"*.

## 7.5 Already running Atlas v1? Upgrade

Atlas v1 used a Conduit-hosted copy of The Curator's skill (`curator-skill/my-curator.md`), which is now retired: The Curator has grown from 17 to 24 tools and ships its own official skills. This prompt upgrades an existing Atlas without losing your profile, mandate or notes.

**UPGRADE TO ATLAS v2 PROMPT:**
```
Upgrade this Atlas from v1 to v2. Coach me through it, and show me every change
before you make it.

1. Back up: copy AGENTS.md to AGENTS.v1-backup.md and opencode.jsonc to
   opencode.v1-backup.jsonc.
2. Download the v2 AGENTS.md from
   https://raw.githubusercontent.com/talirezun/conduit-agent/main/use-cases/atlas-professional-assistant/AGENTS.md
   to AGENTS.v2.md. Carry over EVERYTHING that is mine from my current AGENTS.md:
   my Agent Identity, Professional Profile, my own Mandate additions, and every
   note you recorded about installed tools. Show me the merged result; after I
   confirm, replace AGENTS.md with it and delete AGENTS.v2.md.
3. In opencode.jsonc: remove "curator-skill/my-curator.md" from "instructions"
   (and remove "instructions" entirely if it is then empty). Then run the
   "CURATOR — CONNECT PROMPT" steps 1–3 from the Atlas guide v2, section 7.3:
   https://github.com/talirezun/conduit-agent/blob/main/use-cases/atlas-professional-assistant/Atlas_Professional_Assistant_Guide.md
   (if I have since installed the Curator Mac app, switch my-curator to the Mac
   app launcher). Add "timeout": 60000 to every uvx/npx MCP entry.
4. After I confirm, delete the old curator-skill/ folder.
5. Add these two lines to memory.md: "Learning path: L1 ✓ · L2 [✓ if an MCP is
   installed] · L3 [✓ if the Curator is connected] · L4 – · L5 –" and
   "Coach mode: on".
6. Tell me to quit and reopen opencode, then run "check my setup".
```

---

# 8. Level 4 — A Shared Brain (optional)

A **Shared Brain** is a second brain a team — or a class — builds together. Everyone keeps working in their own private domain. When they choose, they **push contributions** (The Curator distils them first); the admin synthesises everyone's contributions into one collective wiki; and everyone **pulls** it back as a read-only `shared-…` domain that their agents can search. Nothing from your personal domains is shared unless you opt that domain in.

It is an opt-in **beta** in The Curator, built on a private GitHub repository. → [Shared Brain user guide](https://github.com/talirezun/the-curator/blob/main/docs/shared-brain-user-guide.md)

**What you need:** a free **GitHub** account, an **invite token** from your Shared Brain's admin (e.g. your instructor), and to have accepted the admin's GitHub **collaborator invitation** (it arrives by email).

The setup happens in The Curator's own wizard, and some steps involve tokens — so **you do the clicks and Atlas coaches**:

**JOIN A SHARED BRAIN PROMPT:**
```
I want to join my team's / class's Shared Brain in The Curator as a contributor.
I have my invite token from the admin. Coach me through it one step at a time,
using The Curator's Shared Brain user guide as your source:
https://raw.githubusercontent.com/talirezun/the-curator/main/docs/shared-brain-user-guide.md
(read it first).

Rules: I do every click and paste in The Curator app myself. NEVER ask me to paste
my invite token or my GitHub token into this chat, and never try to handle them.
Explain each step before I do it, tell me what I should see, and wait for me to
say "done".

Cover: enabling Shared Brain (Domains → a domain → Shared Brain → Open Shared
Brain → Enable Shared Brain), creating the GitHub access token the wizard asks
for (explain what permission it grants and why), choosing which of my domains to
contribute from, and my first Pull. When the shared domain appears, call
list_domains to confirm you can see it, and remind me it is read-only for you.
```

**Try it:**
```
Search our Shared Brain domain for [a topic the group has worked on]. What does
the group know that my own domain doesn't? Cite the pages.
```

🎓 **Level 4 complete** when Atlas has read from the `shared-…` domain. You've seen how a team's knowledge becomes *every* member's agent's context — without anyone handing over their private notes.

> **Just want a backup, not a team brain?** The Curator can also **sync** your own brain to a private GitHub repository, so it's backed up and available on your other computers. → [Sync guide](https://github.com/talirezun/the-curator/blob/main/docs/sync.md)

---

# 9. Level 5 — Context Management (optional)

`memory.md` helps Atlas continue in *this* folder. **Context management** goes further: The Curator can hold the **working state** of a piece of work — a **standing brief** (what it is and the rules) and the latest **handoff** (where things stand, what's next) — so that **any** agent, in any tool, on any computer, can pick it up. This is how professionals running several AI tools stop re-explaining themselves. → [Working state guide](https://github.com/talirezun/the-curator/blob/main/docs/working-state.md)

**Step 1 — in The Curator (you):** open **Domains**, pick your domain, and under **Projects** create a project (e.g. `atlas`). Optionally write a short standing brief. Then press **Copy agent instructions** — it copies a short block of text with your project's name filled in.

**Step 2 — paste it to Atlas:**

**CONNECT A CURATOR PROJECT PROMPT:**
```
I created a project in The Curator for this work. Here are the agent instructions
it gave me:
---
[PASTE THE BLOCK FROM "Copy agent instructions"]
---
Coach me through this. Insert that block near the top of AGENTS.md (just below
the title), exactly as given. Follow the curator-continuity skill from now on:
load the project context at the start of every session, and save your working
state (scope "opencode") after each material step and before we stop. Keep
updating memory.md too. Explain in plain language what a standing brief and a
handoff are, then save a first handoff now and show me what you saved.
```

**Step 3 — see it work:** quit opencode, reopen it, and say *"continue"*. Atlas loads the handoff and picks up where you left off. Open The Curator's **Context** view to see the same handoff there — and any other agent you connect (Claude, for example) can read it too.

🎓 **Level 5 complete.** You've gone from "my agent remembers my knowledge" to "any of my agents can continue my work" — the discipline of managing context, not just storing it.

---

# 10. Optional Modules

Add these when you want them. They are more involved than the Level 2 tools, so they're separated out.

---

## 10.1 Calendar — read-only

Lets Atlas see your schedule: today's meetings in the Daily Status Check, briefings before important calls, a week-ahead view on Mondays. **Read-only to start** — creating or changing events always needs your approval (it's in the mandate).

There are three free routes. Atlas picks the right one with you:

| You use… | Route | What you do yourself |
|---|---|---|
| **A Mac** (any calendar you've added to the Calendar app — iCloud, Google, Exchange) | Apple Calendar MCP — reads the Calendar app on your Mac, no online login | Allow calendar access once, when macOS asks |
| **Outlook / Microsoft 365** (Windows or Mac) | Microsoft 365 MCP in **read-only calendar** mode | Sign in once in your browser (a Microsoft code page) |
| **Google Calendar on Windows/Linux**, or anything else | A private calendar feed link (iCal / ICS) — read-only by design | Copy your calendar's private iCal address and give it to Atlas's config |

**CALENDAR MODULE PROMPT:**
```
Connect my calendar to Atlas, read-only. Coach me through it one step at a time.
First ask me which computer I use and which calendar service, then use the
matching route below. Register the MCP in the project-level opencode.jsonc in
THIS folder, opencode format, merged into any existing "mcp" block, under the
entry name "calendar".

Route A — Mac (reads the Calendar app; needs Node.js 20+):
    "calendar": {
      "type": "local",
      "command": ["npx", "-y", "mcp-server-apple-events"],
      "enabled": true,
      "timeout": 60000
    }
  This server can also create and edit events and reminders. The mandate says
  calendar changes need my approval — always ask before any create/update/delete.
  The first time you read events, macOS asks for calendar access: tell me to
  choose Full Access (System Settings → Privacy & Security → Calendars) and why.

Route B — Outlook / Microsoft 365 (read-only by construction):
    "calendar": {
      "type": "local",
      "command": ["npx", "-y", "@softeria/ms-365-mcp-server",
                  "--preset", "calendar", "--read-only"],
      "enabled": true,
      "timeout": 60000
    }
  If it's a work or school account, add "--org-mode" to the command. To sign in,
  use its login tool: it gives me a web address and a code — I open it and sign
  in myself; you never see my password. (Some companies require IT to approve
  new apps; if sign-in is blocked, tell me that plainly.)

Route C — private calendar feed (ICS), read-only:
  Walk me through finding my calendar's secret iCal address (Google Calendar:
  Settings → my calendar → "Secret address in iCal format"; Outlook.com:
  Settings → Calendar → Shared calendars → Publish a calendar → ICS link).
  That link lets anyone read my calendar, so it is a secret: do NOT write it into
  AGENTS.md, opencode.jsonc or any file in this folder. Instead ask me to paste it
  into a new file outside this folder that you create for me (e.g.
  ~/.config/atlas/calendar-ics-url, readable only by me, containing ONLY the
  link on one line with no trailing blank line), then register:
    "calendar": {
      "type": "local",
      "command": ["uvx", "--with", "mcp<2", "ics-calendar-mcp"],
      "environment": {
        "ICS_CALENDAR_URL": "{file:<ABSOLUTE path to that file>}",
        "ICS_TIMEZONE": "<my timezone, e.g. Europe/Zagreb>"
      },
      "enabled": true,
      "timeout": 60000
    }
  (This is a small community project; if it stops working, tell me and we'll
  choose another route.)

Then: tell me to quit and reopen opencode, verify by listing my meetings for
today and tomorrow, and record in AGENTS.md (Calendar Layer + Dependencies) which
route is connected, which calendars, read-only or not, and the tool names —
never the feed link or any token.
```

---

## 10.2 Meeting Intelligence (local, private transcription)

**The honest picture:** an MCP does not *listen live* to a call through the protocol. Instead, a **local, on-device** app transcribes your meetings and exposes an MCP so Atlas can pull the transcript **afterward** and turn it into minutes + action items. The recommended app is **[OpenWhispr](https://github.com/OpenWhispr/openwhispr)** — open-source, cross-platform, **100 % local** (audio never leaves your machine), auto-detects Zoom/Teams/Meet calls, and does speaker labelling.

> This module is an **app install + a model download**, so it's heavier than the prompt-only MCPs above. If you'd rather not install it, skip ahead — Atlas still processes any transcript or notes you **paste** or **drop into the `meetings/` folder**. You only lose the automatic capture, not the minutes.

**MEETING INTELLIGENCE PROMPT:**
```
Help me set up local, private meeting transcription and connect it to Atlas.
Coach me through it one step at a time.

1. Install OpenWhispr (https://github.com/OpenWhispr/openwhispr) for my OS —
   download/build per its README. Explain each step. It runs 100% locally.
2. Walk me through enabling a local transcription model (Whisper / Parakeet) and
   turning on meeting transcription with speaker diarization, so my audio never
   leaves my machine.
3. OpenWhispr exposes an MCP server (see its Integrations/MCP docs). Get the exact
   MCP launch command from its docs, then register it in the project-level
   opencode.jsonc in THIS folder, TRANSLATED into opencode's "mcp" format
   (top-level "mcp", "type": "local", "command" as one array, "enabled": true,
   "timeout": 60000). Merge, don't overwrite.
4. Tell me to quit and reopen opencode, then verify: list or fetch a recent
   transcript through the MCP and show me the title.
5. Record in AGENTS.md (Meeting Intelligence Layer) the MCP tool names and where
   transcripts live.

After this, when I say "process the meeting", pull the latest transcript, produce
MEETING MINUTES per AGENTS.md, extract action items with owners and due dates into
tasks.md (and data/action-items.xlsx if Excel is installed), and offer to save a
summary to my Curator.
```

---

## 10.3 Atomic Mail — the agent's own mailbox

Gives Atlas its **own** mailbox — no human webmail login; the agent holds the credentials and is the only one who uses it. This is Atlas's outbound/notification channel (drafts you approve, follow-ups, digests), **separate from your real inbox**.

**ATOMIC MAIL PROMPT:**
```
Set up Atomic Mail for Atlas so it has its own agentic mailbox. This is an
agent-owned mailbox — there is no human inbox login; you (the agent) hold the
credentials and are the only one who accesses it. Work only in the project-level
opencode.jsonc in this folder. Coach me through it.

1. Ask me what username I want for the agent's mailbox (e.g. a short handle like
   "petra-agent").
2. Register the mailbox by running:
       npx --package=@atomicmail/agent-skill atomicmail register --username "<the
       username I give you>"
   Capture the account it returns and store any credentials in the tool's own
   credential store / an environment variable — NEVER in AGENTS.md or any file.
3. Register the Atomic Mail MCP in opencode.jsonc, opencode format EXACTLY
   (merge into any existing "mcp" block):
       "atomicmail": {
         "type": "local",
         "command": ["npx", "-y", "@atomicmail/mcp"],
         "enabled": true,
         "timeout": 60000
       }
4. Tell me to quit and reopen opencode, then verify it works: run a JMAP
   "Mailbox/get" (or send yourself a short test message and read it back).
5. Record in AGENTS.md (Communication Layer + Dependencies): the mailbox handle you
   registered, and that Atomic Mail is installed. Do NOT record credentials.
6. Remind me of the rule: you never send an email without my explicit "confirm
   send", and you always show me the draft first.
After setup, summarise what you did in 5 bullets.
```

> ⚠️ Atlas **never** sends mail without your explicit "confirm send." That rule is in the mandate — keep it there.

---

## 10.4 Connect Your Real Inbox — Gmail / Outlook (Advanced)

To have Atlas triage **your own** inbox and prepare email reports, connect a Gmail or Outlook MCP.

> **Read this before you decide.** This is the highest-friction, highest-privacy-cost step in the guide, so it's optional and advanced:
> - It requires an **OAuth grant** through Google's/Microsoft's own consent screen. **You** complete that in your browser — Atlas never sees or enters your password (a hard rule in the mandate).
> - Start **read-only**. Let Atlas *summarise and draft*; keep *sending/archiving/rules* behind your explicit approval.
> - **Outlook / Microsoft 365** is the smoother route: the same Softeria MCP used for the calendar (§10.1, Route B) has a mail preset, a read-only switch and a simple browser sign-in. **Gmail** MCPs generally require creating your own Google Cloud OAuth app — that's a technical step; ask for help with it.
> - If you're privacy-sensitive, you may prefer to skip this and keep Atlas on the agent-owned Atomic Mail mailbox only.

**CONNECT REAL INBOX PROMPT:**
```
I want to connect my real email inbox to Atlas, read-only to start. Walk me through
it carefully and flag anything I need to approve myself.

1. Find a reputable, actively-maintained Gmail (or Outlook) MCP server that
   supports read + draft. For Outlook / Microsoft 365, prefer
   @softeria/ms-365-mcp-server started with "--preset", "mail", "--read-only"
   (add "--org-mode" for a work or school account) — note that read-only mode may
   also block saving drafts to Outlook; if so, show me drafts here in the chat
   instead, and only widen access when I ask. Tell me which one
   you recommend and why, what OAuth scopes it requests, and whether I would need
   to create anything in a developer console. Prefer read-only scopes to start.
2. Explain the OAuth step: I will complete the consent in my own browser; you will
   NOT ask for or enter my password. Confirm you understand this before proceeding.
3. Register the MCP in the project-level opencode.jsonc in THIS folder, TRANSLATED
   into opencode's "mcp" format (with "timeout": 60000). Put any client secret /
   token path in an "environment" block or the tool's own store — NEVER in
   AGENTS.md or any file in this folder.
4. After I complete the OAuth grant, verify with a safe read (e.g. list the last 5
   subject lines) — do not send, archive, or modify anything.
5. Record in AGENTS.md (Communication Layer): the provider, the scope (read-only?),
   and the tool names. Note that acting on the inbox (send/reply/archive/rules)
   requires my explicit approval each time.

Once connected, when I say "email report", summarise what's in my inbox: what needs
a reply, what's waiting on me, what can wait. Draft replies only for my approval.
```

---

# 11. Running Atlas — Basic Scenarios

Each scenario is a **copy-paste prompt**. Fill the `[BRACKETS]` and paste. Each says what it needs. (The same prompts live in [scenarios/](scenarios/README.md).)

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
*Needs: Web Search (6.2); better with the Curator (7).*
```
Brief me on [company / person / topic] before my meeting at [time].
Check my Curator second brain first, then search the public web. Give me a
RESEARCH BRIEF per AGENTS.md: bottom line up top, key points with sources, what my
second brain already knew, and watch-outs. Keep it to one screen.
```

## Basic 4 — Read a document
*Needs: Documents (6.1).*
```
Read the document at documents/[filename — .docx, .pptx, .xlsx or .pdf]. Give me:
a one-paragraph summary, the 5 things I most need to know, and any dates, numbers,
or obligations that matter. If there's a table worth keeping, extract it into a
new sheet in data/[name].xlsx.
```

## Basic 5 — Weekly digest
*Needs: nothing extra; richer with the modules on.*
```
Produce my WEEKLY DIGEST per AGENTS.md for this week: what got done, meetings and
decisions, what's open or overdue, any research signals, and next week's plan.
Save it to reports/weekly/ and give me the highlights.
```

## Basic 6 — Prepare for tomorrow
*Needs: Calendar (10.1); better with the Curator (7).*
```
Look at my calendar for tomorrow. For each meeting, give me one line: who, what
it's about, and what I should prepare. For the most important one, check my
Curator second brain and give me a short RESEARCH BRIEF. Add any preparation I
need to do as tasks in tasks.md.
```

---

# 12. Advanced Scenarios

These chain modules together — the kind of thing that saves a professional real hours. Fill the `[BRACKETS]` and paste.

## Advanced 1 — Meeting → action items → follow-up drafts → second brain
*Needs: Meeting module (10.2) or pasted transcript · Curator (7) · Atomic Mail (10.3) optional.*
```
Process my last meeting end to end:
1. Read the latest transcript (or I'll paste it below).
2. Produce MEETING MINUTES and file action items with owners + due dates.
3. For each action item that needs an email to someone, draft the email — but do
   NOT send anything; show me the drafts for approval.
4. Save a short summary of the meeting's decisions to my [DOMAIN] Curator domain
   following the my-curator skill (no broken links, no duplicate pages).
Report what you filed, what's drafted, and what's waiting on my approval.
---
[PASTE TRANSCRIPT HERE if OpenWhispr is not installed]
```

## Advanced 2 — Competitive / market monitor (recurring)
*Needs: Web Search (6.2) · Curator (7) · Excel (6.3) optional.*
```
Set up a monitoring routine for [company / market / topic]. Right now:
1. Search the public web for anything new in the last [30] days.
2. Cross-check against what my Curator second brain already has, so you only flag
   what's genuinely new.
3. Give me a RESEARCH SCAN per AGENTS.md with a one-line signal I should care about.
4. Log the findings to data/research-log.xlsx and save anything important to my
   [DOMAIN] Curator domain.
Then tell me: what's the cleanest way for me to re-run this weekly? (e.g. I paste
"run the monitor" each Monday.)
```

## Advanced 3 — Board pack → structured data → briefing
*Needs: Documents (6.1) · Excel (6.3) · Curator (7) optional.*
```
I've dropped [a board deck / contract / financial report] at documents/[filename].
1. Read it with the Documents MCP.
2. Extract the key structured data (terms, dates, figures, parties — whatever fits)
   into a clean sheet in data/[name].xlsx.
3. Write me a one-page RESEARCH BRIEF on what it means for me and any risks or
   deadlines I must not miss.
4. Save the summary to my [DOMAIN] Curator domain.
Flag anything that needs a decision from me.
```

## Advanced 4 — Inbox triage → daily email report → draft replies
*Needs: Real inbox module (10.4).*
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
2. If the calendar is connected, list this week's key meetings and what each needs.
3. Pull last week's WEEKLY DIGEST for context.
4. If the email module is on, add an inbox summary (read-only).
5. If web search is on, run a quick scan on [my key topic] for weekend news.
6. End with a prioritised, numbered plan for my week — the 5 things that matter most.
Keep it to one screen. Don't take any external action without my approval.
```

## Advanced 6 — Team knowledge → my decision
*Needs: Curator (7) · Shared Brain (8).*
```
I have to decide [the decision]. Search our Shared Brain and my own Curator
domain for everything relevant: what the group has learned, what I've noted, and
where they disagree. Give me a RESEARCH BRIEF with a clear recommendation and the
two strongest counter-arguments. Cite the pages you used.
```

---

# 13. Troubleshooting

Most problems are fixed by asking Atlas: *"check my setup"* — it tests every tool and tells you what to do.

| What you see | What it usually means | What to do |
|---|---|---|
| Atlas doesn't have a tool you just installed | opencode only loads tools when it starts | Quit and reopen opencode, then *"check my setup"* |
| A tool times out the first time | The first start downloads its components | Wait a minute and try again; the prompts set `"timeout": 60000` for this |
| macOS says The Curator "cannot be opened" | Normal for the first launch of the Mac app | System Settings → Privacy & Security → **Open Anyway** (§7.1) |
| macOS says The Curator "is damaged" | You have a very old build (v3.30 or earlier) | Download the newest `.dmg` from the Releases page |
| Curator tools fail | The MCP path is wrong, or (Mac app) the app has never been opened | Open The Curator once, then re-run the §7.3 prompt |
| The Curator can't ingest | No AI key, or the free Gemini tier's daily limit is reached | Check **Settings** in the app; wait a day or enable billing |
| Web search returns errors | Free search is rate-limited | Wait, or ask Atlas to switch to the DuckDuckGo fallback (§6.2) |
| A PDF comes back empty | It is a scanned image with no text | Use a text PDF, or ask Atlas for an OCR option |
| Calendar shows nothing (Mac) | Calendar access wasn't granted | System Settings → Privacy & Security → Calendars → allow Full Access |
| Atlas asks you to run a terminal command | It forgot its coaching rules | Say: *"Please do that yourself — I'm not technical (see Coach Mode)."* |

Still stuck? Ask Atlas to explain the error in plain language — or bring the message to your instructor.

---

# 14. FAQ

**Do I need Claude or a paid subscription?**
No. Atlas runs on opencode, which is free and open-source with free models built in — and works with local Ollama models if you want everything on your machine. The Curator needs an AI key only to *ingest* documents; a free Gemini key is enough to start.

**Do I ever type terminal commands or edit config files?**
No. You paste prompts; Atlas installs tools, writes `opencode.jsonc`, and builds the folders. The things you do by hand are installing opencode, creating the top-level folder, installing the Curator Mac app (if you choose it), and approving the prompts your computer shows you.

**What is "coach mode" and can I turn it off?**
It's Atlas explaining what it does and why while it builds itself. Once you're comfortable, say *"coach mode off"* — Atlas just does the work. *"coach mode on"* brings it back.

**Can Atlas really listen to my meetings live?**
Not through the MCP protocol. The Meeting module (10.2) uses a local app that transcribes on your device; Atlas turns the transcript into minutes afterward. Without it, paste or drop in a transcript and Atlas does the same thing.

**Can Atlas read my real Gmail/Outlook and my calendar?**
Yes, via the optional modules (10.1 and 10.4). Calendar access starts read-only; the inbox needs an OAuth grant **you** complete in your browser. Atlas never enters your password. If you'd rather not, keep Atlas on its own Atomic Mail mailbox.

**Is my data private?**
Your files stay in your folder, and your second brain is plain text on your own machine. Data leaves your computer only in these cases: prompts to a **hosted model** (use a local model to avoid it); documents you **ingest** into The Curator go to the AI provider whose key you added; **web searches** go to the search service; and anything you explicitly approve (sending an email) or opt into (connecting your inbox or calendar). Meeting transcription is 100 % on-device.

**Does The Curator app need to be open for Atlas to use my second brain?**
No. The my-curator MCP reads your knowledge folder directly. Open the app to ingest documents, chat, or change settings.

**How do I add a tool that isn't listed?**
Ask Atlas: *"Is there a free, reputable MCP that can do [X]? If so, install it for this opencode project using the standard Conduit pattern and tell me what it can do."* Same pattern as everything in Level 2.

**Can I run more than one Atlas?**
Yes — one folder per agent. Because MCPs are registered in the project `opencode.jsonc`, each agent keeps its own tools. Build a personal one and a work one if you like. They can share one Curator.

**What if a tool is unavailable during a session?**
Atlas will say so and continue with the tools it has. Say *"check my setup"* to see what's wrong, and re-run the relevant install prompt to fix it.

---

# 15. Glossary & Resources

### Glossary

| Term | In plain language |
|---|---|
| **Agent** | An AI that can *act* — read and write files, use tools — not just chat |
| **Agent harness** | The program that turns a model into an agent (here: opencode) |
| **Model** | The AI "engine" — hosted by a provider, or local on your computer |
| **`AGENTS.md`** | Your agent's job description and rulebook — its **mandate** |
| **`memory.md`** | Atlas's notebook for this folder: last session, open items, learning path |
| **MCP** | A standard plug that gives an agent a new ability |
| **`opencode.jsonc`** | The list of MCPs this agent may use — lives in your folder |
| **Skill** | A playbook that teaches an agent to use a tool well |
| **Second brain** | Your knowledge, organised as a connected graph your agent can search (The Curator) |
| **Domain** | One topic area of your second brain (e.g. `work`, `personal`) |
| **Ingest** | Feeding a document into your second brain |
| **Shared Brain** | A second brain a team builds together |
| **Context management** | Deciding what an agent knows when it starts — e.g. a Curator project's brief and handoff |
| **Handoff** | A saved note of where the work stands, so the next session can continue |
| **API key** | A password that lets an app use an AI provider on your account — it goes in the app, never in a chat |

### Resources

| | |
|---|---|
| [Conduit framework](../../README.md) | The framework Atlas is built on |
| [The Curator — repository & releases](https://github.com/talirezun/the-curator) | Source code, the Mac app downloads |
| [Curator user guide](https://github.com/talirezun/the-curator/blob/main/docs/user-guide.md) | Everything The Curator does |
| [Curator Mac app guide](https://github.com/talirezun/the-curator/blob/main/docs/mac-app.md) | Installing, approving and updating the Mac app |
| [Curator MCP guide](https://github.com/talirezun/the-curator/blob/main/docs/mcp-user-guide.md) | The 24 MCP tools, the skills, troubleshooting |
| [Shared Brain user guide](https://github.com/talirezun/the-curator/blob/main/docs/shared-brain-user-guide.md) | Joining or running a Shared Brain |
| [Working state guide](https://github.com/talirezun/the-curator/blob/main/docs/working-state.md) | Projects, briefs and handoffs |
| [Curator guide for agents](https://github.com/talirezun/the-curator/blob/main/llm-docs/curator-user-guide.md) | Written for AI agents — Atlas can read it to help you |
| [opencode docs](https://opencode.ai/docs/) | The agent harness |
| [MarkItDown](https://github.com/microsoft/markitdown) | The Documents MCP |

---

*Atlas — Professional Assistant Agent · Guide v2.0 · Conduit Use Case #2*
*opencode-native · prompt-driven · privacy-first · coach-led. Built on the [Conduit framework](../../README.md).*

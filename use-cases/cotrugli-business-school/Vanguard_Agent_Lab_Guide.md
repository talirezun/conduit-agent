**VANGUARD AGENT LAB**

Complete Setup and Operations Guide

COTRUGLI Business School  |  Vanguard MBA  |  Chasing Jarvis

Dr. Tali Rezun  |  Version 3.1  |  June 2026

|  |  |
| --- | --- |
| **Track A** | opencode — free, recommended for demonstrations and students without a Claude subscription |
| **Track B** | Claude Desktop with **Claude Cowork** (or Claude Code) — for students with Claude Pro ($20/month) |

> **What changed in v3.0 — read this first.** You no longer type terminal commands. You give your agent **prompts**, and the agent installs and configures everything for you. Both harnesses can do this: opencode (Track A), and **Claude Cowork** (Track B) — Cowork runs in a local environment with file and shell access, so it can install software, edit configuration files, and build the ledger connector just like opencode. Claude Code works for this too. **Plain Claude Chat cannot** run this setup — it is not an agent and has no file access. So on Track B, always work in Claude Cowork (or Claude Code), never in the normal chat window.

# **1. What We Are Building**

You will build a personal AI Execution Agent — a business accountability partner that helps you complete one real improvement project in 30 days.

The agent has a job description, knows your goal, knows its boundaries, and produces a structured record of every commitment, action, and outcome. You interact with it in natural language. It reads your files, writes reports to your computer, connects to external tools, and sends tamper-evident hashes to the Cotrugli Ledger.

After the course, the agent stays with you. It is a tool for your business — not a demo.

| **KEY** | Two configurations, one architecture. The same AGENTS.md file is used for both the Personal Agent (individual productivity, planning, personal projects) and the Company Agent (business process improvement, team KPIs, company workflows). You repeat the setup twice with different configurations. |
| --- | --- |

### **The agent architecture — six layers**

| **Layer** | **Purpose** | **Status** |
| --- | --- | --- |
| **📋 Execution** | 30-day project tracking — commitments, fulfillments, evidence | Required |
| **🧠 Memory** | Short-term: memory.md  │  Long-term: My Curator MCP (compounding second brain) | Required |
| **📒 Ledger** | Cotrugli Ledger connector — tamper-evident lifecycle hashes | Required |
| **📊 Data** | Excel workbooks for structured tracking and reporting | Recommended |
| **📡 Intelligence** | Student-defined data sources, web monitoring, signals | Optional |
| **📧 Communication** | Atomic Mail MCP for agentic email | Optional |

| **NOTE — spreadsheets differ by track** | **Claude Desktop (Track B) handles Excel natively** — no extra tool to install. **opencode (Track A) does not write Excel natively** — if you need spreadsheets on opencode, install the Excel MCP (`haris-musa/excel-mcp-server`) with the prompt in Section 6. |
| --- | --- |

### **The 30-day lifecycle — what the agent tracks**

Every project follows this structured sequence. The agent records every stage and never deletes anything — the complete trail is your proof.

| **Stage** | **What happens** |
| --- | --- |
| **COMMITMENT** | You declare your goal, target, deadline, and reviewer |
| **FULFILLMENT** | You complete an activity and report it with evidence |
| **ACCEPTANCE** | Your reviewer confirms the result is sufficient |
| **DISPUTE** | Reviewer finds evidence insufficient — the record stays |
| **CORRECTION** | You provide additional evidence addressing the dispute |
| **RESOLUTION** | The dispute is formally resolved |
| **FINAL RESULT** | The outcome is permanently recorded |

# **2. Tool Setup**

You only need to install one application: your agent. Everything else, the agent installs for you later, through prompts.

## **Track A — opencode (Primary, Free)**

opencode is an open-source AI coding agent that works as a powerful agent harness. It reads your AGENTS.md file, accesses your computer's file system, connects to MCP servers, installs software, and writes configuration files. All demonstrations in this course are performed using opencode.

### **Step 1 — Download and install opencode**

- Go to opencode.ai and download the desktop application for your operating system (macOS, Windows, or Linux).
- Install the application. This takes about two minutes.
- When you open opencode for the first time, select a free model from the list. Free models are always available — you do not need to pay for API access.

| **FREE** | opencode is completely free. Free LLM models are built in. You do not need a subscription or API key to get started. |
| --- | --- |

### **Step 2 — (Optional) Install VS Code**

You only need VS Code if you want to read or edit AGENTS.md by hand. If you prefer, you can let the agent fill AGENTS.md for you (Section 5, Option A) and never open an editor at all.

- If you want it: download from code.visualstudio.com, install, and open. No extensions are needed for markdown editing.

| **uv, Node.js, and other tools?** | You do not install these yourself. When a tool is needed for an MCP or the ledger connector, your setup prompt tells the agent to check for it and install it if missing. See Sections 4, 6, and 8. |
| --- | --- |

## **Track B — Claude Desktop + Claude Cowork**

Claude Desktop is the desktop application from Anthropic. The agent you will work in is **Claude Cowork** (a tab inside Claude Desktop): it runs in a local environment with access to a folder on your computer, can read/edit/create files, and can run installation steps — so it can complete this entire setup from prompts, just like opencode. (Claude Code works as an alternative.)

### **Step 1 — Install Claude Desktop and open Cowork**

- Download Claude Desktop from claude.ai/download.
- Install and sign in with your Claude Pro account ($20/month).
- Open the **Cowork** tab and point it at your project folder. This is where you will paste the prompts.

| **IMPORTANT — use Cowork, not Chat** | The ordinary Claude chat window cannot install software or edit files — it is not an agent. All setup on Track B happens in **Claude Cowork** (or Claude Code). If a prompt does nothing, check that you are in Cowork and not the normal chat. |
| --- | --- |

### **Step 2 — How MCP configuration works on Claude Desktop**

On Track B, MCP servers are registered in Claude Desktop's configuration file. Cowork has file access, so when you paste an install prompt, **Cowork can edit this file for you** — you do not edit it by hand. The file lives at:

```
# macOS:
~/Library/Application Support/Claude/claude_desktop_config.json

# Windows:
%APPDATA%\Claude\claude_desktop_config.json
```

Once an MCP is in this file, Claude Desktop automatically makes it available inside Cowork. There is **one** manual step a prompt cannot do for you: **after any change to this file you must fully quit and restart Claude Desktop** so the new MCP loads. The agent will remind you.

| **CRITICAL — do not cross the streams** | opencode and Claude Desktop have **completely separate** MCP configurations. opencode's file is **opencode.jsonc** (inside your project folder). Claude Desktop's file is **claude_desktop_config.json** (system location above). When you prompt an opencode agent, it must edit opencode.jsonc, never the Claude file — and a Cowork agent must edit claude_desktop_config.json, never opencode.jsonc. Every install prompt below tells the agent which file to use; keep that line in. |
| --- | --- |

# **3. Creating Your Project Folder**

Each agent lives in its own folder on your computer. You create one folder per agent. If you build both a Personal Agent and a Company Agent, you create two folders.

You only need to create the **top-level folder** by hand and place AGENTS.md inside it. The Setup Prompt (Section 4) creates everything else.

### **Step 1 — Create one folder per agent**

Name the top-level folder clearly. Examples:

```
vanguard-agent-personal/
vanguard-agent-company/
petra-sales-agent/
ivan-operations-agent/
```

### **Step 2 — Place the AGENTS.md file**

- Copy the AGENTS.md file provided by your instructor into the root of your project folder.
- Do **not** create the subfolders yourself — the Setup Prompt will. (You only make the one top-level folder.)

| **TRACK B** | If using Claude Desktop: rename AGENTS.md to CLAUDE.md. Add it as Project Knowledge in your Claude Project (click + in the Files section), and paste the project instructions into the Instructions field of your project. |
| --- | --- |

### **The folder structure (for reference)**

This is what your folder will look like after the Setup Prompt runs. You do not build it by hand.

```
[your-agent-folder]/              <- you create this one folder
│
├── AGENTS.md                     <- master agent config (you place this)
├── opencode.jsonc                <- MCP config (agent creates — opencode only)
├── ledger_connector.py           <- ledger connector (agent builds during setup)
├── memory.md                     <- session memory (agent creates)
├── email-log.md                  <- email log (if Atomic Mail installed)
│
├── reports/
│   ├── weekly/                   <- WEEKLY-YYYY-MM-DD.md
│   ├── monthly/                  <- MONTHLY-YYYY-MM.md
│   └── run-reports/              <- RUN-YYYY-MM-DD.md
│
├── commitments/                  <- commitment records
├── evidence/                     <- evidence files and screenshots
├── projects/                     <- project documentation
└── data/                         <- Excel workbooks
    ├── project-tracker.xlsx
    ├── weekly-summary.xlsx
    └── evidence-log.xlsx
```

# **4. The Setup Prompt — Let the Agent Build Everything**

Instead of configuring anything manually, you instruct your agent to read AGENTS.md and set everything up. This is the approach for **all** students, on both tracks.

### **How it works**

You open your project folder in opencode (or Claude Cowork / Claude Code), paste the Setup Prompt below, and the agent creates the folder structure, the memory file, and the configuration it needs. It also checks for required tools (like uv or Node.js) and installs them only if they are missing.

### **Step 1 — Open your project folder in your agent**

- **Track A — opencode:** use File > Open Folder (or drag your project folder into the app). You should see your folder as the working directory.
- **Track B — Claude Cowork:** open the Cowork tab in Claude Desktop and point it at your project folder. (Claude Code also works.)

### **Step 2 — Paste the Setup Prompt**

**SETUP PROMPT — copy and paste this into your agent:**

```
Read the AGENTS.md file in this project folder carefully. Then do the following,
and explain each step in plain language as you go:

1. Create this folder structure inside the current project folder:
   reports/weekly/  reports/monthly/  reports/run-reports/
   commitments/  evidence/  projects/  data/

2. Create memory.md in the project root with this starting content:
   Last session: [today's date]
   Last action: Setup complete
   Current project status: Day 0 — not yet started
   Unresolved items: None
   Notable events: Agent configured and ready
   Next planned action: Create first Commitment

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

5. Whenever you install or configure anything that future sessions will need
   to know about, append a short note to the relevant section of AGENTS.md so
   the next session is aware of it. Never write secrets (API keys, tokens,
   tenant IDs, mail credentials) into AGENTS.md — those belong in environment
   variables or the tool's own credential store.

6. Confirm what was installed and created, then run the Activation Status Check
   described in AGENTS.md and show me the result.
```

### **Step 3 — Verify the setup**

After the Setup Prompt runs, your agent shows you what it created. Check that:

- The folder structure exists
- memory.md was created
- (opencode) opencode.jsonc exists if any MCP was configured
- The Activation Status Check runs and shows your project details

# **5. Configuring Your Agent (AGENTS.md)**

AGENTS.md is the heart of your agent — its job description, goal, mandate, and boundaries. You have **two ways** to fill it. Pick whichever you prefer.

## **Option A — Let the agent fill AGENTS.md for you (recommended)**

You do not have to open an editor. Tell your agent about your project in plain language and let it write your answers into AGENTS.md. Paste this prompt and answer its questions one at a time.

**FILL MY AGENTS.md — copy and paste this into your agent:**

```
Open the AGENTS.md file in this project folder. You are going to help me fill it in.

First, ask me whether this is a PERSONAL agent (my own productivity / a personal
30-day improvement project) or a COMPANY agent (a business process improvement
project at my organisation).

Then interview me ONE question at a time to collect:
  - Agent name and my full name (and organisation, if Company)
  - My one clear goal for the next 30 days, with a measurable target
  - My current state, described with at least one number
  - My deadline (30 days from today)
  - My reviewer's name and role
  - 2–3 actions the agent IS allowed to do for this project
  - 1–2 actions the agent must NEVER do
  - 2–3 pieces of evidence I will collect to prove the result

After you have my answers, write them into the matching section of AGENTS.md,
replacing the placeholders. Leave the other configuration section as-is. Show me
the filled-in section and ask me to confirm before saving. Do not invent details —
if something is unclear, ask me.
```

The agent asks, you answer, it writes the file. When it shows you the result, read it and confirm.

## **Option B — Fill AGENTS.md yourself in VS Code**

If you prefer to edit directly: open AGENTS.md in VS Code, replace every [PLACEHOLDER], choose the PERSONAL or COMPANY section, and save. The two reference tables below tell you what to write.

### **What to fill in — Personal configuration**

| **Placeholder** | **What to write** |
| --- | --- |
| **[AGENT_NAME]** | A name for your agent. Example: My Personal Agent or Maria's Planner |
| **[YOUR_FULL_NAME]** | Your full name |
| **[PERSONAL GOAL]** | Your 30-day personal challenge in 1-2 sentences. Must have a measurable target. |
| **[CURRENT_STATE]** | How things are today, with a number. Example: 2 hours/day on email, no inbox system |
| **[TARGET_STATE]** | How things should be in 30 days, with a number. Example: 30 minutes/day, zero backlog |
| **[DEADLINE]** | Exact date — 30 days from today |
| **[REVIEWER_NAME]** | Who will confirm your result. Can be a mentor, manager, or accountability partner |
| **[ALLOWED_ACTION_1..3]** | What the agent may help you with. Example: research productivity tools, redesign daily routine, track time blocks |
| **[FORBIDDEN_ACTION_1..2]** | What the agent must never do. Example: change anything in my professional tools without explicit approval |
| **[EVIDENCE_1..3]** | What proof you will collect. Example: daily time log, before/after screenshots, reviewer sign-off |

### **What to fill in — Company configuration**

| **Placeholder** | **What to write** |
| --- | --- |
| **[AGENT_NAME]** | Example: Petra's Sales Agent or Operations Improvement Agent |
| **[ORGANISATION]** | Your company name |
| **[JOB_ROLE]** | Your position. Example: Head of Operations, Sales Manager |
| **[BUSINESS_PROBLEM]** | The problem in 1-2 sentences with a number. Example: Sales report takes 4 hours/week, delaying Friday review |
| **[CURRENT_STATE]** | Today's measurable situation. Example: 4 hours every Friday, report ready at 5 PM |
| **[TARGET_STATE]** | Measurable goal. Example: Under 60 minutes, report ready by noon |
| **[DEADLINE]** | Exact date — 30 days from today |
| **[REVIEWER_NAME]** | Name and role. Example: Marija Novak, Sales Director |
| **[ALLOWED_ACTION_1..3]** | What the agent may help with. Example: research AI tools, redesign workflow, train one team member |
| **[FORBIDDEN_ACTION_1..2]** | Hard limits. Example: change report content without director approval, share raw data externally |
| **[EVIDENCE_1..3]** | Verifiable proof. Example: time log before/after, workflow description with screenshots, reviewer confirmation |

### **Example — Personal Agent (pre-filled)**

Use this as a reference for what a completed Personal configuration looks like:

```
Agent name:       Ana's Personal Agent
Owner:            Ana Marković
Configuration:    PERSONAL

Personal goal:    Reduce time spent on email from 2 hours per day to 30 minutes
                  by building an AI-assisted inbox triage workflow.
Current state:    2 hours/day on email, no system, reactive, frequent missed follow-ups
30-day target:    30 minutes/day, zero backlog every Friday, zero missed follow-ups
Deadline:         July 20, 2026
Reviewer:         Ivan Bauer, my manager

Allowed actions:
  1. Research and test AI-assisted email management tools
  2. Redesign my daily inbox routine and document the new process
  3. Measure daily time spent on email for 4 consecutive weeks

Forbidden actions:
  1. Send emails on my behalf without explicit confirmation
  2. Access email accounts other than the one declared for this project

Evidence required:
  1. Daily time log (before: 3 weeks, after: 4 weeks)
  2. Description of the new workflow with screenshots
  3. Written confirmation from Ivan Bauer that follow-up quality improved
```

# **6. Optional MCPs — Install by Prompt**

MCPs give your agent extra abilities. You install them the same way you do everything else: with a prompt. **You never paste JSON into a config file by hand** — the agent does it, and records what it added back into AGENTS.md.

The required Cotrugli Ledger connection is covered separately in Section 8.

| **WORKS ON BOTH TRACKS** | The install prompts below work in opencode (Track A) and in Claude Cowork / Claude Code (Track B). Each prompt tells the agent to register the MCP in the correct file for its harness — opencode.jsonc for opencode, claude_desktop_config.json for Claude. On Claude, remember to restart Claude Desktop afterwards so the MCP loads. |
| --- | --- |

## **Excel MCP — Spreadsheets on opencode (Track A only)**

Claude Desktop already reads and writes Excel files natively, so **Track B students can skip this.** On opencode, install the Excel MCP if your project needs spreadsheets (project tracker, weekly summary, evidence log).

**INSTALL EXCEL MCP (opencode) — copy and paste this into opencode:**

```
Install the Excel MCP server (haris-musa/excel-mcp-server) for this opencode
project so the agent can read, create, and update .xlsx files.

Steps:
1. Make sure a Python runner (uv / uvx) is installed; if not, install it for me.
2. Register the Excel MCP in opencode.jsonc in THIS project folder (do not touch
   any Claude configuration file), using:
       {
         "mcpServers": {
           "excel": {
             "type": "stdio",
             "command": "uvx",
             "args": ["excel-mcp-server", "stdio"]
           }
         }
       }
3. Confirm the MCP is available, then create a test workbook at
   data/project-tracker.xlsx with a header row (Date, Action ID, Stage, Evidence,
   Status) to prove it works.
4. Record in AGENTS.md (Data Layer) that the Excel MCP is installed and available.
After setup, summarise what you did in 3 bullet points.
```

## **My Curator MCP — Long-Term Memory (the agent's second brain)**

The Curator is an open-source local knowledge system that gives your agent a **persistent, compounding second brain**. You ingest documents, notes, and research, and The Curator organises them into a **knowledge graph** the agent can query across sessions.

Why this matters: instead of starting cold each time, your agent gains on-demand access to a growing neural network of your knowledge. The graph is split into **domains** — each domain can be **your own knowledge** or a **company brain** — and it **compounds over time**: the more you add, the more context your agent has. That context is exactly what makes the agent good at executing real tasks, because it can ground its work in what you (or your company) already know rather than guessing.

**Documentation:**
- Repository: https://github.com/talirezun/the-curator
- User guide: https://github.com/talirezun/the-curator/blob/main/docs/user-guide.md
- GitHub sync (optional): see docs/sync.md in the repository

**INSTALL THE CURATOR — copy and paste this into your agent:**

```
Please install "The Curator" on this machine for me.
Project: https://github.com/talirezun/the-curator
User Guide: https://github.com/talirezun/the-curator/blob/main/docs/user-guide.md
Steps:
1. Verify Node.js 18+ is installed; if not, install it (Homebrew on macOS,
   nodejs.org installer on Windows, system package manager on Linux).
2. git clone https://github.com/talirezun/the-curator.git into the user's home directory.
3. cd the-curator && npm install
4. On macOS: bash scripts/build-app.sh to build "The Curator.app", then move/copy
   it to /Applications and remind me to drag it from Finder into my Dock.
5. On Linux/Windows: skip the .app build; explain how to start the server
   (`node src/server.js`, with CURATOR_NO_OPEN=1 on Windows) and remind me to
   open http://localhost:3333.
6. Open the URL once the server is running so I can complete the onboarding
   wizard (API key + first domain).
7. Tell me what to do if I want to enable GitHub sync (point me to docs/sync.md).
Do not edit any files outside ~/the-curator. Do not commit anything to my git config.
Do not ask me for my API key — the in-app onboarding wizard will handle it.
After the install finishes, summarise what you did in 5 bullet points, and add a
note to AGENTS.md recording that the my-curator MCP is installed and how to reach
it, so future sessions know it is available.
```

| **TIP — set up your first domain** | After install, the onboarding wizard asks you to create your first domain. Make one domain for your **personal** knowledge and, if relevant, a separate domain for your **company** knowledge. Ingest a few real documents early — the agent's usefulness compounds as the graph grows. The user guide walks through ingesting sources and querying domains. |
| --- | --- |

## **Atomic Mail MCP — Agentic Email (Optional)**

Atomic Mail gives your agent its own mailbox. This is a fully **agentic** mailbox: there is no human webmail login. The agent registers the account with a username you choose, holds the credentials, and is the only one who reads or sends from it. It speaks JMAP under the hood.

**Reference commands (the agent runs these — you do not):**

```
# Register the agent's mailbox
npx --package=@atomicmail/agent-skill atomicmail register --username "myagent"

# Send a JMAP request inline
npx --package=@atomicmail/agent-skill atomicmail jmap_request \
  --ops '[["Mailbox/get", {"accountId": "$ACCOUNT_ID"}, "m0"]]'

# Send a JMAP request from a preset file
npx --package=@atomicmail/agent-skill atomicmail jmap_request --ops-file send_mail.json

# Ask for help
npx --package=@atomicmail/agent-skill atomicmail help
```

**MCP registration (the agent adds this to the right config file for your harness):**

```
{
  "mcpServers": {
    "atomicmail": {
      "command": "npx",
      "args": ["-y", "@atomicmail/mcp"]
    }
  }
}
```

**INSTALL ATOMIC MAIL — copy and paste this into your agent:**

```
Please set up Atomic Mail for this agent so it has its own agentic mailbox.
This is an agent-owned mailbox — there is no human inbox login; you (the agent)
hold the credentials and are the only one who accesses it.

Steps:
1. Ask me what username I want for the agent's mailbox (for example a short handle
   based on my project, like "petra-agent").
2. Register the mailbox by running:
       npx --package=@atomicmail/agent-skill atomicmail register --username "<the
       username I give you>"
   Capture the account it returns and store any credentials securely in the tool's
   own credential store / an environment variable — NEVER in AGENTS.md or any file
   in this project.
3. Register the Atomic Mail MCP in the config file for the harness you are running in:
   - opencode -> opencode.jsonc in this project folder
   - Claude Cowork / Claude Code -> claude_desktop_config.json (system location),
     and remind me to fully restart Claude Desktop afterwards
   using:
       {
         "mcpServers": {
           "atomicmail": {
             "command": "npx",
             "args": ["-y", "@atomicmail/mcp"]
           }
         }
       }
4. Verify it works: run a JMAP "Mailbox/get" request (or send yourself a short test
   message and read it back) and confirm the mailbox is live.
5. Record in AGENTS.md (Communication Layer): the mailbox username/address you
   registered, and that Atomic Mail is installed. Do NOT record credentials.
6. Remind me of the email rules: you never send an email without my explicit
   "confirm send", and you always show me the draft first.
After setup, summarise what you did in 5 bullet points.
```

| **RULE** | The agent never sends an email without explicit confirmation. It always presents a draft and waits for you to say "confirm send" before using any email tool. |
| --- | --- |

# **7. Running Your Sessions**

## **First session — Create your Commitment**

On Day 1, tell your agent what you are committing to. Type this (adjust to your project):

```
I am ready to start my 30-day project.
My business problem is: [describe your problem]
Current situation: [current state with a number]
My 30-day target is: [measurable goal]
My deadline is: [date]
My reviewer is: [name and role]
Create my Commitment and save it to commitments/COMMIT-[today's date].md
```

The agent produces a structured COMMITMENT record and saves it. This is your contract.

## **Weekly check-in — Record a Fulfillment**

Every time you complete a step toward your goal, tell the agent:

```
I completed the following today:
What I did: [describe the activity]
Date: [today]
Evidence: [name of document, screenshot, measurement, or conversation]
Record this as a Fulfillment and update data/project-tracker.xlsx
```

| **EVIDENCE RULE** | The agent will not mark a Fulfillment complete without a specific, named evidence reference. A claim without proof is not a Fulfillment. |
| --- | --- |

## **Reviewer interaction — Get an Acceptance**

```
Prepare a summary for my reviewer to review.
```

Share the output with your reviewer. When they confirm:

```
My reviewer has accepted the result. Record the Acceptance.
```

## **Handling a Dispute**

```
My reviewer has disputed the result.
Their reason is: [exact reason they gave]
```

The agent records the DISPUTE and helps you prepare a CORRECTION. When the reviewer confirms the correction:

```
My reviewer accepted the correction. Record the Resolution.
```

| **IMPORTANT** | Disputes are never deleted. The complete trail — Commitment, Fulfillment, Dispute, Correction, Resolution — makes the final result more credible, not less. |
| --- | --- |

## **Run Report**

```
Produce my Run Report and save it to reports/run-reports/RUN-[today's date].md
```

# **8. Connecting to the Cotrugli Ledger (Required)**

The Cotrugli Ledger records tamper-evident hashes of your agent's lifecycle events. **Connecting to the ledger is a required part of this lab.** It does not change how your agent works locally — it adds an auditable, tamper-evident trail that aligns with the EU AI Act.

### **What gets sent to the ledger**

Only a hash (fingerprint) of each event — never the raw content. No business data leaves your computer in readable form. This is GDPR-correct by design.

### **How the connection works — one thin file**

All communication with the ledger goes through a single function, send_agent_event(), in a single file the agent builds for you: ledger_connector.py. This is the only thing that ever changes when the backend changes — your agent never does. You build it with one prompt.

| **YOU WILL NEED** | Your instructor gives you three values for the sandbox: a **base URL** (the ledger host), an **API key**, and a **tenant / participant ID**. Keep these handy. You give them to the agent when it asks, and the agent stores them as environment variables (`LEDGER_BASE_URL`, `LEDGER_API_KEY`, `LEDGER_TENANT_ID`) — never in any file in your project. |
| --- | --- |

| **NOTE — this is a real connectivity test** | Earlier versions of this lab ran on synthetic data only. You are now connecting to the **live Cotrugli DLT sandbox** and submitting real lifecycle events. The sandbox's sign-in is currently demo-friendly (relaxed auth), so this is perfect for a first connectivity test — but treat the tenant ID as a routing label, not a secret identity. Hardened per-student tokens come later, for the full CdayZ cohort. |
| --- | --- |

**LEDGER CONNECTION — copy and paste this into your agent:**

```
Build a thin connector that lets this agent submit lifecycle event hashes to the
Cotrugli Ledger (the live DLT sandbox). Follow this exactly, and explain each step
in plain language as you go. I am not a developer — you do all the technical work.

1. Create a single file, ledger_connector.py, in this project folder. It must
   expose ONE function: send_agent_event(event). This is the ONLY point that talks
   to the backend — the agent never calls the endpoint directly. I (the agent /
   the lifecycle) will pass you a small, simple payload; YOU map it to the server's
   wire format and hash the content. This separation is the whole point: when the
   backend changes later, only this file changes.

2. Ask me for three values and store them as environment variables (NEVER write
   them into ledger_connector.py, AGENTS.md, or any file in this project):
       LEDGER_BASE_URL    the sandbox host
       LEDGER_API_KEY     my API key
       LEDGER_TENANT_ID   my tenant / participant ID

3. The payload I pass you (keep this stable and simple) is:
       action_type    one of: COMMITMENT, FULFILLMENT, ACCEPTANCE, DISPUTE,
                      CORRECTION, RESOLUTION
       action_id      e.g. COMMIT-20260628
       session_id     current session ID
       agent_id       the agent name
       input_content  the text/record that triggered the event (RAW — you hash it)
       output_content the output record (RAW — you hash it)
       reviewer_ref   reviewer name, or None
       approval       auto / human_approved
       status         success / error / denied

4. send_agent_event() POSTs to:
       POST  {LEDGER_BASE_URL}/integrations/agent/events
   with headers (ALL THREE required):
       X-API-Key:    (from LEDGER_API_KEY)
       X-Tenant:     (from LEDGER_TENANT_ID)
       Content-Type: application/json     <- mandatory, or the server returns 422
   The body wraps the event in an "events" ARRAY (not a bare object):
       { "events": [ { ...AgentToolCallEvent... } ] }
   Build each AgentToolCallEvent from my simple payload:
       trace_id          a stable correlation id you generate per action_id
       span_id           a unique id you generate for this event
       parent_span_id    optional: the parent action's id for fulfillments/
                         corrections (else omit)
       session_id        from my payload
       agent_id          from my payload
       event_type        the lifecycle stage (COMMITMENT, FULFILLMENT, …). If the
                         server rejects a custom value with 422, retry with
                         event_type "tool_call" and put the stage in tool_name.
       tool_name         "vanguard-lifecycle"
       tool_input_hash   "sha256:" + SHA-256(input_content)   <- prefix is sha256: (no caret)
       tool_output_hash  "sha256:" + SHA-256(output_content)
       status            from my payload
       ts_start          RFC3339 timestamp
       ts_end            RFC3339 timestamp
       model_ref         the model you are running on
       approval          { "mode": approval, "actor_ref": reviewer_ref }
   IMPORTANT: only the hashes go on the wire — never input_content/output_content
   in raw form.

5. Handle the response:
   a. The POST returns a receipt_id (it looks like "sha256:c4c0…") and an
      anchor_status. Return the full receipt from send_agent_event() so it can be
      logged in the run report.
   b. This sandbox does NOT return a was_duplicate flag — do not look for one.
      Dedup is implicit: the receipt_id is deterministic, so re-sending the same
      event returns the same receipt instead of a duplicate record.
   c. Anchoring runs on a ~60-second timer (you need NO anchor key). After sending,
      wait up to 60 seconds, then verify inclusion by fetching the proof bundle:
          GET  {LEDGER_BASE_URL}/receipts/{URL-ENCODED receipt_id}/proof-bundle
          Header:  X-Tenant: (the SAME value you used on the POST)   <- required here too
      URL-ENCODE the receipt_id in the path (it contains "sha256:" and the colon
      breaks the URL). Add a helper verify_receipt(receipt_id) that does this and
      returns the proof bundle (leaf_hash, inclusion_proof, signed checkpoint), or
      "not anchored yet" if the 60s timer hasn't passed. Do not block on it.
   THREE GOTCHAS (all real — confirmed in testing):
      • X-Tenant must be IDENTICAL on the POST and the proof-bundle GET (else 404).
      • Content-Type: application/json is mandatory on the POST (else 422).
      • URL-encode the receipt_id in the proof-bundle path.

6. Respect these three rules:
   a. The connector is the ONLY swap point. Keep it to this one file. The agent
      stays unaware of the backend and the wire schema.
   b. Tenant is NOT identity. X-Tenant (LEDGER_TENANT_ID) is routing only and must
      stay OUT of any hash. Semantic identity travels inside the hashed body via
      agent_id. (If a future backend uses a principal_ref field, map agent_id ->
      principal_ref here — one line, in this file only.)
   c. The agent's mandate IS the governance mandate: forbidden actions and
      actions-requiring-approval map to the co-sign / approval level. Mark approval
      as human_approved only after I have confirmed.

7. Write a short comment block at the TOP of ledger_connector.py mapping the
   current schema to the future canonical Ledger (VEB), so a later swap is trivial:
       agent_id + session_id + trace_id  -> actor_ref + process_ref
       event_type                        -> event_type
       ts_start / ts_end                 -> semantic_time
       input/output hash                 -> body_binding -> ENCRYPTED_EXTERNAL / evidence_refs
       approval.mode                     -> hai5_level
       approval.actor_ref (reviewer)     -> relations / co-signer

8. Send ONE test event (event_type "COMMITMENT", with dummy input/output content)
   to confirm the connection works end to end. Show me the receipt_id and status.
   Then wait ~60 seconds for the auto-anchor timer and call verify_receipt() — show
   me the proof bundle (leaf_hash, inclusion_proof, signed checkpoint). If the first
   try says "not anchored yet", wait the rest of the 60s and try once more.

9. Record in AGENTS.md (Cotrugli Ledger section): the connector file path, the base
   URL/endpoint it targets, the verify endpoint, and the names of the environment
   variables it reads, and set status to CONNECTED. Do NOT record the API key,
   tenant ID, or base URL value itself. Add anything else a future session needs.

After you finish, summarise in 5 bullet points what you built and how a future
session triggers a ledger submission and verifies anchoring.
```

### **Triggering a submission during the lifecycle**

After the connector is built, you send events at each lifecycle moment by telling the agent:

```
Send this Fulfillment to the Cotrugli Ledger.
```

The agent always confirms before sending, because these records are immutable:

```
> I will send a hash of FUL-20260621-1 to the Cotrugli Ledger.
> Endpoint: POST {LEDGER_BASE_URL}/integrations/agent/events
> This creates an immutable record. Confirm? (yes / no)
```

| **DEMO NOTE** | The current endpoint (`POST {LEDGER_BASE_URL}/integrations/agent/events`) is the **live Cotrugli DLT sandbox** — your events are really ingested and anchored, but with test data and no real money or settlement. Submission is immediate; tamper-evident anchoring is asynchronous — your event becomes Merkle-verifiable via `GET /receipts/{id}/proof-bundle` after the server's next anchor batch. For the CdayZ demonstration in October 2026, the connector swaps to the full canonical Cotrugli Ledger (VEB) seam — a change in this one file only, with the agent untouched. |
| --- | --- |

# **9. Generic Scenarios**

If you do not yet have a specific business problem, use one of the four scenarios below. The data is fictional. Use it to fill your AGENTS.md (Option A or B in Section 5).

## **Scenario 1 — Sales Report Optimisation**

**Adriatica Wholesale d.o.o. — Sales Manager**

```
Business problem: Weekly sales report takes 4 hours every Friday to prepare.
                  Data pulled manually from three systems, reformatted in Excel.
                  This delays the Friday afternoon review meeting.
Current state:   4 hours per week, report ready at 5 PM earliest
Target:          Under 60 minutes per week, report ready by noon
Deadline:        [30 days from today]
Reviewer:        Marija Novak, Sales Director

Allowed:   Research AI tools for data pulling | Redesign template | Train one team member
Forbidden: Change report content without director approval | Share raw data externally
Evidence:  Time log (3 weeks before, 4 weeks after) | Workflow description with screenshots
           Written confirmation from reviewer that quality is maintained
```

## **Scenario 2 — Customer Onboarding Improvement**

**ProServ Consulting d.o.o. — Customer Success Manager**

```
Business problem: New client onboarding takes 18-22 business days. Clients report
                  confusion. 3 clients delayed second invoice in the last quarter
                  due to onboarding friction.
Current state:   18-22 days average, 4+ status queries per onboarding
Target:          Under 8 days average, zero unsolicited status queries
Deadline:        [30 days from today]
Reviewer:        Luka Petrovic, Head of Sales

Allowed:   Map current process | Design new onboarding sequence | Test with 2 new clients
Forbidden: Change contract terms or payment conditions | Commit to delivery dates without approval
Evidence:  Current process map with time measurements | New process document
           Time logs from 2 test onboardings | Written feedback from at least 1 client
```

## **Scenario 3 — Meeting Load Reduction**

**TechGroup Zagreb d.o.o. — Head of Product**

```
Business problem: Product team spends 16 hours/person/week in meetings. Most are
                  recurring status updates that could be async. Deep work time is
                  under 2 hours per day per person.
Current state:   16 hours/person/week in meetings, under 2h deep work blocks/day
Target:          Under 8 hours/person/week, at least 3-hour deep work blocks daily
Deadline:        [30 days from today]
Reviewer:        Ivan Bauer, CEO

Allowed:   Audit all recurring meetings | Design async status process | Cancel reducible meetings
Forbidden: Cancel cross-functional meetings without aligning other department heads
           Reduce 1:1 meetings with direct reports
Evidence:  Calendar audit (meeting hours per person) | List of cancelled/converted meetings
           Calendar data after 4 weeks | Brief team feedback
```

## **Scenario 4 — Invoice Processing Acceleration**

**Kontura Manufacturing d.o.o. — Operations Manager**

```
Business problem: Supplier invoices take 4-6 business days from receipt to approval.
                  3-5 late payment penalties per month. Process involves manual data
                  entry and Excel tracking.
Current state:   4-6 days processing, 3-5 late payments/month, manual Excel
Target:          Under 24 hours processing, zero late payments for 4 consecutive weeks
Deadline:        [30 days from today]
Reviewer:        Petra Simic, CFO

Allowed:   Map and time current process | Test AI-assisted invoice reading tool
           Redesign approval workflow
Forbidden: Change payment terms with suppliers without CFO approval
           Access banking systems directly
Evidence:  Current process map with time measurements | Late payment log (before)
           Description and test results of new process
           Invoice log for 4 weeks after launch | CFO confirmation
```

# **10. Project Canvases**

Complete these before configuring AGENTS.md. They ensure you have all the information you need before you start. (If you use Option A in Section 5, your agent will ask you for exactly these items.)

## **Project Canvas**

```
PROJECT CANVAS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Name:
Organisation:
Role:
Date:

BUSINESS PROBLEM
What is wrong, too slow, or too expensive?

CURRENT STATE
Describe today's situation with at least one number.

30-DAY TARGET
What will be measurably true in 30 days?

DEADLINE:

REVIEWER
Name:                    Role:
How they will verify the result:

EVIDENCE I WILL COLLECT
1.
2.
3.

DEFINITION OF DONE
The project is complete when:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## **Agent Design Canvas**

```
AGENT DESIGN CANVAS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Agent name:
Owner:
Business role (one sentence):
Track:   [ ] Track A — opencode     [ ] Track B — Claude Cowork

ALLOWED ACTIONS
1.
2.
3.

FORBIDDEN ACTIONS
1.
2.

REQUIRED EVIDENCE
1.
2.
3.

REVIEWER
Name:                    Role:

INTELLIGENCE LAYER (optional)
What external data sources should the agent monitor?

COMMUNICATION LAYER (optional)
[ ] Atomic Mail MCP — agent mailbox username:

LEDGER CONNECTION (required)
[ ] Connected to Cotrugli Ledger — API key + tenant ID from instructor
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

# **11. Frequently Asked Questions**

### **Do I need a SKILL.md file?**

No. For this lab, everything lives in one file: AGENTS.md (renamed CLAUDE.md if you use Claude Desktop). There is no separate SKILL.md to create or manage. We keep it to a single file on purpose, so nothing is confusing.

### **Can I run the setup in the normal Claude chat window?**

No. Plain Claude Chat is not an agent and cannot touch your files. On Track B you must work in **Claude Cowork** (or Claude Code), which has file and shell access. opencode (Track A) works the same way — it is an agent harness.

### **Do I have to type any terminal commands?**

No. You give your agent prompts, and the agent installs and configures everything — including tools like uv or Node.js, the optional MCPs, and the required ledger connector. The only manual step is restarting Claude Desktop after an MCP is added on Track B.

### **Do I need to install an Excel tool?**

Depends on your track. **Claude Desktop (Track B) handles Excel natively — no install.** On **opencode (Track A)**, install the Excel MCP (`haris-musa/excel-mcp-server`) with the prompt in Section 6 if your project needs spreadsheets.

### **What does The Curator actually give my agent?**

A compounding second brain. Your documents and notes become a knowledge graph the agent can query, organised into domains (your personal knowledge, a company brain, etc.). The more you add over time, the more context your agent has — which makes it far better at executing real tasks. See the user guide: https://github.com/talirezun/the-curator/blob/main/docs/user-guide.md

### **Does memory.md need to be created manually?**

No. The Setup Prompt instructs your agent to create it with an initial template. After that, the agent updates it automatically at the end of every session. (memory.md is short-term, per-project memory; The Curator is long-term, cross-project knowledge.)

### **Can I build both a Personal and Company agent?**

Yes. Create two separate project folders. Place AGENTS.md in each. Fill the Personal configuration in one and the Company configuration in the other. Each agent has its own memory.md, reports, and files. They are completely independent.

### **What if my 30-day goal needs to change mid-project?**

Tell your agent: "My original Commitment needs to change. The new target is [new target]." The agent records a superseding Commitment. The original is not deleted.

### **Do I have to connect to the Cotrugli Ledger?**

Yes. The ledger connection is a required part of this lab — it is what makes your agent's project trail auditable and tamper-evident, on the live Cotrugli DLT sandbox. Your instructor gives you three values (base URL, API key, tenant ID), and the Ledger Connection prompt (Section 8) builds the connector for you.

### **Can I add more MCPs after the course?**

Yes. Just prompt your agent to install the MCP you want and to register it in the correct configuration file (opencode.jsonc for opencode; claude_desktop_config.json for Claude Desktop, then restart) — and to record it in AGENTS.md. The agent handles the rest.

### **What if a tool or MCP is unavailable during a session?**

The agent notes the failure clearly and continues with the tools it has. Your session is not blocked. Fix it afterward with a prompt and it will be available next session.

### **How do I access GitHub from my agent?**

Both opencode and Claude (via Cowork / Claude Code) can use the GitHub CLI (gh). If you need it, prompt your agent to check for the GitHub CLI and install/authenticate it (cli.github.com, free; "gh auth login"). The agent can then run read-only gh commands as defined in your mandate.

*Vanguard Agent Lab | COTRUGLI Business School | Vanguard MBA | Chasing Jarvis — Module 5+*
*Version 3.1 — Dr. Tali Rezun — June 2026 (live Cotrugli DLT connector)*

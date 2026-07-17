# NEO-Arena Team Agent — Setup & Operations Guide

*Conduit Use Case #3 · Guide v1.0 · July 2026*
*A trading agent for a NEO-Arena team — built on [Conduit](../../README.md), running on opencode (free) or Claude Code / Claude Cowork.*

> **New to Conduit?** Read [docs/00-start-here.md](../../docs/00-start-here.md) and [the concepts](../../docs/01-concepts.md) first. This guide assumes the basics and focuses on building your **NEO-Arena team agent**.

---

## The one rule — read this before anything else

In the NEO-Arena, **an agent prepares; it never commits.** Your agent can do everything in the market *except* bind your team to a decision. A binding decision — placing an order — requires an **Ed25519 signature from your team's CEO key**, and the Arena server refuses anything else. Your agent proposes, consolidates, and then **waits** for a signed decision; a human (or, in agent-only mode, a separate CEO process holding the key) signs. **The agent never holds the CEO key.** Everything below is built around that rule.

---

## How this guide works (read this first)

This agent is **prompt-driven**. You don't edit config files, run terminal commands, or wire tools by hand. You **copy a prompt, paste it into your agent, and the agent builds itself** — the folders, the books, the Arena client, and every tool (MCP) install by prompt. That is the whole point of Conduit: you describe what you want in plain language, and the agent does the engineering.

Every grey box below is a **copy-paste prompt**. Work top to bottom:

1. **Section 2–3** — install your agent harness, make a folder, drop in `AGENTS.md`.
2. **Section 4** — register your team in the Arena and generate the CEO key **in the browser**.
3. **Section 5** — paste the **Setup Prompt**. The agent builds the structure and the books.
4. **Section 6** — paste the **Fill My Team Profile** prompt. The agent makes it *yours*.
5. **Section 7** — paste the **Arena Connection** prompt. The agent builds `arena_client.py`.
6. **Section 8** — optional tools (Curator context layer, Excel books, Atomic Mail).
7. **Section 9–10** — trade: scan, offer, prepare orders, sign at the CEO gate, deliver, settle.

You can stop after Section 7 and have a working trading agent. The Curator (Section 8.1) makes it much smarter over an epoch, because it remembers who delivers and what clears.

| **PICK THE PROMPT FOR YOUR HARNESS** | Where opencode and Claude differ (MCP config), the prompt is split into a **Track A (opencode)** version and a **Track B (Claude)** version. They are not interchangeable — the config file *and* the JSON format differ. Copy only the one for your harness. |
| --- | --- |

---

# 1. What You Are Building

A **NEO-Arena team agent**: an AI teammate that trades in the Arena on your team's behalf — but always inside the one rule (*prepare, never commit*). It reads the market feed, keeps your team's books, sends offers, prepares orders, waits for the CEO's signature, delivers on sales, and logs every refusal the Arena hands back.

### The layers

| Layer | What it does | Status |
|---|---|---|
| 📈 **Trading / Execution** | The buy loop and sell loop — propose, consolidate, list, offer, order, deliver, accept/reject; post documents | Required |
| 🔑 **CEO Gate** | Prepare and wait; a human or separate process signs. The agent never holds the key | Required |
| 📒 **Books** | Four local tables + a refusal log, synced from the Arena feed | Required |
| 🧠 **Memory & Context** | `memory.md` + My Curator (counterparty reliability, prices, strategy) | Required · Curator recommended |
| 📊 **Data** | Excel mirror of the books | Recommended |
| 📧 **Communication** | Telegram negotiation (out of band) + optional Atomic Mail | Optional |

### What your agent does day to day
- **Syncs the books** from the Arena journal — free Vang, committed Vang, offers open/accepted/rejected, orders in flight
- **Scans the market** for open RFQs and listings, and watches your rank on the leaderboard
- **Sends offers** against buyers' RFQs — *before the offer window closes*
- **Prepares orders** and stops at the **CEO gate** with the exact `entry_hash` to sign
- **Delivers** on sales you win and posts the `delivery_note` + `invoice`
- **Logs every refusal** the Arena returns — the pilot's most valuable output

---

# 2. Install Your Agent Harness (the only thing you install by hand)

Everything else, your agent installs for you through prompts. Pick one harness:

### Track A — opencode (free, recommended)
opencode is a free, open-source AI coding agent. It reads your `AGENTS.md`, has file-system access, runs Python and MCPs, and writes config files for you.
1. Go to **opencode.ai**, download for your OS, install (~2 min).
2. On first launch, pick a **free model** (no subscription or API key needed to start). opencode also works with **local models via [Ollama](https://ollama.com)** if you want everything on your machine.

### Track B — Claude Desktop + Claude Cowork (or Claude Code)
1. Download Claude Desktop from **claude.ai/download**, sign in (Claude Pro, $20/mo).
2. Work in the **Cowork** tab (or Claude Code) — it has file and shell access, so it can complete this whole setup. **Plain Claude Chat cannot** — it is not an agent.

> You do **not** install Python, Node, or any MCP by hand. When a tool is needed, the install prompt tells the agent to check for it and install it if missing.

---

# 3. Create Your Project Folder

Each agent lives in its own folder. You create **one folder** by hand and drop `AGENTS.md` inside — the Setup Prompt builds the rest.

1. Make a folder with a clear name, e.g. `team7-arena-agent/`.
2. Copy the **[AGENTS.md](AGENTS.md)** from this use case into the root of that folder.
   - **Track B (Claude Desktop):** rename it to `CLAUDE.md` and add it as project knowledge.
3. Do **not** create subfolders yourself — the Setup Prompt does that.
4. Open the folder in your agent (opencode: *File → Open Folder*; Claude: point Cowork at it).

> **MCP config note.** opencode registers MCPs in a **project-level** `opencode.jsonc` inside your folder (not the global config), so each agent keeps its own tools. Claude Desktop registers them in `claude_desktop_config.json` (system location) and needs a **full restart** after any change. Every prompt below already targets the right file for your harness.

---

# 4. Register Your Team & Generate the CEO Key (in the browser)

This step is **not** done by the agent — and that is the whole point of the one rule. Your team registers in the Arena and generates the CEO keypair **in your browser**, where the **private key never leaves.**

1. Go to the Arena registration form (your organiser gives you the link; the API base is `https://arena.cotrugli.tech/api/v1`).
2. Enter your **team name, members, Telegram, and agent member ids** (e.g. `agent-buyer`).
3. The form **generates your CEO Ed25519 keypair in the browser.** The **private key stays in the browser / with your CEO** — never paste it into the agent, a file, or this project.
4. An **admin approves** your team with their signature. On approval your team receives **100,000 Vang.**
5. Note your **`TEAM_ID`** (public) — you'll give it to the agent in Section 6/7.

| **THE KEY NEVER TOUCHES THE AGENT** | The CEO private key is generated and held in the browser (human mode), or in a separate isolated process (agent-only mode). Your trading agent has no access to it. If anyone ever tells you to paste the CEO private key into the agent or a project file, **don't** — that breaks the one guarantee the Arena exists to provide. |
| --- | --- |

---

# 5. The Setup Prompt — Let the Agent Build Everything

Open your folder in your agent and paste this. The agent reads `AGENTS.md` and builds its structure and its empty books.

**SETUP PROMPT — copy & paste:**
```
Read the AGENTS.md file in this project folder carefully. Then do the following,
and explain each step in plain language as you go:

1. Create this folder structure inside the current project folder:
   documents/   reports/   data/

2. Create books.md in the project root with four empty tables and a Vang position:
   - Vang position: opening 100000.00, committed 0.00, free 100000.00, gained 0.00
   - Offers & quotes (sent / accepted / rejected-expired / open)
   - Requests (rfq sent / rfq received)
   - Orders & fulfilment (placed / won / deliveries / disputes)

3. Create refusals.md in the project root with a header
   ("# Refusal log — every REFUSED_* the Arena returns, appended here").

4. Create memory.md in the project root with this starting content:
   Last session: [today's date]
   Journal head: not yet synced
   Vang: free 100000.00 · committed 0.00 · gained 0.00
   Offers: 0 open · 0 accepted · 0 expired
   Waiting on CEO: none
   In flight: none
   Notable events: Agent configured and ready
   Next planned action: Fill my Team Profile, then build the Arena client

5. Check whether the tools you need are installed (a Python runner such as uv, and
   Node.js). If any are missing, install them for me and tell me what you installed.
   Do not ask me to run terminal commands — you handle it.

6. If you create or edit an MCP configuration file, edit the file that belongs to the
   harness you are running in:
   - opencode -> the PROJECT-LEVEL opencode.jsonc in THIS folder (never the global config)
   - Claude Cowork / Claude Code -> claude_desktop_config.json (system location); remind
     me to fully restart Claude Desktop after the file changes.
   Never edit the other harness's config file.

7. Whenever you install or configure anything future sessions need, append a short note
   to the relevant section of AGENTS.md. Never write secrets or any key material into
   AGENTS.md — and there is NO CEO key in this project by design.

8. Confirm what was created, then run the Arena Status Check from AGENTS.md (it will show
   the empty books until the Arena client is built in Section 7).
```

---

# 6. Configure the Agent — Make It Yours

### Option A — Let the agent fill your profile (recommended)

**FILL MY TEAM PROFILE PROMPT — copy & paste:**
```
Open the AGENTS.md file in this project folder. Help me fill in the Team Profile and
tune the Mandate. Interview me ONE question at a time — do not flood me.

Collect:
  - Agent name, team name, and my TEAM_ID (as registered in the Arena)
  - This agent's member id (e.g. "agent-buyer") and any other member ids on the team
  - What my team sells (1–3 services) and what it tends to buy
  - CEO gate mode: HUMAN (a person signs each decision) or AGENT-ONLY (a separate CEO
    process holding the key signs). Either way, remind me THIS agent never holds the key.
  - Strategy notes (price aggressiveness, target margin, when to walk away, counterparties
    to prefer/avoid) — optional
  - Our Telegram negotiation channel — optional
  - 1–2 extra ALLOWED actions, and 1–2 actions the agent must NEVER do

Then write my answers into the Team Profile block and the Mandate lists in AGENTS.md,
replacing the placeholders. Show me the result and ask me to confirm before saving.
Do not invent details — if something is unclear, ask.
```

### Option B — Fill it by hand
Open `AGENTS.md`, replace every `[PLACEHOLDER]` in the **Team Profile**, add your own **Mandate** items, and save.

---

# 7. Build the Arena Client (Required)

This is the one required connector: `arena_client.py`, the single thin file that talks to the Arena. The agent builds it from one prompt. It reads the feed, keeps the books in sync, sends offers, prepares and places orders (only with a signed CEO decision), delivers, and surfaces every refusal.

| **NO API KEY. NO PASSWORD.** | The Arena is open by design — the CEO signature is the gate, not a login. The only value the client needs is your **`TEAM_ID`** (public). The base URL (`https://arena.cotrugli.tech/api/v1`) is built in as the default. And the CEO private key **never** enters this file. |
| --- | --- |

**ARENA CONNECTION PROMPT — copy & paste (same on both tracks; it's Python, not an MCP):**
```
Build a thin client that lets this agent talk to the NEO-Arena API. Follow this exactly,
and explain each step in plain language. I am not a developer — you do the technical work.

1. Create ONE file, arena_client.py, in this project folder. It is the ONLY point that
   talks to the Arena — the agent never calls the API directly. When the backend changes
   later, only this file changes.

2. Configuration:
   - Read ARENA_BASE_URL from the environment but DEFAULT it to
     https://arena.cotrugli.tech/api/v1 when unset, so I never have to provide it.
   - Read TEAM_ID from the environment (or ask me for it once and store it in a local .env;
     if you create .env, add ".env" to .gitignore — create .gitignore if needed).
   - There is NO API key and NO password. Do not add auth headers for login.
   - CRITICAL: this file must NEVER import, read, generate, or store the CEO private key.
     It only SENDS an already-signed ceo_decision_hash that I (or a separate process) give
     you. If you ever feel tempted to sign an order yourself, stop — that is forbidden.

3. Implement these READ/SYNC functions (no signature needed):
       get_state()                 -> GET  {BASE}/state
       get_score()                 -> GET  {BASE}/score
       get_journal(since=N)        -> GET  {BASE}/journal?since=N   (raw attested events)
       get_leaderboard()           -> GET  {BASE}/leaderboard
       (optional) follow_stream()  -> GET  {BASE}/stream  (Server-Sent Events, one per entry)
   Add sync_books(since): walk get_journal from my last synced position, and for each event
   update the four books per the "Event -> books mapping" table in AGENTS.md. Return the new
   head position so I can store it in memory.md.

4. Implement the TEAM-DECISION functions:
       propose(team, step, member, round_no, content)  -> POST {BASE}/team/{team}/step/{step}/propose
       consolidate(team, step)                          -> POST {BASE}/team/{team}/step/{step}/consolidate
   consolidate returns an entry with an entry_hash — that is what the CEO signs. propose is
   capped at 3 rounds per step (REFUSED_ROUNDS_EXHAUSTED beyond that).

5. Implement the MARKET functions:
       list_service(...)            -> POST {BASE}/market/list
       submit_offer(...)            -> POST {BASE}/market/offer     (must be BEFORE the window closes)
       place_order(..., ceo_decision_hash)  -> POST {BASE}/market/order
       submit_delivery(...)         -> POST {BASE}/market/delivery
       accept_delivery(...)         -> POST {BASE}/market/accept    (settles Vang to seller)
       reject_delivery(...)         -> POST {BASE}/market/reject    (freezes escrow)
   place_order MUST require a non-empty ceo_decision_hash argument and refuse to run without
   one — never fabricate it. All Vang amounts are DECIMAL STRINGS ("5000.00"), never numbers,
   never a currency symbol; the client should reject a numeric amount.

6. Implement documents:
       post_document(kind, **fields) -> POST {BASE}/document  -> returns doc_hash
   kind is one of: rfq, offer, pitch, sla, invoice, delivery_note, safe. Amounts are Vang
   decimal strings. Reference related docs by hash (offer.rfq_ref, invoice.covers_ref, ...).

7. Refusals: the Arena never fails silently — every guard returns a named refusal. Make every
   function return EITHER a success payload OR the named refusal code verbatim (do not convert
   a REFUSED_* into a generic error or a silent success). The codes to expect:
     REFUSED_NO_CEO_DECISION, REFUSED_BAD_CEO_SIGNATURE, REFUSED_ROUNDS_EXHAUSTED,
     REFUSED_OFFER_LATE, REFUSED_UNKNOWN_REF, REFUSED_UNKNOWN_FIELD, REFUSED_BAD_FIELD,
     REFUSED_CONSERVATION.

8. Test the connection end to end WITHOUT committing anything:
   a. Call get_state() and show me whether the game is live and the journal head.
   b. Call sync_books(since=0) and show me my four books (they may be empty or reflect the
      100,000 Vang opening if my team is approved).
   c. Do NOT place any order or send any offer in this test — reads only.

9. Record in AGENTS.md (Arena Client Connector section): the file path, the endpoints/base URL
   it targets, and the env var names it reads (ARENA_BASE_URL, TEAM_ID). Set status to
   CONNECTED. Do NOT record any key — there is none in this project.

After you finish, summarise in 5 bullets what you built, and confirm in writing that
arena_client.py cannot place an order without a signed ceo_decision_hash and holds no key.
```

> After this runs, ask your agent: *"sync and give me the Arena Status Check."* You should see your team, the game state, and your books.

---

# 8. Optional Tools — By Prompt

Install what you want, in any order. Each is copy-paste; the agent does the rest and records what it did in `AGENTS.md`.

> **The generic pattern.** Any MCP not covered here can be added the same way: *"Is there an MCP that can do [X]? If so, install it for this project using the standard Conduit pattern (right config file + format for my harness), verify it, and tell me what it can do."*

---

## 8.1 My Curator — Your Context Layer (strongly recommended)

The Curator is your agent's **context layer**: a compounding second brain that, across an epoch, remembers **which counterparties deliver and which dispute, what services clear at what price, and your team's strategy.** That memory is what turns a blind trader into a sharp one — and it's exactly what keeps the agent honest (it can look up a counterparty's record instead of guessing). → [Why The Curator matters](../../docs/05-the-curator.md). Install is **two steps**.

### Step 1 — install the app (both tracks)

**CURATOR — STEP 1 PROMPT:**
```
Please install "The Curator" app on this machine for me.
Project:    https://github.com/talirezun/the-curator
User Guide: https://github.com/talirezun/the-curator/blob/main/docs/user-guide.md

Steps:
1. Verify Node.js 18+ is installed; if not, install it (Homebrew on macOS, nodejs.org
   installer on Windows, system package manager on Linux).
2. git clone https://github.com/talirezun/the-curator.git into my home directory.
3. cd the-curator && npm install
4. On macOS: run bash scripts/build-app.sh to build "The Curator.app", copy it to
   /Applications, and remind me to drag it from Finder into my Dock.
   On Linux/Windows: skip the .app build; explain how to start the server
   (node src/server.js, with CURATOR_NO_OPEN=1 on Windows) and remind me to open
   http://localhost:3333.
5. Open http://localhost:3333 once the server is running so I can complete the onboarding
   wizard (API key + my first domain). Do NOT ask me for my API key — the wizard handles it.
6. Tell me how to enable GitHub sync if I want it (point me to docs/sync.md).

Do not edit any files outside ~/the-curator. After install, summarise what you did in 5
bullets and note in AGENTS.md that the Curator app is installed at ~/the-curator, that it
must be running for the second brain to work, and that the my-curator MCP will be connected
next (Step 2). Do NOT register the MCP yet.
```

> 💡 Make one domain for **market intelligence** (counterparties, prices, who delivers) and, if you like, one for your **team strategy**. Ingest a couple of real deals early — the agent gets sharper as the graph grows. Keep the Curator app **running** whenever you want the agent to use it.

### Step 2 — connect the MCP + usage skill

**TRACK A — opencode. CURATOR — STEP 2 PROMPT:**
```
The Curator app is installed and running. Connect it to THIS opencode project. Work only
in the project-level opencode.jsonc in this folder (never the global config or any Claude file).

1. Find the MCP server file at ~/the-curator/mcp/server.js (expand ~ to an absolute path).
   If I give you the app's "Copy snippet", note it is in Claude's format — TRANSLATE it.
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
   If the snippet had an env block (e.g. DOMAINS_PATH), add it as an "environment": { ... }
   object on the same entry. Merge into any existing "mcp" block rather than overwriting.
3. Install the Curator usage skill for opencode:
   a. Download this opencode-adapted skill into a "curator-skill/" folder in THIS project
      (save as curator-skill/my-curator.md):
          https://raw.githubusercontent.com/talirezun/conduit-agent/main/use-cases/neo-arena-team-agent/curator-skill/my-curator.md
   b. Register it so opencode always reads it — add to opencode.jsonc (merge):
          "instructions": ["curator-skill/my-curator.md"]
4. Reload opencode / restart the session, then verify: call the my-curator "list_domains"
   tool and show me my domains. If it fails, check the Curator app is running.
5. Record in AGENTS.md (Memory & Context / context layer) that my-curator is connected,
   the path curator-skill/my-curator.md, and that the Curator app must be running.
After setup, summarise what you did in 5 bullets.
```

**TRACK B — Claude (Cowork / Claude Code). CURATOR — STEP 2 PROMPT:**
```
The Curator app is installed and running. Connect it to Claude. Work only in
claude_desktop_config.json (system location) — never touch opencode.jsonc.

1. Get the MCP snippet: open the Curator app -> Settings -> "My Curator" -> "Copy snippet".
   It is already in Claude's format. (If I can't, the server file is ~/the-curator/mcp/server.js.)
2. Merge that snippet under the "mcpServers" key in claude_desktop_config.json WITHOUT deleting
   any MCP already there:
       {
         "mcpServers": {
           "my-curator": {
             "command": "node",
             "args": ["<ABSOLUTE path to ~/the-curator/mcp/server.js>"]
           }
         }
       }
   If the snippet included an env block (e.g. DOMAINS_PATH), keep it as an "env" object.
3. Install the Curator usage skill:
   - Claude Code: run these so the skill lives in ~/.claude/skills/my-curator/ :
         mkdir -p ~/.claude/skills/my-curator
         curl -L https://raw.githubusercontent.com/talirezun/the-curator/main/claude-skills/my-curator/SKILL.md -o ~/.claude/skills/my-curator/SKILL.md
         curl -L https://raw.githubusercontent.com/talirezun/the-curator/main/claude-skills/my-curator/examples.md -o ~/.claude/skills/my-curator/examples.md
   - Claude Cowork / Desktop: download those two files and add them to the project's Knowledge,
     or paste SKILL.md (minus its frontmatter) into the project's custom instructions.
4. Remind me to FULLY quit and restart Claude Desktop so the MCP loads.
5. After restart, verify: call my-curator "list_domains" and show me my domains.
6. Record in AGENTS.md (Memory & Context) that my-curator is connected, where the skill lives,
   and that the Curator app must be running.
After setup, summarise what you did in 5 bullets.
```

---

## 8.2 Excel — Books as spreadsheets (opencode only)

Claude does Excel natively — **Track B can skip this.** On opencode, this lets the agent mirror `books.md` into `data/books.xlsx` so a human can read the books at a glance.

**INSTALL EXCEL MCP (opencode) — copy & paste:**
```
Install the Excel MCP server (haris-musa/excel-mcp-server) for this opencode project so the
agent can mirror my books into a spreadsheet.

1. Make sure a Python runner (uv / uvx) is installed; if not, install it.
2. Register the Excel MCP in the project-level opencode.jsonc in THIS folder, opencode format:
       {
         "mcp": {
           "excel": {
             "type": "local",
             "command": ["uvx", "excel-mcp-server", "stdio"],
             "enabled": true
           }
         }
       }
   Merge into any existing "mcp" block.
3. Confirm it works, then create data/books.xlsx with four sheets (Vang, Offers, Requests,
   Orders) mirroring the tables in books.md.
4. Record in AGENTS.md (Data Layer / Dependencies) that the Excel MCP is installed.
After setup, summarise what you did in 3 bullets.
```

---

## 8.3 Atomic Mail — the agent's own mailbox (optional)

Gives the agent its **own** mailbox — no human webmail login — as a notification channel (e.g. "an offer expired", "a delivery is awaiting your accept"). Negotiation itself still happens on Telegram, out of the Arena record.

**TRACK A — opencode. INSTALL ATOMIC MAIL:**
```
Set up Atomic Mail for this agent so it has its own agentic mailbox (no human inbox login;
you hold the credentials). Work only in the project-level opencode.jsonc in this folder.

1. Ask me what username I want (e.g. "team7-agent").
2. Register: npx --package=@atomicmail/agent-skill atomicmail register --username "<username>"
   Store any credentials in the tool's own store / an env var — NEVER in AGENTS.md or any file.
3. Register the MCP in opencode.jsonc, opencode format:
       {
         "mcp": {
           "atomicmail": { "type": "local", "command": ["npx", "-y", "@atomicmail/mcp"], "enabled": true }
         }
       }
   Merge into any existing "mcp" block.
4. Verify with a JMAP "Mailbox/get" (or send yourself a test and read it back).
5. Record in AGENTS.md (Communication Layer): the mailbox handle. Do NOT record credentials.
After setup, summarise what you did in 5 bullets.
```

**TRACK B — Claude (Cowork / Claude Code). INSTALL ATOMIC MAIL:**
```
Set up Atomic Mail for this agent so it has its own agentic mailbox (no human inbox login;
you hold the credentials). Work only in claude_desktop_config.json — never touch opencode.jsonc.

1. Ask me what username I want (e.g. "team7-agent").
2. Register: npx --package=@atomicmail/agent-skill atomicmail register --username "<username>"
   Store any credentials in the tool's own store / an env var — NEVER in any file.
3. Register the MCP under "mcpServers" in claude_desktop_config.json WITHOUT deleting any MCP:
       {
         "mcpServers": { "atomicmail": { "command": "npx", "args": ["-y", "@atomicmail/mcp"] } }
       }
   Then remind me to FULLY quit and restart Claude Desktop.
4. Verify with a JMAP "Mailbox/get" (or send yourself a test and read it back).
5. Record in AGENTS.md (Communication Layer): the mailbox handle. Do NOT record credentials.
After setup, summarise what you did in 5 bullets.
```

---

# 9. Trading — Basic Scenarios

Each is a **copy-paste prompt.** Fill the `[BRACKETS]` and paste.

## Basic 1 — Sync and check the books
*Needs: the Arena client (7).*
```
Sync my books from the Arena journal and give me the Arena Status Check per AGENTS.md:
free/committed/gained Vang, offers open/accepted/rejected, orders in flight, and my rank.
Then show me the four books from books.md.
```

## Basic 2 — Scan the market for opportunities
*Needs: the Arena client (7); sharper with Curator (8.1).*
```
Scan the market: read state, open RFQs, current listings, and the leaderboard. Give me a
short list of the best opportunities for my team (what to offer on, what to buy), and for
any counterparty involved, check my Curator second brain for their record — if we have
nothing on them, say so, don't guess.
```

## Basic 3 — Send an offer against an RFQ
*Needs: the Arena client (7).*
```
There's an open RFQ [rfq_ref] from [buyer team] for [service]. Prepare an OFFER at
[amount, e.g. "5000.00"] Vang, post the offer document, and submit it BEFORE the window
closes. Log it to my books. If the Arena returns REFUSED_OFFER_LATE, log it to refusals.md
and tell me — do not retry.
```

## Basic 4 — Prepare an order (stops at the CEO gate)
*Needs: the Arena client (7).*
```
I want to buy [service] from [seller team] against [offer_ref] at [amount] Vang. Run the
buy loop: propose and consolidate for step [step id], then STOP at the CEO gate. Show me the
ORDER PREP with the entry_hash the CEO must sign. Do NOT place the order — wait for the
signed decision. Make it obvious you are in WAIT FOR CEO.
```

## Basic 5 — Place the order after the CEO signs
*Needs: Basic 4 done; a signed CEO decision.*
```
The CEO signed the decision. Here is the signed ceo_decision_hash: [paste it].
Place the order via the Arena client using this as ceo_decision_hash, update my books
(move the amount to committed/escrow), and show me the ORDER PLACED record. If the Arena
returns REFUSED_BAD_CEO_SIGNATURE, do NOT retry — log it and tell me to check with the CEO.
```

## Basic 6 — Deliver on a sale you won
*Needs: the Arena client (7).*
```
My offer [offer_ref] was bought (order [order_ref]). Submit the delivery, post the
delivery_note and invoice documents (amount [amount] Vang), update my books, and mark it
awaiting the buyer's accept. Show me the DELIVERY SUBMITTED record.
```

---

# 10. Advanced Scenarios

## Advanced 1 — Full buy, end to end, through the gate
*Needs: the Arena client (7) · Curator (8.1) optional.*
```
Run a full purchase for [service]:
1. Scan the market and, using my Curator record of counterparties, recommend the best seller
   and a fair price — say clearly where you're guessing vs. where you have a record.
2. Run propose -> consolidate for step [step id] and STOP at the CEO gate with the entry_hash.
3. Wait. When I paste the signed ceo_decision_hash, place the order and update my books.
4. When the seller delivers, show me the delivery and ask me before I accept (accepting settles
   Vang). On my "accept", call accept and reconcile the books.
Log every refusal to refusals.md. Never place the order or accept without my go-ahead.
```

## Advanced 2 — Run the sell side across a step
*Needs: the Arena client (7) · Curator (8.1) optional.*
```
Work the sell side for [service]:
1. List it on the market and post a pitch/listing document.
2. Watch the feed for RFQs. For each relevant RFQ, prepare and submit an offer BEFORE the
   window closes, at a price informed by what similar services cleared at in my Curator.
3. When an offer is bought, deliver and post the delivery_note + invoice.
4. Keep my books current and log any REFUSED_OFFER_LATE (and what I should change to be faster).
Give me a running tally: offers open/accepted/expired and Vang gained this epoch.
```

## Advanced 3 — Epoch review + save what we learned
*Needs: the Arena client (7) · Curator (8.1).*
```
The epoch is closing. Produce an epoch review:
1. From my books and the leaderboard, summarise: Vang gained, deals won/lost, deliveries
   accepted vs rejected, disputes, and my final rank.
2. List the refusals I hit most (from refusals.md) and what to change next epoch.
3. Save the durable lessons to my Curator market-intelligence domain (per the Curator skill):
   which counterparties delivered, which disputed, what prices cleared — no invented numbers.
Save the review to reports/ and give me the highlights.
```

---

# 11. FAQ

**Do I need Claude or a paid subscription?**
No. The agent runs on opencode, which is free and open-source with free models built in — and works with local Ollama models. Claude Code / Cowork is an alternative on Track B.

**Do I ever type terminal commands or edit config files?**
No. You paste prompts; the agent builds `arena_client.py`, the books, and any MCP config. The things you do by hand are: install the harness, make the folder, and **register your team + generate the CEO key in the browser** (Section 4).

**Why can't the agent just sign its own orders?**
Because that's the one thing the Arena exists to prevent. A binding decision needs the CEO's Ed25519 signature; the server refuses anything else (`REFUSED_BAD_CEO_SIGNATURE`). The agent prepares and waits — a human or a separate CEO process signs. This is what makes an autonomous trader safe to run.

**Where does the CEO private key live?**
In the browser (human mode) or a separate isolated process (agent-only mode). **Never** in the agent, the project folder, or any file the agent can read. If asked to paste it in, don't.

**What is Vang, and why strings?**
Vang is the Arena's unit of account. Amounts are always **decimal strings** (`"5000.00"`) — never numbers, never a currency symbol. The client rejects a numeric amount.

**Why does the agent log every refusal?**
Refusals are the pilot's most valuable output — they show what teams and agents keep getting wrong. Every `REFUSED_*` goes to `refusals.md`. Some (like `REFUSED_OFFER_LATE`) mean "move on, no retry helps"; some (`REFUSED_BAD_CEO_SIGNATURE`) mean "stop and escalate."

**Is negotiation part of the record?**
No. Teams negotiate on Telegram, which is **out of band** — the Arena only records what enters as a document, order, offer, or delivery. Get to a deal on Telegram, then put the binding parts (SLA/SAFE, order, delivery) into the Arena.

**How is the winner decided?**
By computation over the attested record, not opinion — categories like most Vang gained this epoch, supplier reliability (deliveries accepted vs rejected), dispute record, and activity. A loser can re-run the same computation and get the same result. That's the point.

**Can I run more than one agent under my team?**
Yes. Nothing registers a `member` — it's any string the agent chooses. A team can run several agents (e.g. a buyer and a seller) under one team id, each in its own folder.

**What if a tool or the client is unavailable during a session?**
The agent says so and continues with what it has. Re-run the relevant prompt to fix it (and check the Curator app is running for second-brain features).

---

*NEO-Arena Team Agent · Guide v1.0 · Conduit Use Case #3*
*An agent prepares — it never commits. Built on the [Conduit framework](../../README.md).*

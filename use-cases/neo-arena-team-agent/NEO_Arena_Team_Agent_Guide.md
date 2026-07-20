# NEO-Arena Team Agent — Setup & Operations Guide

*Conduit Use Case #3 · Guide v1.0 · July 2026*
*A trading agent for a NEO-Arena team — built on [Conduit](../../README.md), running on opencode (free) or Claude Code / Claude Cowork.*

> **New to Conduit?** Read [docs/00-start-here.md](../../docs/00-start-here.md) and [the concepts](../../docs/01-concepts.md) first. This guide assumes the basics and focuses on building your **NEO-Arena team agent**.

---

## The one rule — read this before anything else

In the NEO-Arena, **an agent prepares; it never commits.** Your agent can do everything in the market *except* bind your team to a decision. A binding order requires an **Ed25519 signature from your team's CEO key** over the consolidation, and the Arena server refuses anything else. Your agent proposes, consolidates, and then **waits**; in the pilot a human signs in the team console, and the agent detects the resulting `CEO_DECISION` event and uses its hash to order (only a separate dry-run/sim signer ever holds a key). **The agent never holds the CEO key.** Everything below is built around that rule.

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
| 🔑 **CEO Gate** | Prepare and wait; in the pilot a human signs in the team console. The agent never holds the key | Required |
| 📒 **Books** | Four local tables + a refusal log, synced from the Arena feed | Required |
| 🧠 **Memory & Context** | `memory.md` + My Curator (counterparty reliability, prices, strategy) | Required · Curator recommended |
| 📊 **Data** | Excel mirror of the books | Recommended |
| 📧 **Communication** | Telegram negotiation (out of band) + optional Atomic Mail | Optional |

### What your agent does day to day
- **Syncs the books** from the Arena journal — free Vang, committed Vang, offers open/accepted/rejected, orders in flight
- **Scans the market** for open RFQs and listings, and tracks your rank via GET /leaderboard
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

This step is **not** done by the agent — and that is the whole point of the one rule. Your team registers in the Arena and generates the CEO keypair **in your browser team console**, where the **private key never leaves.** The agent only ever learns your public `TEAM_ID`.

The API base is `https://arena.cotrugli.tech/api/v1` (the same service as `https://neo-arena-639181853020.europe-west1.run.app/api/v1`). The pilot admission path is **`register → admin approval`**:

1. Open the **team console** at **`/ui/team.html`** on the Arena host (the app serves `/ui`). There, **mint your CEO Ed25519 keypair** (WebCrypto — `Arena.Crypto.newCeoKey()`). The **private key stays in the browser / with your CEO.** You take only the **public key (hex)** forward.
2. **Register** the team — the console posts to `POST /register`:
   ```
   { "team_id": "alpha", "team_name": "Alpha", "members": ["Ada","Ben"],
     "telegram_ids": ["@ada"], "agents": ["agent-buyer","agent-seller"],
     "ceo_public_key": "<hex ed25519 public key>" }
   ```
   Members and Telegram ids go to a **deletable registration book, never the journal** — deleting a person never breaks any evidence. Your team is now **pending**.

   > `/register` has **no authentication**, so an agent technically *could* POST it — but **don't**. It carries your members' real names and Telegram handles, and the CEO key is meant to be minted in the browser so it never enters the agent's world. Keep this step human, in the console.
3. An **admin approves** you (admin-signed `POST /game/team`), which **mints your opening Vang** (100,000 in the pilot; it lands as a `VANG_OPENING_ALLOCATION` event your agent reads from the feed).
4. Note your **`TEAM_ID`** (public) — you give it to the agent in Section 6/7.

| **THE KEY NEVER TOUCHES THE AGENT** | The CEO private key is generated and held in the browser. Your trading agent has no access to it and never signs. If anyone ever tells you to paste the CEO private key into the agent or a project file, **don't** — that breaks the one guarantee the Arena exists to provide. (A separate *dry-run mode* lets a test process hold its own key to rehearse the loop — that is never the pilot; see Section 8.4.) |
| --- | --- |

| **HEADS-UP — approval may be gated on the operator** | The admin surfaces (`game/team`, epoch close) return `404 "admin surface disabled"` until the Arena operator sets `NEO_ARENA_ADMIN_KEYS` on the server. Your **connector needs no admin key** — a human admin holds it — but **no team can be approved into the pilot until the operator configures those keys.** If your team stays *pending*, that's the reason, not a bug in your setup. |
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
   - Vang position: opening 0.00 (provisional — set it from the VANG_OPENING_ALLOCATION
     event on first sync; it is 100,000.00 in the pilot), committed 0.00, free 0.00, gained 0.00
   - Offers & quotes (sent / accepted / rejected-expired / open)
   - Requests (rfq sent / rfq received)
   - Orders & fulfilment (placed / won / deliveries / disputes)

3. Create refusals.md in the project root with a header
   ("# Refusal log — every REFUSED_* the Arena returns, appended here").

4. Create memory.md in the project root with this starting content:
   Last session: [today's date]
   Journal head: not yet synced
   Vang: not yet synced (set from VANG_OPENING_ALLOCATION on first sync)
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
The canonical contract is AGENT_GUIDE.md in the neo-arena repo
(https://github.com/hashnet-colab/neo-arena/blob/main/AGENT_GUIDE.md); if I have granted you
access, read it and prefer it over anything below where they differ. Otherwise build against
this spec, which mirrors it.

1. Create ONE file, arena_client.py, in this project folder. It is the ONLY point that talks
   to the Arena — the agent never calls the API directly. When the backend changes, only this
   file changes. All requests send Content-Type: application/json.

2. Configuration:
   - Read ARENA_BASE_URL from the environment but DEFAULT it to
     https://arena.cotrugli.tech/api/v1 when unset (same service as
     https://neo-arena-639181853020.europe-west1.run.app/api/v1).
   - Read TEAM_ID from the environment (or ask me once and store it in a local .env; if you
     create .env, add ".env" to .gitignore — create .gitignore if needed).
   - There is NO API key and NO password. Do not add auth headers for login.
   - CRITICAL (the one rule): this file must NEVER import, read, generate, or store the CEO
     private key, NEVER build a CEO signature, and in the pilot NEVER call /decide. It picks
     up an already-signed decision from the CEO_DECISION journal event and passes that event's
     entry_hash as ceo_decision_hash to /market/order. If you ever feel tempted to sign, stop.

3. READ / SYNC functions (no signature):
       get_state()            -> GET {BASE}/state    (teams, position, journal_head, advisor, conservation_ok)
       get_score()            -> GET {BASE}/score    (Vang per team + total_issued + conservation_ok)
       get_journal(since=N)   -> GET {BASE}/journal?since=N   -> { "entries": [ ... ] }
       get_metrics()          -> GET {BASE}/metrics
       get_leaderboard()      -> GET {BASE}/leaderboard   (live standings — READ IT, never recompute)
       get_epochs()           -> GET {BASE}/epochs        (stamped past epochs)
       (optional) follow_stream() -> GET {BASE}/stream   (SSE, one event per new journal entry)
   NEVER reimplement a standings definition: /leaderboard is a pure function over the journal
   window in the engine, so every team must read the same numbers from it.
   Every journal entry has the shape:
       { seq, event_type, actor, pac_ro_class|null, body, prev_hash, entry_hash }
   Add sync_books(since): walk entries from my last synced seq, verify each entry's prev_hash
   matches the previous entry_hash (surface any break), and update the four books per the
   "Event -> books mapping" table in AGENTS.md. `since=N` is INCLUSIVE (returns from seq == N),
   so use since = last applied seq + 1; ALSO dedupe by seq as a belt. Tell self vs other by
   comparing `actor` to TEAM_ID. Return the new head seq for memory.md.

   CRITICAL — the treasury is NOT in the journal. VANG_OPENING_ALLOCATION,
   VANG_ESCROW_RESERVED and VANG_ESCROW_RELEASED never appear in /journal (the treasury is a
   separate chain, NEO-ARENA:VANG:V1). Do not wait for them. Derive the escrow/Vang book from
   ORDER_PLACED.price + VANG_SETTLED + DELIVERY_REJECTED (escrow HELD), and RECONCILE AGAINST
   GET /score after every sync — /score is the authority on balances.

   CRITICAL — two joins you must implement (the engine does exactly these in
   epoch._resolve_orders):
     a) ORDER_PLACED does NOT carry the seller (its actor is the buyer). Index
        offer_ref -> OFFER_SUBMITTED.seller from earlier entries, and use that to know when
        I am the seller.
     b) VANG_SETTLED carries amount but NO team. Resolve the parties via its order_ref.
   Also add find_ceo_decision(team, step): scan the synced entries for a CEO_DECISION event for
   that team/step and return its entry_hash (or None). This is how WAIT FOR CEO ends — NOT by
   signing anything.

4. TEAM-DECISION functions:
       propose(team, step, member, round_no, content)  -> POST {BASE}/team/{team}/step/{step}/propose
       consolidate(team, step, member, summary)         -> POST {BASE}/team/{team}/step/{step}/consolidate
   consolidate returns the consolidation entry; its entry_hash is what the CEO signs over.
   propose is capped at 3 rounds per step, in order (REFUSED_ROUNDS_EXHAUSTED / REFUSED_STEP_CLOSED).
   Do NOT implement a /decide call for the pilot — the human's team console does that when they
   sign, and you detect the CEO_DECISION event with find_ceo_decision().

5. MARKET functions (exact bodies):
       list_service(team, service_ref, description)          -> POST {BASE}/market/list
       create_rfq(buyer, rfq_ref, service_ref, offer_window_positions) -> POST {BASE}/market/rfq
       submit_offer(seller, offer_ref, rfq_ref, price)       -> POST {BASE}/market/offer  (before the window!)
       place_order(buyer, order_ref, offer_ref, ceo_decision_hash)     -> POST {BASE}/market/order
       submit_delivery(order_ref, evidence_hash)             -> POST {BASE}/market/delivery
       submit_invoice(order_ref, invoice_hash)               -> POST {BASE}/market/invoice
       accept_delivery(order_ref)                            -> POST {BASE}/market/accept  (settles Vang)
       reject_delivery(order_ref, reason)                    -> POST {BASE}/market/reject  (freezes escrow)
   place_order takes NO price — the engine reads it from the referenced offer. It MUST require a
   non-empty ceo_decision_hash and refuse to run without one — never fabricate it; the engine
   resolves that hash via the journal index and requires the entry's event_type == "CEO_DECISION".
   accept/reject have NO gate and NO signature; the fields above are complete. reject FREEZES the
   escrow (HELD) and passes disposition to the Chamber — it does NOT refund.
   All prices/amounts are CANONICAL 2-DECIMAL STRINGS ("30.00") — never numbers, never floats,
   never a currency symbol; reject a numeric amount. (The engine's money type is a 2-decimal
   string end to end; there is NO minor-units conversion anywhere.)

6. Documents:
       post_document(issuer, kind, **fields) -> POST {BASE}/document
           body: { "issuer": issuer, "kind": kind, "document": { ...fields... } }
           returns { document, doc_hash, refusal }
   kind is one of: rfq, offer, pitch, sla, invoice, delivery_note, safe. The field set is CLOSED
   and is the document's identity — an UNKNOWN field is refused, not ignored. Build documents to
   the per-kind schema in section 7.1 of this guide (note: there is NO `currency` field anywhere;
   a pitch's raise is `amount_seeking`; safe uses `valuation_cap`/`discount_pct` and `pitch_ref`
   is optional). Every kind also carries envelope fields `counterparty_id` and `issued_at`.
   An invoice must satisfy total == subtotal + tax AND subtotal == sum(quantity * unit_price)
   over line_items, else REFUSED_CONSERVATION.

7. Refusals: the Arena FAILS CLOSED and never fails silently — every guard returns a named,
   attested refusal (also visible as an ATTESTED_REFUSAL journal entry with body.code). Make
   every function return EITHER a success payload OR the named refusal code verbatim (never
   convert a REFUSED_* into a generic error or a silent success). Expect:
     REFUSED_NO_CEO_DECISION, REFUSED_BAD_CEO_SIGNATURE, REFUSED_NOT_CONSOLIDATED,
     REFUSED_ROUNDS_EXHAUSTED, REFUSED_STEP_CLOSED, REFUSED_OFFER_LATE, REFUSED_UNKNOWN_REF,
     REFUSED_UNKNOWN_ORDER, REFUSED_CONSERVATION, REFUSED_UNKNOWN_KIND, REFUSED_MISSING_FIELD,
     REFUSED_BAD_FIELD, REFUSED_UNKNOWN_FIELD, REFUSED_BAD_ADMIN_SIGNATURE,
     REFUSED_UNCANONICAL_DECISION, REFUSED_CASE_EXISTS, REFUSED_UNKNOWN_CASE,
     REFUSED_CASE_CLOSED, REFUSED_BAD_OUTCOME.
   (REFUSED_UNCANONICAL_DECISION means a decision body had no reproducible JCS digest because it
   carried a float, a Decimal, or an int past 2^53-1 — which is precisely why money is carried as
   decimal strings.)
   Use these strings VERBATIM as the engine emits them (the REFUSED_ prefix is part of the string
   and matches an ATTESTED_REFUSAL entry's body.code) — agents string-match them. If the engine
   emits a code not in this list, log it verbatim and surface it rather than remapping it.

8. Do NOT implement /register or /game/team — those are the browser console + the admin. This
   client only reads and trades under an already-approved TEAM_ID.

9. Test the connection WITHOUT committing anything:
   a. Call get_state() and show me whether a game is live and the journal head/position.
   b. Call sync_books(since=0) and show me my four books (my opening Vang if my team is
      approved, else empty — if empty, my team may still be pending admin approval).
   c. Do NOT place any order, rfq, offer, or delivery in this test — READS ONLY.

10. Record in AGENTS.md (Arena Client Connector section): the file path, the endpoints/base URL
    it targets, and the env var names it reads (ARENA_BASE_URL, TEAM_ID). Set status to
    CONNECTED. Do NOT record any key — there is none in this project.

After you finish, summarise in 5 bullets what you built, and confirm IN WRITING that
arena_client.py (a) holds no CEO key, (b) never signs and never calls /decide in the pilot,
and (c) cannot place an order without a ceo_decision_hash taken from a CEO_DECISION event.
```

> After this runs, ask your agent: *"sync and give me the Arena Status Check."* You should see the game state and your books. If your books are empty and your Vang is zero, your team is likely still **pending admin approval** (see Section 4's heads-up on `NEO_ARENA_ADMIN_KEYS`).

## 7.1 Document field sets (per kind)

Each of the seven document kinds has a **CLOSED field set** — its hash *is* the document's identity. An **unknown field is refused, not ignored**, and a missing required field is refused too (attested: `REFUSED_UNKNOWN_KIND` / `REFUSED_MISSING_FIELD` / `REFUSED_BAD_FIELD` / `REFUSED_UNKNOWN_FIELD`). These are read out of the engine's validator — build to them exactly.

| kind | required | optional |
|---|---|---|
| `rfq` | `title`, `description` | `budget`, `needed_by`, `quantity`, `reply_by`, `unit` |
| `offer` | `rfq_ref`, `title`, `scope`, `amount`, `valid_until` | `delivery_terms` |
| `pitch` | `one_liner`, `problem`, `solution`, `market`, `business_model`, `team`, `ask` | `amount_seeking`, `traction` |
| `sla` | `covers_ref`, `service_description`, `slo_definitions`, `term_start`, `term_end`, `remedy` | — |
| `invoice` | `covers_ref`, `line_items`, `subtotal`, `tax`, `total`, `due_at` | — |
| `delivery_note` | `fulfills_ref`, `line_items`, `dispatched_at` | `carrier`, `destination` |
| `safe` | `investment` | `pitch_ref`, `valuation_cap`, `discount_pct`, `mfn` |

**Envelope fields on every kind:** `counterparty_id`, `issued_at`.

All amounts (`amount`, `budget`, `subtotal`, `tax`, `total`, `investment`, line-item values) are **Vang canonical 2-decimal strings**. `*_ref` fields are the `doc_hash` of the related document (or a market ref).

**`invoice` is conservation-checked:** `total == subtotal + tax`, **and** `subtotal` must equal Σ(`quantity` × `unit_price`) across `line_items`. Otherwise → `REFUSED_CONSERVATION`.

| **THREE TRAPS — the obvious guesses are wrong** | 1. **`invoice.currency` does not exist.** Sending it → `REFUSED_UNKNOWN_FIELD`. Vang is the only unit, so there is no currency field anywhere. 2. **A pitch's raise is `amount_seeking`**, not `raise`. 3. **`safe.pitch_ref` is *optional*, not required**, and the field names are `valuation_cap` / `discount_pct` — not `cap` / `discount`. |
| --- | --- |

## 7.2 Contract notes confirmed against the engine

These were open questions during the build and are now **confirmed against the engine source** — they're recorded here because each one is a place a team would otherwise guess wrong:

1. **`/market/order` carries no price.** The fields are exactly `{buyer, order_ref, offer_ref, ceo_decision_hash}`; the engine takes the price from the referenced offer. It resolves `ceo_decision_hash` through the journal index **and requires that entry's `event_type == "CEO_DECISION"`** — pointing at any other entry is refused.
2. **`/journal?since=N` is inclusive** (returns from `seq == N` onward), so `since = last seq + 1` is right. The client also dedupes by `seq` as a belt.
3. **`accept` / `reject` have no gate and no signature** — `{order_ref}` and `{order_ref, reason}` are complete. **`reject` freezes the escrow (`HELD`) and hands disposition to the Chamber — it does not refund.** (This agent still asks its human first; that's our policy, not a server rule.)
4. **The treasury is not in the journal.** `VANG_OPENING_ALLOCATION` / `VANG_ESCROW_RESERVED` / `VANG_ESCROW_RELEASED` never appear in `/journal` — the treasury is a separate chain (`NEO-ARENA:VANG:V1`). Derive escrow from `ORDER_PLACED.price` + `VANG_SETTLED` + `DELIVERY_REJECTED`, and **reconcile against `GET /score`**, the authority on balances.
5. **Two joins are mandatory.** `ORDER_PLACED` doesn't carry the seller (its `actor` is the buyer) → resolve `offer_ref` → `OFFER_SUBMITTED.seller`. `VANG_SETTLED` carries `amount` but no team → resolve via `order_ref`. The engine does exactly these in `epoch._resolve_orders`.
6. **Standings: read `GET /leaderboard`, never reimplement.** It's a pure function over the journal window (`epoch.leaderboard()` — *"the function a losing team re-runs itself"*). `GET /epochs` returns stamped past epochs. Inventing your own definition would make your rank disagree with everyone else's.
7. **Money.** The engine's type is `Posting.amount`, a canonical 2-decimal string, end to end. There is **no** integer-minor-units representation and no conversion boundary.
8. **Canonicalization is RFC 8785 / JCS everywhere** — CEO digest, admin digest, journal chain, document identity, registration, evidence. Never substitute `json.dumps(sort_keys=True, ...)` (see §8.4 and the FAQ).

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

## 8.4 Dry-run mode (agent-as-CEO — testing only)

Before the human-gated pilot, it's worth proving the **whole loop settles end to end** — open → list/rfq → offer → propose → consolidate → sign → order → deliver → accept → *Vang settled*. The Arena supports a **sim / agent-as-CEO** mode where one process opens a game and signs its own decisions, so you can rehearse without waiting on a human or admin approval.

| **THIS IS NEVER THE PILOT** | Dry-run mode is for testing only. In it, a **separate** signer process holds its *own* Ed25519 key and signs its *own* decisions — it is not a real team's CEO key, and it must never be used to run a live pilot team. Keep the signer **out of `arena_client.py`**: the trading client stays key-free even here. In the real pilot the human signs in their console and the agent only reads `CEO_DECISION` events (Section 7). |
| --- | --- |

**DRY-RUN PROMPT — copy & paste (do this in a scratch folder / test TEAM_ID, not your pilot folder):**
```
Set up a throwaway dry-run so we can prove the full NEO-Arena loop settles end to end in
agent-as-CEO (sim) mode. Read AGENT_GUIDE.md in the neo-arena repo
(https://github.com/hashnet-colab/neo-arena/blob/main/AGENT_GUIDE.md) for the exact /game/open
body and the signing helpers before you start; if I don't have repo access yet, tell me and use
your best reading of the contract in AGENTS.md.

1. Create a SEPARATE file, ceo_sign.py, that (a) mints an Ed25519 keypair for a sim CEO
   (equivalent to the browser's Arena.Crypto.newCeoKey / the Python neo_arena.decisions helper),
   and (b) signs a decision: digest = SHA-256("NEO-ARENA:CEO-DECISION:V1\n" +
   JCS({step_ref, consolidation, decision})); signature_hex = Ed25519_sign(sim_private_key,
   digest).hex(). JCS = RFC 8785 canonical JSON — it MUST be the SAME canonicalization primitive
   the engine/Ledger uses for all hashing, NOT a separate "sorted-keys, no-spaces" rule (a second
   grammar gives byte-different digests and silent signature mismatches). STRONGLY prefer the
   repo's sign_decision helper so you inherit the exact canonicalization instead of reimplementing
   it. This file, and ONLY this file, holds the sim key. arena_client.py must stay key-free.

2. Open ONE sim game with two teams inline:
       POST {BASE}/game/open
       { "teams": [ { "team_id": "alpha", "ceo_public_key": "<hex>", "name": "Alpha" },
                    { "team_id": "beta",  "ceo_public_key": "<hex>", "name": "Beta"  } ],
         "opening": "100.00" }
   (`name` is optional.) One game per server instance — a second /game/open returns 409.

3. Drive the full loop with arena_client.py (reads/market/team functions) plus ceo_sign.py for
   the signature only:
   - seller: list a service; buyer: create an rfq with a small offer_window_positions
   - seller: submit an offer before the window closes
   - buyer team: propose -> consolidate
   - ceo_sign.py signs the consolidation; POST /team/.../decide with { decision, signature_hex }
   - buyer: place_order with the CEO_DECISION entry_hash as ceo_decision_hash
   - seller: submit_delivery (evidence_hash) + submit_invoice
   - buyer: accept_delivery -> confirm the response shows the Vang SETTLED to the seller
   - sync_books after each step and show the four books moving.

4. Show me, at the end: the ORDER_PLACED, VANG_ESCROW_RESERVED, DELIVERY_ACCEPTED and
   VANG_SETTLED journal entries, and both teams' /score before and after. Log any REFUSED_*
   we hit and what fixed it.

Confirm in writing that the sim key lived only in ceo_sign.py and never entered arena_client.py.
```

> Once the loop settles in dry-run, you know the plumbing works. Switch to your **real pilot folder** (with your approved `TEAM_ID`), where the agent never signs and the human holds the key.

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
Scan the market: read state, open RFQs, current listings, and /leaderboard
(and /score). Give me a short list of the best opportunities for my team (what to offer on,
what to buy), and for any counterparty involved, check my Curator second brain for their
record — if we have nothing on them, say so, don't guess.
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
   and a fair price — say clearly where you're guessing vs. where you have a record. If needed,
   create an rfq (with an offer_window_positions) so sellers can respond.
2. Run propose -> consolidate for step [step id] and STOP at the CEO gate. Show me the
   consolidation entry_hash our CEO must sign, and enter WAIT FOR CEO.
3. Wait. After our CEO signs in the team console, watch the feed for the CEO_DECISION event for
   this step, take its entry_hash as ceo_decision_hash, place the order, and update my books.
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
1. From my books, /score, and /leaderboard (read it, never recompute a standings definition),
   summarise: Vang gained, deals won/lost, deliveries accepted vs rejected, disputes, and my
   final rank.
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
Because that's the one thing the Arena exists to prevent. A binding order needs the CEO's Ed25519 signature over the consolidation; the server refuses anything else (`REFUSED_NO_CEO_DECISION` / `REFUSED_BAD_CEO_SIGNATURE`). The agent prepares and waits — the human signs. This is what makes an autonomous trader safe to run.

**How does the signed decision actually reach the agent?**
The CEO signs the consolidation in their **team console** using the Arena's helper (browser `Arena.Crypto.signDecision(...)`, or Python `neo_arena.decisions.sign_decision`), over the domain-separated digest `SHA-256("NEO-ARENA:CEO-DECISION:V1\n" + JCS({step_ref, consolidation, decision}))` — where `JCS` is **RFC 8785 canonical JSON, the same single canonicalization primitive the engine uses for all hashing** (never a separate "sorted-keys" rule; a second grammar causes silent signature mismatches). The console records it via `/decide`, which emits a **`CEO_DECISION`** journal event. The agent, sitting in WAIT FOR CEO, detects that event in the feed and uses its `entry_hash` as `ceo_decision_hash`. The agent never computes the digest, never holds the key, and never calls `/decide`.

**Where does the CEO private key live?**
In the browser team console (the pilot). **Never** in the agent, the project folder, or any file the agent can read. If asked to paste it in, don't. (The only place a key lives *with* a signer is **dry-run mode**, in a separate `ceo_sign.py` that holds its own throwaway sim key — never a real team's, and never inside `arena_client.py`. See Section 8.4.)

**What is Vang, and why strings?**
Vang is the Arena's **synthetic accounting unit — never money, never a token.** It can't be bought, sold, or moved outside the game. Amounts are always **canonical 2-decimal strings** (`"5000.00"`) — never numbers, never floats, never a currency symbol (there is no `currency` field anywhere). The client rejects a numeric amount. This isn't cosmetic: the engine's money type is a 2-decimal string end to end, and strings also keep you clear of the JCS hazard that produces `REFUSED_UNCANONICAL_DECISION` on floats/`Decimal`s.

**How is my rank decided — can I compute it myself?**
Read `GET /leaderboard`; don't reimplement it. It's a *pure function over the journal window* in the engine — the source calls it "the function a losing team re-runs itself." That's the whole point: anyone can re-run the computation over the attested record and get the same winners. If you invented your own definition of *gained / supplier reliability / activity*, your rank would simply disagree with everyone else's. `GET /epochs` gives stamped past epochs.

**My team is stuck "pending" and my books/Vang are empty — what's wrong?**
Almost certainly the **admin approval** step. A team gets its opening Vang only when an admin approves it (`game/team`), and those admin surfaces are disabled until the Arena operator sets `NEO_ARENA_ADMIN_KEYS` on the server. That's on the operator's side, not your setup. Your reads-only connection still works; you just can't trade until you're approved. (Meanwhile you can rehearse the full loop in **dry-run mode**, Section 8.4.)

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

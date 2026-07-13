# Atlas scenarios — ready-to-run prompts

Don't have your own workflow in mind yet? Use these to try Atlas. Each is a **copy-paste prompt** — fill the `[BRACKETS]` and paste it into your agent. They mirror Sections 8–9 of the [guide](../Atlas_Professional_Assistant_Guide.md), organised as **Basic** (works with little or no setup) and **Advanced** (chains modules together). Each notes what it needs.

The `[DOMAIN]` placeholder means one of your Curator domains (e.g. `work` or `personal`). Drop that line if you haven't installed the Curator.

---

## Basic scenarios

### B1 — Capture a task
*Needs: nothing extra.*
```
New task: [what needs doing]. Owner: [me / a name]. Due: [date or "none"].
This came from: [a meeting / an email / just my request].
Log it per AGENTS.md and confirm what's now open.
```

### B2 — Meeting → minutes + action items
*Needs: nothing extra (paste notes) — or the Meeting module for auto-capture.*
```
Here are my raw notes / the transcript from a meeting:
---
[PASTE NOTES OR TRANSCRIPT — or say: "read the latest transcript" if OpenWhispr is on]
---
Produce MEETING MINUTES per AGENTS.md: summary, decisions, action items with owners
and due dates, and open questions. File the action items into tasks.md (and
data/action-items.xlsx if Excel is on). Save the minutes to meetings/.
```

### B3 — Brief me before a call
*Needs: Web Research; better with Curator.*
```
Brief me on [company / person / topic] before my meeting at [time].
Check my Curator second brain first, then the public web. Give me a RESEARCH BRIEF
per AGENTS.md: bottom line up top, key points with sources, what my second brain
already knew, and watch-outs. Keep it to one screen.
```

### B4 — Read a document
*Needs: PDF Reading.*
```
Read the document at documents/[filename].pdf. Give me: a one-paragraph summary,
the 5 things I most need to know, and any dates, numbers, or obligations that
matter. If there's a table worth keeping, extract it into a new sheet in
data/[name].xlsx.
```

### B5 — Weekly digest
*Needs: nothing extra; richer with the modules on.*
```
Produce my WEEKLY DIGEST per AGENTS.md for this week: what got done, meetings and
decisions, what's open or overdue, any research signals, and next week's plan.
Save it to reports/weekly/ and give me the highlights.
```

---

## Advanced scenarios

### A1 — Meeting → action items → follow-up drafts → second brain
*Needs: Meeting module or pasted transcript · Curator · Atomic Mail (optional).*
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

### A2 — Competitive / market monitor (recurring)
*Needs: Web Research · Curator · Excel (optional).*
```
Set up a monitoring routine for [company / market / topic]. Right now:
1. Do a research scan of the public web for anything new in the last [30] days.
2. Cross-check against what my Curator second brain already has, so you only flag
   what's genuinely new.
3. Give me a RESEARCH SCAN per AGENTS.md with a one-line signal I should care about.
4. Log the findings to data/research-log.xlsx and save anything important to my
   [DOMAIN] Curator domain.
Then tell me the cleanest way to re-run this weekly.
```

### A3 — Document → structured data → briefing
*Needs: PDF Reading · Excel · Curator (optional).*
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

### A4 — Inbox triage → daily email report → draft replies
*Needs: Real inbox module (Gmail/Outlook).*
```
Give me my email report for today (read-only): what needs a reply, what's waiting
on me, and what can wait. For the [3] most important, draft replies in my voice —
do NOT send anything; show me the drafts. For anything that's really a task, add it
to tasks.md. Nothing leaves my inbox without my explicit "confirm send".
```

### A5 — Monday morning start-of-week brief
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

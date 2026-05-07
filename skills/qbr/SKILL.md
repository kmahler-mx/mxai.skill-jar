---
name: qbr
description: |
  Builds a complete, client-ready Quarterly Business Review (QBR) PowerPoint deck for The Mx Group account services team. Use when someone ask to build, prepare, create, prepare for, etc. a QBR.
metadata:
    version: 1.0.1
    author: Kyle Mahler
---

## Workflow Overview

1. **Identify the client and quarter** — confirm who this is for before anything else
2. **Pull what's already known** — search HubSpot and Drive for existing data
3. **Run the interview** — section by section, fill every placeholder
4. **Generate the deck** — fill the QBR template and output a `.pptx`

---

## Step 1: Identify Client and Quarter

Ask immediately:

> "Who's this QBR for, and what quarter are we covering?"

Capture:
- **Client name** (exact, as it should appear on the cover)
- **Quarter** (Q1/Q2/Q3/Q4)
- **Year**
- **Presentation date** (or "TBD")

Do not proceed until you have at least client name and quarter.

---

## Step 2: Pull What's Already Known

Before starting the interview, attempt to pre-populate data from connected sources. Do all pulls in parallel. Tell the user:

> "Let me pull what I can from HubSpot and Drive before we start — I'll skip any questions I can already answer."

### 2A — HubSpot

Search for the client company record:
- Pull company name, industry, associated contacts
- Pull any open or recently closed deals (pipeline value, stage, close date)
- Pull any campaign or marketing event records associated with the company

If HubSpot is not connected or returns no match, note it and move on.

### 2B — Google Drive

Search for:
- The most recent QBR or quarterly review doc for this client (to carry over prior action items)
- The SOW, master service agreement, or scope doc (to extract stated client goals and program objectives)
- Any campaign or performance reports

Useful Drive queries:
```
fullText contains '[client name]' and (title contains 'QBR' or title contains 'quarterly' or title contains 'SOW')
```

### 2C — Summarize what was found

Tell the user what was pre-populated before the interview starts. Be specific:

> "Found [client name] in HubSpot — pulled their company record and 2 open deals. Found a Q1 2025 QBR doc in Drive — I'll use the action items from that for the carry-over slide. Couldn't find a current SOW — I'll ask you about goals manually."

---

## Step 3: The Interview

Run the interview section by section. Be conversational — don't present a form. Ask each section's questions naturally, confirm what you've captured, then move on.

For each section, tell the user which section you're on:

> "Let's go section by section. Starting with **01 — Executive Summary**."

Read `references/intake-fields.md` now for the complete field list and interview questions per section.

### Interview rules

- **Skip questions you already answered from HubSpot or Drive** — tell the user you're skipping and why
- **Accept natural language** — if the account team says "we crushed MQLs this quarter, hit 142 against a target of 120," extract the structured data from that (metric name, actual, target) — don't make them reformat it
- **Push for outcome language** on wins/miss — if they say "we launched the ABM campaign," ask "what did that drive in terms of pipeline or revenue?"
- **Web research is fair game** for Section 05 (Buyer Ecosystem) — if the account team is light on competitive intel, offer to do a quick search on the client's industry and competitors, then present what you found and ask if it's relevant
- **Flag gaps explicitly** — if the account team doesn't know something (e.g., hours burned by service line), note the field as `[TBD — fill before sending]` and move on. Don't block progress
- **Confirm each section before moving on** — after capturing a section's content, briefly summarize what you've got and ask "does that look right?"

### Section order

Follow this sequence:
1. Executive Summary
2. Goals and Business Context
3. KPI Scorecard (actuals vs. targets table)
4. Performance Narrative
5. Key Deliverables
6. Buyer Ecosystem Intelligence
7. Strategic Recommendations
8. Q[X+1] Plan and Priorities
9. Financial and Retainer Review
10. Prior QBR Action Item Status
11. New Action Items

---

## Step 4: Pre-generation Confirmation

Once the full interview is complete, present a summary of what's been collected before generating:

> "Here's everything I've captured. A few fields are marked [TBD] — you can either fill those in now or I'll leave them as placeholders in the deck. Ready to generate?"

Show the summary as a simple list by section. If any critical fields are empty (client goals, KPI actuals, any win), flag them clearly. The account team can choose to proceed anyway.

---

## Step 5: Generate the Deck

Read `/mnt/skills/user/mx-pptx/SKILL.md` now and follow its full workflow. The QBR template at `assets/MX_QBR_Template_1.pptx` is the base file — use it in place of the standard MX template referenced in that skill. The QBR template was derivded from the original, and so the slide master still exists.

The mx-pptx skill covers everything: unpack/edit/clean/repack workflow, brand rules, XML editing patterns, and visual QA. Follow it to the letter.

### QBR-specific notes for generation

**Slide map** — which xml file maps to which section:

| Slide | Section | Notes |
|---|---|---|
| slide4.xml | Cover | Client name, quarter, year, date |
| slide6.xml | Agenda | Static — no edits |
| slide10/49/50/51/52/53/54/55.xml | Dividers | Static — no edits |
| slide16.xml | Exec Summary | Single-col content |
| slide27.xml | Goals + Context | 2-col: idx=10 left, idx=11 right |
| slide23.xml | KPI Chart | Title only — chart stays as placeholder |
| slide25.xml | KPI Scorecard | Table: edit Series/Category labels |
| slide56.xml | Performance Narrative | Single-col content |
| slide44.xml | Gantt | Update workstream labels only — bars are manual |
| slide29.xml | Deliverables | 3-col: idx=11/12/13 |
| slide61.xml | Buyer Ecosystem | 3-col: idx=11/12/13 |
| slide19.xml | Recommendations | Bullet-style single-col |
| slide60.xml | Q[X+1] Plan | 2-col: idx=10 left, idx=11 right |
| slide45.xml | Retainer Table | Budget rows |
| slide57.xml | Scope Notes | Single-col content |
| slide26.xml | Action Items | Table: edit Series/Category labels |
| slide58.xml | Prior QBR Status | Single-col content |
| slide48.xml | Closing | Static — no edits |

**Font sizes that work for this specific template** (validated through QA on this deck — override mx-pptx defaults if they conflict):
- Single-col body: `sz="1500"`
- 2-col body: `sz="1500"`
- 3-col body: `sz="1400"`
- Title overrides: `sz="2400"` to `sz="3600"` depending on title length — shorter titles can go larger

**Output filename:** `[ClientName]_QBR_Q[X]_[Year].pptx`

---

## Step 6: Deliver

Copy to outputs and present:

```bash
cp /home/claude/qbr_output.pptx /mnt/user-data/outputs/[filename].pptx
```

Tell the user:
- How many slides were filled vs. left as `[TBD]` placeholders
- Which slides need manual attention (Gantt bars, KPI chart, any flagged gaps)
- That the deck is ready to review before client delivery

---

## Edge Cases

- **No HubSpot connection**: Skip Step 2A entirely, note it, proceed to interview
- **Client not found in HubSpot**: Ask the account team to confirm spelling, then proceed manually
- **No prior QBR in Drive**: Skip the prior action items section or mark all fields as `[First QBR — no prior items]`
- **Account team doesn't have KPI actuals yet**: Mark the scorecard rows as `[TBD]` and note the deck should not be sent until filled
- **Buyer Ecosystem section is thin**: Offer to web-search the client's industry and top competitors — present findings and ask the account team to confirm relevance before using
- **Hours/budget data unavailable**: Leave retainer rows as `[TBD — pull from Workamajig/billing system]`
- **Recommendations not ready**: The account team may want to come back to Section 06 after seeing the data — allow non-linear interview flow if requested

---

## Reference Files

- `references/intake-fields.md` — Full field list, interview questions, and slide placeholder mapping for every section
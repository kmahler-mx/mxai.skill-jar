---
name: market-intelligence
description: >
  Researches a product category and target market, then generates a formatted
  Market & Buyer Ecosystem report as a .docx file ready to open as a Google Doc.
  Use this skill whenever someone asks to "run a market intelligence report",
  "build a buyer ecosystem doc", "research the market for [product]",
  "generate a persona report", "map the buyer journey for [segment]", or
  "create a market analysis for [client]".
metadata:
  version: 1.0.0
  author: Kyle Mahler
---
 
# Market Intelligence Skill
 
Produces a single `.docx` document with six research sections plus an executive
summary:
 
1. **Executive Summary** — top-line takeaways across all sections
2. **Industry Scope** — market size, major players, regulatory context, top challenges
3. **Key Stakeholders** — 3–5 personas who influence or decide the purchase
4. **Motivations & KPIs** — per-persona buying drivers and success metrics
5. **Information Sources** — where these buyers research (watering holes)
6. **Buyer Journey** — awareness → consideration → decision breakdown
7. **Competitor Benchmark** — 3–5 competitors, positioning, strengths, gaps
> **For all .docx generation mechanics** — how to scaffold the script, page
> setup, table rules, hyperlinks, bullets, validation, and the Drive delivery
> note — consult the `docx-to-google-doc` skill. This skill owns the *what*
> (structure, content, sections); `docx-to-google-doc` owns the *how* (rendering).
 
---
 
## Step 1: Gather Inputs
 
Ask the user for the following in a single message before starting:
 
- **Client name** (required — appears in the document title and header)
- **Product / service category** (required — what is being bought/sold)
- **Target market segments** (required — who buys it, e.g. "mid-market dental groups")
- **Target geographies** (required — e.g. "North America", "US only")
- **Additional focus areas** (optional — e.g. "focus on AI adoption trends")
Do not proceed until you have at minimum: client name, product category, and
target market segments.
 
If a brief, RFP, or context document was already uploaded, read it for the above
fields — do not ask again for information already provided.
 
---
 
## Step 2: Research
 
Use web search to populate all six sections. Run targeted searches for each —
do not batch everything into one query. Prioritize recency (last 12–18 months).
 
**Source priority:** industry analyst reports (Gartner, Forrester, IDC) →
trade publications → LinkedIn / professional communities → company websites →
news sources
 
**Rules:**
- Never fabricate statistics — label unknowns as `"Data not available"` or
  prefix estimates with `"Estimate: ~"`
- For persona sections: use job-title-level archetypes only — no real named individuals
- For competitor sections: use real company names with their official website URLs
### Section research guidance
 
**Industry Scope**
- Market size (current + projected), growth rate, CAGR
- 4–6 major competitors / category leaders (names + URLs)
- Top 3–5 regulatory factors or compliance pressures
- Top 3–5 challenges influencing purchase decisions
**Key Stakeholders** (3–5 personas)
Per persona:
- Job title and typical seniority level
- Key responsibilities relevant to the purchase
- Primary pain points
- Role in the decision process: evaluator / influencer / decision-maker / user
**Motivations & KPIs**
Per persona (matching the stakeholder list above):
- Top 2–3 buying motivations
- Key KPIs or success metrics they track (e.g. ROI, time saved, compliance rate)
**Information Sources (Watering Holes)**
Grouped by channel type:
- Trade publications and industry blogs
- LinkedIn groups or communities
- Conferences and events
- Podcasts
- Analyst / research firms
**Buyer Journey**
Three stages:
- **Awareness** — how they learn a problem exists; triggers and content consumed
- **Consideration** — how they evaluate solutions; criteria, comparison methods
- **Decision** — final approval process, procurement steps, typical timeline
**Competitor Benchmark** (3–5 competitors)
Per competitor:
- Company name and website URL
- Key product/service offerings
- Market positioning and primary strengths
- Notable weaknesses or gaps
---
 
## Step 3: Generate the Document
 
Using the `docx-to-google-doc` skill for all rendering mechanics, build the
document to the spec below.
 
---
 
### Global styles
 
| Element | Value |
|---|---|
| Font | Arial throughout |
| Body text size | 22 (11pt equivalent in DXA half-points) |
| All widths | DXA only — never percentage |
| Total content width | 10080 DXA (US Letter, 0.75" margins) |
| Column split (label/value tables) | 2400 DXA label / 7680 DXA value |
 
### Color palette
 
| Element | Hex |
|---|---|
| Title text | `1A3A5C` |
| Section header background | `1A3A5C` |
| Section header text | `FFFFFF` |
| Label cell background | `2C5282` |
| Label cell text | `FFFFFF` |
| Alternate row fill (even rows) | `EBF4FF` |
| Persona title row background | `2C5282` |
| Competitor title row background | `1A3A5C` |
| Accent / divider color | `3182CE` |
| Body text | `1A202C` |
| Hyperlinks | `1155CC` |
| AI-generated / subtitle text | `718096` |
 
---
 
### Document header (above all tables)
 
```
{Client Name} | Market & Buyer Ecosystem     ← bold, size 52, color 1A3A5C
{Product Category} — {Target Market Segments} ← size 24, color 718096
AI Generated · {YYYY-MM-DD}                  ← size 20, color 718096
```
 
Add a thin horizontal rule paragraph (top border only, color 3182CE) after the
header block to visually separate it from the first section.
 
---
 
### Section 1: Executive Summary
 
Full-width section header row (columnSpan 2, bg `1A3A5C`):
- Line 1: `"Executive Summary"` — size 26, bold, white
- Line 2: `"Top-line findings across all six research areas"` — size 20, color `AAAAAA`
Then 5–7 bullet-point rows in a single-column full-width table (no label column
for this section). Each bullet is one crisp finding pulled across all six
sections. Use `EBF4FF` alternating row fill.
 
---
 
### Sections 2–6: Research Sections
 
Each section follows the same structure:
 
**Section header row** — full-width (columnSpan 2), bg `1A3A5C`:
- Line 1: section title (e.g. `"Industry Scope"`) — size 26, bold, white
- Line 2: one-line description of what the section covers — size 20, color `AAAAAA`
Section titles and subtitles:
 
| Section | Title | Subtitle |
|---|---|---|
| 2 | Industry Scope | Market size, key players, regulatory factors, and purchase challenges |
| 3 | Key Stakeholders | The 3–5 roles who influence or decide this purchase |
| 4 | Motivations & KPIs | What drives each stakeholder and how they measure success |
| 5 | Information Sources | Where these buyers research, learn, and consume content |
| 6 | Buyer Journey | How buyers move from awareness through to purchase decision |
| 7 | Competitor Benchmark | Key competitors, their positioning, strengths, and gaps |
 
---
 
### Section 2 layout — Industry Scope
 
Two-column label/value table (2400 / 7680 split):
 
| Row label | Content | Format |
|---|---|---|
| Market Overview | Size, projected growth, CAGR | Plain text |
| Key Players | Competitor names + URLs | Linked bullets |
| Regulatory Factors | Top 3–5 pressures | Bullet list |
| Purchase Challenges | Top 3–5 challenges | Bullet list |
 
Alternate row fill on even rows (`EBF4FF`).
 
---
 
### Section 3 layout — Key Stakeholders
 
One **persona card** per stakeholder. Each card is a self-contained 5-row table
at full width (10080 DXA), 2400 / 7680 split:
 
```
Row 1 — Persona title row
  Full-width cell (columnSpan 2), bg 2C5282
  Text: "{Job Title}"  ← size 24, bold, white
 
Row 2 — Seniority & Responsibilities
  Label cell (bg 2C5282): "Role & Responsibilities"
  Value cell: plain text
 
Row 3 — Pain Points
  Label cell: "Primary Pain Points"
  Value cell (bg EBF4FF): bullet list
 
Row 4 — Decision Role
  Label cell: "Decision Role"
  Value cell: plain text (e.g. "Decision-Maker — final budget approval")
 
```
 
Add a spacer paragraph (10pt, no border) between persona cards.
 
---
 
### Section 4 layout — Motivations & KPIs
 
One table covering all personas. Three columns: Persona | Top Motivations | Key KPIs.
 
Column widths: 2000 / 4080 / 4000 (sums to 10080 DXA).
 
Header row: bg `1A3A5C`, white bold text for each column label.
 
One row per persona. Alternate row fill (`EBF4FF`) on even rows.
Motivations and KPIs rendered as bullet lists within their cells.
 
---
 
### Section 5 layout — Information Sources
 
Two-column label/value table (2400 / 7680 split).
 
| Row label | Content | Format |
|---|---|---|
| Trade Publications | Names and URLs | Linked bullets |
| LinkedIn / Communities | Group names or community types | Bullet list |
| Conferences & Events | Top 3–5 by name | Bullet list |
| Podcasts | Top podcasts by name | Bullet list |
| Analyst / Research Firms | Firm names and URLs | Linked bullets |
 
Alternate row fill on even rows.
 
---
 
### Section 6 layout — Buyer Journey
 
Three-column table: Stage | What Happens | Implications for Marketing.
 
Column widths: 1800 / 4200 / 4080 (sums to 10080 DXA).
 
Header row: bg `1A3A5C`, white bold.
 
Rows: Awareness, Consideration, Decision.
Each cell uses bullet lists for the content. Alternate row fill on even rows.
 
---
 
### Section 7 layout — Competitor Benchmark
 
One **competitor card** per competitor. Each card is a 5-row table at full
width (10080 DXA), 2400 / 7680 split:
 
```
Row 1 — Competitor title row
  Full-width cell (columnSpan 2), bg 1A3A5C
  Text: "{Company Name}"  ← size 24, bold, white; include URL as hyperlink
 
Row 2 — Offerings
  Label cell (bg 2C5282): "Key Offerings"
  Value cell: bullet list
 
Row 3 — Positioning & Strengths
  Label cell: "Positioning & Strengths"
  Value cell (bg EBF4FF): bullet list
 
Row 4 — Weaknesses / Gaps
  Label cell: "Weaknesses / Gaps"
  Value cell: bullet list
```
 
Add a spacer paragraph between competitor cards.
 
---
 
### Section spacing
 
Add a **spacer paragraph** (12pt, no border) between each major section (after
each section's last table, before the next section header row). Do not add page
breaks between sections — let content flow naturally.
 
---
 
### Output filename
 
`{Client Name} Market Intelligence.docx`
 
### Notes
- Title uses spaces between words, not underscores.
- Body text inside table cells must be size 22 — do not accidentally set it to 20.
- Always alternate row fill using `EBF4FF`, not grey tones.
- Persona cards and competitor cards are separate tables, not rows inside a
  larger table — this ensures clean spacing and independent borders.
---
name: company-background
description: >
  Researches a company and generates a formatted Executive Summary company
  background document as a .docx file ready to open as a Google Doc. Use this
  skill whenever someone asks to "run a company background", "pull a background
  on [company]", "research a prospect", "build a prospect profile", or "create
  a company overview doc".
metadata:
  version: 1.0.1
  author: Kyle Mahler
---

# Company Background Skill

Produces a single `.docx` document with two sections:
1. **Executive Summary** — structured fact table about the company
2. **Use Case Stories** — 3 buyer-anchored stories showing how the company's
   products solve real problems

> **For all .docx generation mechanics** — how to scaffold the script, page
> setup, table rules, hyperlinks, bullets, validation, and the Drive delivery
> note — consult the `docx-to-google-doc` skill. This skill owns the *what*
> (structure, content, fields); `docx-to-google-doc` owns the *how* (rendering).

---

## Step 1: Gather Inputs

Ask the user for the following in a single message before starting:

- **Company name** (required)
- **Company website URL** (required — used to anchor research)
- **Their ask / RFP context** (optional — paste a brief or upload a doc)
- **Additional focus areas** (optional — e.g., "focus on their AI investments")

Do not proceed until you have at minimum: company name and website URL.

If a file or document was already uploaded to the conversation, read it directly
for the "their ask" field — do not ask again.

---

## Step 2: Research

Use web search to populate all fields for both sections. Run multiple targeted
searches — do not rely on a single query.

**Source priority:** official company site → SEC/10-K filings → LinkedIn →
press releases → analyst reports

**Rules:**
- Never fabricate specific numbers — label unknowns as `"Data not available"`
  or prefix estimates with `"Estimate: ~"`
- For array fields (products, competitors): gather each as a separate item
  with its corresponding URL

### Section 1 fields — Executive Summary

| Field | What to find |
|---|---|
| `high_level_overview` | 1–2 sentence elevator pitch — what they do and who they serve |
| `products` | Core products or services (general product name first followed by Branded name, e.g. if the company was Apple... Smartphones (iPhone), Laptops (Mac), etc...) |
| `product_urls` | Official product page URL for each, in same order as `products` |
| `client_industry` | Primary industry classification (e.g. "Technology / SaaS") |
| `markets_served` | Markets or customer segments they sell into |
| `revenue_and_employees` | Revenue first, headcount second. Fiscal year if public; estimate if private |
| `geography` | Where they primarily sell — if worldwide, just "Global" |
| `competitors` | 4–7 primary competitors (names only) |
| `competitor_urls` | Official website for each, in same order as `competitors` |
| `their_ask` | What marketing services they need. Only populate if RFP/brief provided — otherwise `"Data not available"` |
| `website_url` | Their main website URL |
| `todays_date` | Today's date in YYYY-MM-DD format |

### Section 2 fields — Use Case Stories

Research and write **3 distinct use case stories**. Each story should target a
different buyer role, company type, or challenge. Vary across:
- Decision-maker type (e.g. COO vs. owner-dentist vs. compliance manager)
- Customer size or segment (e.g. enterprise vs. SMB vs. multi-location group)
- Primary pain (efficiency, risk/compliance, growth, cost reduction)

For each story, populate these fields:

| Field | What to find/write |
|---|---|
| `story_title` | Short, benefit-focused title (e.g. "First-Time-Right Crowns") |
| `customer_type` | Archetypal customer description — industry, size, decision-maker role and focus |
| `business_problem` | 2–3 sentences describing the specific operational or business pain |
| `applicable_products` | Which of the company's specific products/services address this need |
| `expected_outcome` | 3–4 bullet points with tangible results (efficiency, cost, risk, experience) |

**Story writing rules:**
- Use archetypal customers only — never real companies or named individuals
- Define all industry jargon and acronyms on first use
- Keep each story tight and scannable — no padding
- Anchor outcomes to specific, believable improvements
- Base stories on researched information about the company's actual product line

---

## Step 3: Generate the Document

Using the `docx-to-google-doc` skill for all rendering mechanics, build the
document to the spec below. The document has two visually connected sections
that share the same color palette and table style.

---

### Global styles

| Element | Value |
|---|---|
| Font | Arial throughout |
| Body text size | 22 (11pt) |
| All widths | DXA only — never percentage |
| Total content width | 10080 DXA (US Letter, 0.75" margins) |
| Column split (label/value tables) | 2200 DXA label / 7880 DXA value |

### Color palette

| Element | Hex |
|---|---|
| Title text / section header bg / label bg | `1F2D3D` |
| Label / header text | `FFFFFF` |
| Alternate row fill (even rows) | `F0F4F8` |
| Story title row bg | `2E4A6B` |
| Story title text | `FFFFFF` |
| Body text | `000000` |
| Hyperlinks | `1155CC` |

---

### Layout — Section 1: Executive Summary

```
[Title block — above the table, not inside it]
  {Company Name} | Company Background    ← bold, size 44, color 1F2D3D
  AI Generated                           ← size 20, color 555555
  YYYY-MM-DD                             ← size 20, color 555555

[Two-column table — label / value rows]
  Column split: 2200 / 7880
```

| Row label | Field | Format |
|---|---|---|
| High Level Overview | `high_level_overview` | Plain text |
| Product | `products` + `product_urls` | Linked bullets |
| Client Industry | `client_industry` | Plain text |
| Markets Served | `markets_served` | Plain bullets |
| Size + Financials | `revenue_and_employees` | Plain bullets |
| Geography | `geography` | Plain text |
| Competitors | `competitors` + `competitor_urls` | Linked bullets |
| Ask | `their_ask` | Plain text |
| Website | `website_url` | Single hyperlink |

Alternate row fill: even rows get `F0F4F8`, odd rows white.

---

### Layout — Section 2: Use Case Stories

Add a **page break** paragraph between Section 1 and Section 2.

```
[Use Case Stories header row]
  Full-width cell spanning both columns, bg 1F2D3D
  Line 1: white bold text "Use Case Stories" — size 24
  Line 2: color AAAAAA size 18 — "Buyer-anchored scenarios illustrating how {Company Name} creates value"
```

Then render each of the 3 stories as its own **story card** — a self-contained
5-row table at full width (10080 DXA), same column split as Section 1:

```
Row 1 — Story title row
  Full-width cell (columnSpan 2), bg 2E4A6B
  Text: "{N}. {story_title}"  ← size 24, bold, white

Row 2 — Typical Customer
  Label cell (2200, bg 1F2D3D, white bold): "Typical Customer"
  Value cell (7880, white bg): plain text

Row 3 — Business Problem
  Label cell: "Business Problem"
  Value cell (bg F0F4F8): plain text

Row 4 — Applicable Products
  Label cell: "Applicable Products"
  Value cell (white bg): bullet list — one bullet per product

Row 5 — Expected Outcome
  Label cell: "Expected Outcome"
  Value cell (bg F0F4F8): bullet list — one bullet per outcome point
```

Add a **spacer paragraph** (12pt, no border) between story cards. Do not add a
page break between stories — keep all 3 on the same page where possible.

---

### Output filename
`{Company Name} Company Background.docx` 

### Notes
This skill has a habit of titling incorrectly. Title only with spaces in between title words rather than underscores.
This skill also hasa habit of making the size of the font 20 instead of 22 inside of tables. Do not mess that up.

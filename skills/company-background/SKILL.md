---
name: company-background
description: >
  Researches a company and generates a formatted Executive Summary company
  background document as a .docx file ready to open as a Google Doc. Use this
  skill whenever someone asks to "run a company background", "pull a background
  on [company]", "research a prospect", "build a prospect profile", or "create
  a company overview doc". 
---

# Company Background Skill

Produces a formatted Executive Summary company background document by:
1. Gathering the company name and website from the user
2. Conducting web research across all required fields
3. Generating a `.docx` using the structure defined below
4. Presenting it for download

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

Use web search to populate each field. Run multiple targeted searches — do not
rely on a single query.

**Source priority:** official company site → SEC/10-K filings → LinkedIn →
press releases → analyst reports

**Rules:**
- Never fabricate specific numbers — label unknowns as `"Data not available"`
  or prefix estimates with `"Estimate: ~"`
- For array fields (products, competitors, markets): gather each as a separate
  item with its corresponding URL

### Fields to research

| Field | What to find |
|---|---|
| `high_level_overview` | 1–2 sentence elevator pitch — what they do and who they serve |
| `products` | Core products or services (names only) |
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

---

## Step 3: Generate the Document

Using the `docx-to-google-doc` skill for all rendering mechanics, build the
document to the following spec.

### Layout

```
[Title block]
  {Company Name} | Company Background    ← bold, size 44, color 1F2D3D
  AI Generated                           ← size 20, color 555555
  YYYY-MM-DD                             ← size 20, color 555555

[Executive Summary — full-width header row]
  Dark navy background (1F2D3D), white bold text, spans both columns

[Executive Summary — two-column table]
  Column split: 2200 DXA (label) / 7880 DXA (value)
  Total width: 10080 DXA (US Letter, 0.75" margins)
```

### Row definitions

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

### Color palette

| Element | Hex |
|---|---|
| Title text / label background | `1F2D3D` |
| Label text | `FFFFFF` |
| Alternate row fill (rows 2, 4, 6, 8) | `F0F4F8` |
| Body text | `000000` |
| Hyperlinks | `1155CC` |

### Hyperlinked fields

- **Product** — each bullet links to its `product_urls` entry
- **Competitors** — each bullet links to its `competitor_urls` entry
- **Website** — the URL itself is the clickable link

All hyperlinks: color `1155CC`, underline, font Arial, size 20.

### Output filename
`{Company_Name}_Company_Background.docx` (spaces replaced with underscores)

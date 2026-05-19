---
name: stakeholder-bios
description: >
  Researches individuals at a prospect or client company and generates a
  formatted team profile document as a .docx file ready to open as a Google
  Doc. Use this skill whenever someone asks to "research the team at [company]",
  "build stakeholder profiles", "pull bios on [names]", "create team profiles".
metadata:
  version: 1.0.0
  author: Kyle Mahler
---

# Stakeholder Bios Skill

Produces a formatted team profile document — one card per person — by:
1. Gathering inputs (names, LinkedIn URLs, or LinkedIn PDF exports)
2. Researching each person
3. Generating a `.docx` with one profile card per person
4. Presenting it for download

> **For all .docx generation mechanics** — script scaffolding, page setup,
> table rules, hyperlinks, bullets, image embedding, validation, and the Drive
> delivery note — consult the `docx-to-google-doc` skill. This skill owns the
> *what* (structure, content, fields); `docx-to-google-doc` owns the *how*
> (rendering).

---

## Step 1: Gather Inputs

Confirm with the user that what they want to run the Stakeholder Bios skill before going through the full pipeline.

Accept any of the following — use whatever the user provides:

- **Names + titles** (minimum viable input)
- **LinkedIn profile URLs** (preferred — improves research accuracy significantly)
- **Downloaded LinkedIn PDFs** (best — extract all fields directly from the PDF before doing any web research)
- **Company name** (required)
- **Additional context** (optional)

If LinkedIn PDFs are uploaded, extract all fields directly from them first.
Only fall back to web research for fields not found in the PDFs.
Do not ask for information already present in the conversation or uploads.

---

## Step 2: Research

For each person, populate the fields below. Use LinkedIn URL or PDF as primary
source, supplemented by company website, press releases, and web search.

**Source priority:** LinkedIn PDF → LinkedIn profile → company About/Team page
→ press releases → news mentions

**Rules:**
- Never fabricate — use `"Unknown"` for missing fields
- `headshot_url`: direct image URL (PNG/JPG, no auth required) from company
  team page or press kit only.
  Set to `null` if not found — a missing headshot is fine.
- `company_bio`: official bio from the company's website About/Team page
- `general_observations`: 2–4 concise bullet strings with sales/marketing
  intelligence — career trajectory, recent moves, talking points, hooks

### Fields per person

| Field | What to find |
|---|---|
| `name` | Full name |
| `title` | Current job title |
| `linkedin_url` | Full LinkedIn profile URL |
| `headshot_url` | Direct image URL or `null` |
| `company_bio` | Official company website bio, or `"Not available"` |
| `general_observations` | Array of 2–4 bullet strings — sales intelligence |
| `based_in` | City, State/Country |
| `current_role` | Title + 1–2 sentences on responsibilities |
| `previous_roles_this_company` | Previous positions at this company (array, max 3, or `["None listed"]`) |
| `previous_roles_other` | Prior career — "Title at Company (Years)" (array, max 4) |
| `education` | "Degree, Institution, Year" or `"Unknown"` |
| `personal_interests` | Hobbies, causes, speaking topics, or `"Unknown"` |

---

## Step 3: Generate the Document

Using the `docx-to-google-doc` skill for all rendering mechanics, build the
document to the following spec.

### Page setup
- US Letter: 12240 × 15840 DXA, margins 1080 all sides
- Content width: 10080 DXA

### Document layout

```
[Title block]
  {Company Name} | Stakeholder Bios     ← bold, size 44, color 1F2D3D
  AI Generated                          ← size 20, color 555555
  YYYY-MM-DD                            ← size 20, color 555555

[Blank spacing paragraph]

[Person card × N]
[Blank spacing paragraph between cards]
```

---

### Font sizes
- All body text: size **22** (11pt in half-points)
- Label cell text: size **22** bold
- Name header: size **24** bold

### Per-person card — ONE table, multiple rows

Each card is a single table with column widths **2200 + 7880 = 10080 DXA**.
Every row has exactly two cells EXCEPT Row 1 which uses `columnSpan: 2` for a
true full-width merged header. This is the one safe place to use columnSpan —
a single-row header-only merge renders reliably in Google Docs. Do not use
columnSpan or rowSpan anywhere else in the table.

#### Row 1 — Name header (true merged, full width)

Single cell with `columnSpan: 2`, full 10080 DXA width.

```javascript
new TableRow({
  children: [
    new TableCell({
      columnSpan: 2,
      width: { size: 10080, type: WidthType.DXA },
      borders: thinBorders,
      shading: { fill: "A0A0A0", type: ShadingType.CLEAR },
      margins: { top: 120, bottom: 120, left: 200, right: 200 },
      children: [new Paragraph({
        children: [new ExternalHyperlink({
          link: person.linkedin_url,
          children: [new TextRun({
            text: `${person.name} — ${person.title}`,
            size: 24, bold: true, font: "Arial", color: "FFFFFF", underline: {}
          })]
        })]
      })]
    })
  ]
})
```

**If no LinkedIn URL available:** render as plain bold white TextRun, same
styling, no link.

---

#### Row 2 — Headshot + Bio/Observations

Left cell: headshot image if available, else gray placeholder with "[Photo]" text.
Right cell: company bio bullet(s) first, then general observations bullets below.

```javascript
new TableRow({
  children: [
    // Left: headshot or placeholder
    new TableCell({
      width: { size: 2200, type: WidthType.DXA },
      borders: thinBorders,
      shading: { fill: "EEEEEE", type: ShadingType.CLEAR },
      margins: { top: 80, bottom: 80, left: 80, right: 80 },
      verticalAlign: VerticalAlign.TOP,
      children: headshot
        ? [new Paragraph({ children: [new ImageRun({
            data: headshotBuf,
            transformation: { width: 120, height: 140 },
            type: headshotType
          })]})]
        : [new Paragraph({
            alignment: AlignmentType.CENTER,
            children: [new TextRun({ text: "[Photo]", size: 18, color: "999999", font: "Arial" })]
          })]
    }),
    // Right: company bio then observations — all as bullets
    new TableCell({
      width: { size: 7880, type: WidthType.DXA },
      borders: thinBorders,
      margins: { top: 120, bottom: 120, left: 200, right: 200 },
      verticalAlign: VerticalAlign.TOP,
      children: [
        // Company bio — render as a bullet item
        new Paragraph({
          numbering: { reference: "bullets", level: 0 },
          children: [new TextRun({ text: person.company_bio, size: 22, font: "Arial" })]
        }),
        // General observations — one bullet per item
        ...person.general_observations.map(o =>
          new Paragraph({
            numbering: { reference: "bullets", level: 0 },
            children: [new TextRun({ text: o, size: 22, font: "Arial" })]
          })
        )
      ]
    })
  ]
})
```

---

#### Rows 3–7 — Label / value detail rows

Same two-column pattern. Left cell: dark navy label. Right cell: value content.
Alternating fill on rows 4 and 6 (`F0F4F8`).

| Row | Left label | Right value | Format |
|---|---|---|---|
| 3 | Based In | `based_in` | Plain text |
| 4 | Current Role | `current_role` | Plain text |
| 5 | Previous {Company} Roles | `previous_roles_this_company` | Bullet list |
| 6 | Previous Jobs | `previous_roles_other` | Bullet list |
| 7 | Education | `education` | Plain text |
| 8 | Personal Interests/Other | `personal_interests` | Plain text |

Label cells: navy `1F2D3D` background, white bold Arial size 20.
Value cells: plain text or bullet list, Arial size 20, black.
Alternating fill: rows 4 and 6 get `F0F4F8`.

```javascript
const labelCell = (text) => new TableCell({
  width: { size: 2200, type: WidthType.DXA },
  borders: thinBorders,
  shading: { fill: "1F2D3D", type: ShadingType.CLEAR },
  margins: { top: 80, bottom: 80, left: 160, right: 160 },
  verticalAlign: VerticalAlign.TOP,
  children: [new Paragraph({
    children: [new TextRun({ text, size: 22, bold: true, color: "FFFFFF", font: "Arial" })]
  })]
});

const plainValueCell = (text, bg) => new TableCell({
  width: { size: 7880, type: WidthType.DXA },
  borders: thinBorders,
  shading: bg ? { fill: bg, type: ShadingType.CLEAR } : undefined,
  margins: { top: 80, bottom: 80, left: 160, right: 160 },
  verticalAlign: VerticalAlign.TOP,
  children: [new Paragraph({
    children: [new TextRun({ text, size: 22, font: "Arial" })]
  })]
});

const bulletValueCell = (items, bg) => new TableCell({
  width: { size: 7880, type: WidthType.DXA },
  borders: thinBorders,
  shading: bg ? { fill: bg, type: ShadingType.CLEAR } : undefined,
  margins: { top: 80, bottom: 80, left: 160, right: 160 },
  verticalAlign: VerticalAlign.TOP,
  children: items.map(item => new Paragraph({
    numbering: { reference: "bullets", level: 0 },
    children: [new TextRun({ text: item, size: 22, font: "Arial" })]
  }))
});

// The rows:
new TableRow({ children: [labelCell("Based In"),                                    plainValueCell(person.based_in)] }),
new TableRow({ children: [labelCell("Current Role"),                                plainValueCell(person.current_role, "F0F4F8")] }),
new TableRow({ children: [labelCell(`Previous ${companyName} Roles`),              bulletValueCell(person.previous_roles_this_company)] }),
new TableRow({ children: [labelCell("Previous Jobs"),                               bulletValueCell(person.previous_roles_other, "F0F4F8")] }),
new TableRow({ children: [labelCell("Education"),                                   plainValueCell(person.education)] }),
new TableRow({ children: [labelCell("Personal Interests/Other"),                    plainValueCell(person.personal_interests, "F0F4F8")] }),
```

---

### Color palette

| Element | Hex |
|---|---|
| Name header background | `A0A0A0` |
| Name header text | `FFFFFF` |
| Photo placeholder cell | `EEEEEE` |
| Label cell background | `1F2D3D` |
| Label text | `FFFFFF` |
| Alternate row fill | `F0F4F8` |
| Body text | `000000` |
| Hyperlinks | `1155CC` |

### Page break between cards
Each person gets their own page. After each card (except the last), insert a
page break paragraph before the next card:

```javascript
new Paragraph({
  children: [new TextRun({ break: 1 })]  // PageBreak
})
```

Use `PageBreak` from docx-js:
```javascript
import { PageBreak } from 'docx';
// or
new Paragraph({ children: [new PageBreak()] })
```

### Output filename
`{Company_Name} Stakeholder Bios.docx`

Spaces in between each word in the title, not underscores. Capitalize correctly.

---

## Notes

Best sources: company team/about page → press releases → conference bios → Twitter/X.
If not found: `null` → renders `[Photo]` placeholder — acceptable and clean.

This skill has had a continous problem of hallucinating the stakeholder LinkedIn URL's when creating hyperlinks. Be extra attentive when setting them. Double check each URL to make sure it matches that which was provided initially.

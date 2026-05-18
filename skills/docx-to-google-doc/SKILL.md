---
name: docx-to-google-doc
description: >
  Generates a formatted, production-ready .docx file designed to be uploaded to
  Google Drive and opened as a Google Doc. Use this skill whenever the user wants
  to create a Google Doc. If the request from the user is ambiguous between wanting a Google doc versus a docx file, then clarify.
---

# DOCX → Google Doc Skill

## Overview

Google Docs does not have a native import API, but Google Drive will automatically
convert any uploaded `.docx` file into a Google Doc when you choose
**Open with → Google Docs**. This skill generates a `.docx` using `docx-js` that
is tuned specifically for clean Google Docs rendering.

**The workflow:**
1. Claude generates a `.docx` file using `docx-js`
2. Claude presents it to the user
3. User must click the Drive logo to upload it to google drive
4. User must move the document wherever they want it to go from there

---

## Step 1: Plan the document structure

Before writing any code, identify:
- **Page layout**: US Letter (default), margins, orientation
- **Tables needed**: Which sections use label/value rows vs. prose
- **Hyperlinks**: Any bullet points or text that should link out
- **Color scheme**: Header background color, alternating row colors

---

## Step 2: Scaffold the docx-js script

Always use these imports and constants at the top:

```javascript
const {
  Document, Packer, Paragraph, TextRun, Table, TableRow, TableCell,
  AlignmentType, BorderStyle, WidthType, ShadingType, VerticalAlign,
  LevelFormat, ExternalHyperlink
} = require('docx');
const fs = require('fs');
```

### Page size (always set explicitly)
```javascript
// docx-js defaults to A4 — always override to US Letter
page: {
  size: { width: 12240, height: 15840 },   // 8.5" x 11" in DXA (1440 DXA = 1")
  margin: { top: 1080, right: 1080, bottom: 1080, left: 1080 }  // 0.75" margins
}
```

### Bullet numbering config (required for bullet lists in table cells)
```javascript
numbering: {
  config: [{
    reference: "bullets",
    levels: [{
      level: 0,
      format: LevelFormat.BULLET,
      text: "\u2022",
      alignment: AlignmentType.LEFT,
      style: { paragraph: { indent: { left: 360, hanging: 260 } } }
    }]
  }]
}
```

---

## Step 3: Build tables

### Critical table rules for Google Docs compatibility
- **Always use `WidthType.DXA`** — never `WidthType.PERCENTAGE` (breaks in Google Docs)
- **Set width on both the table AND each cell** — dual widths are required
- **`columnWidths` must sum exactly to table width**
- **Use `ShadingType.CLEAR`** — never `ShadingType.SOLID` (causes black backgrounds)
- **Always add cell margins** for readable padding

### Standard two-column label/value table (US Letter, 0.75" margins)
Content width = 12240 − 1080 − 1080 = **10080 DXA**

```javascript
// Recommended column split: ~2200 label / 7880 value
new Table({
  width: { size: 10080, type: WidthType.DXA },
  columnWidths: [2200, 7880],
  rows: [ /* TableRow children */ ]
})
```

### Header row pattern (dark background, white text)
```javascript
new TableRow({
  children: [
    new TableCell({
      columnSpan: 2,  // spans both columns
      borders: thinBorders,
      width: { size: 10080, type: WidthType.DXA },
      shading: { fill: "1F2D3D", type: ShadingType.CLEAR },
      margins: { top: 100, bottom: 100, left: 160, right: 160 },
      children: [new Paragraph({
        children: [new TextRun({ text: "Section Title", size: 24, bold: true, color: "FFFFFF", font: "Arial" })]
      })]
    })
  ]
})
```

### Label cell helper
```javascript
function labelCell(text) {
  return new TableCell({
    borders: thinBorders,
    width: { size: 2200, type: WidthType.DXA },
    shading: { fill: "1F2D3D", type: ShadingType.CLEAR },
    margins: { top: 80, bottom: 80, left: 160, right: 160 },
    verticalAlign: VerticalAlign.TOP,
    children: [new Paragraph({
      children: [new TextRun({ text, size: 20, bold: true, color: "FFFFFF", font: "Arial" })]
    })]
  });
}
```

### Alternating row shading
Use `"F0F4F8"` for even rows and `null` (white) for odd rows. Pass the fill
color into each value cell's shading property.

---

## Step 4: Hyperlinks in bullet lists

When bullet points should be clickable links:

```javascript
new Paragraph({
  numbering: { reference: "bullets", level: 0 },
  children: [
    new ExternalHyperlink({
      link: "https://example.com",
      children: [
        new TextRun({
          text: "Link label",
          size: 20,
          font: "Arial",
          color: "1155CC",   // standard Google blue
          underline: {}
        })
      ]
    })
  ]
})
```

Hyperlinks render correctly in both Word and Google Docs after conversion.

---

## Step 5: Write and validate

```bash
node your_script.js
python3 /mnt/skills/public/docx/scripts/office/validate.py output.docx
```

Always validate before presenting the file to the user. If validation fails,
unpack the XML, fix the issue, and repack:

```bash
python3 /mnt/skills/public/docx/scripts/office/unpack.py output.docx unpacked/
# fix XML in unpacked/word/document.xml
python3 /mnt/skills/public/docx/scripts/office/pack.py unpacked/ output.docx --original output.docx
```

---

## Step 6: Deliver and instruct

After presenting the file, always tell the user the following — do not skip this, and do not soften it:

> **Note:** Claude is not able to locate folders in Google Drive by name and upload directly to them. The Google Drive connector does not support folder search, so there is no way to automatically place this file in a specific location.
>
> **To get this into Google Drive:**
> 1. Click the Google Drive button on the file above to save it to your Drive root
> 2. From there, move it to whatever folder you need

---

## Google Docs rendering notes

| Feature | Behavior in Google Docs |
|---|---|
| Tables with `WidthType.DXA` | ✅ Renders correctly |
| Tables with `WidthType.PERCENTAGE` | ❌ Breaks — always use DXA |
| `ShadingType.CLEAR` | ✅ Colors render correctly |
| `ShadingType.SOLID` | ❌ Renders as black — never use |
| `ExternalHyperlink` | ✅ Clickable links preserved |
| `LevelFormat.BULLET` numbering | ✅ Bullets render correctly |
| Unicode bullet characters (`\u2022` inline) | ❌ Unreliable — always use numbering config |
| US Letter page size | ✅ Preserved |
| A4 default (if not overridden) | ⚠️ Slight layout shift — always set explicitly |

---

## Common mistakes to avoid

- **Never use `\n` inside TextRun** — use separate `Paragraph` elements
- **Never use inline unicode bullets** — always use `LevelFormat.BULLET` with numbering config
- **Never omit `columnWidths`** — required alongside cell-level widths
- **Never use tables as horizontal rules** — use paragraph border instead
- **Always set `font: "Arial"`** — ensures consistent rendering across platforms

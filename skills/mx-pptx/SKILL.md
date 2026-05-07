---
name: mx-pptx
description: >
  Create and edit PowerPoint presentations using The Mx Group's official branded
  template (mx_powerpoint_template_v3.pptx). Use this skill whenever someone
  asks to build a slide deck, pitch, proposal, presentation, or any .pptx file
  for MX agency use — internal or client-facing. Always start from the MX
  template, never from scratch with pptxgenjs.
metadata:
    version: 1.0.1
    author: Kyle Mahler
---

# MX PowerPoint Skill

## Quick Reference

| Task | Approach |
|------|----------|
| New deck from scratch | Copy template → unpack → edit slides → repack |
| Edit existing deck | Unpack → edit slides → repack |
| Identify which slide layout to use | See [Slide Layout Catalog](#slide-layout-catalog) below |
| Visual QA | See [QA](#qa) section |

---

## Critical: Always Use the Template

**Never use pptxgenjs to create MX presentations from scratch.** The template
contains brand-correct layouts, fonts, colors, the gradient bar image, the MX
logo, and slide masters that cannot be reproduced programmatically without
significant effort. Always start from the template file at:

```
/mnt/skills/user/mx-pptx/mx_powerpoint_template_v3.pptx
```

Workflow:

```bash
# 1. Copy the template to your working directory
cp /mnt/skills/user/mx-pptx/references/mx_powerpoint_template_v3.pptx /home/claude/output.pptx

# 2. Unpack it
python /mnt/skills/public/pptx/scripts/office/unpack.py /home/claude/output.pptx /home/claude/unpacked

# 3. Edit slides (see editing.md in the public pptx skill)
# Delete slides you don't need, duplicate slides you do, edit content

# 4. Clean up orphaned files
python /mnt/skills/public/pptx/scripts/clean.py /home/claude/unpacked

# 5. Repack
python /mnt/skills/public/pptx/scripts/office/pack.py /home/claude/unpacked /home/claude/output.pptx --original /mnt/skills/user/mx-pptx/refernces/mx_powerpoint_template_v3.pptx

# 6. QA (see below)
```

For editing details (how to manipulate slides, XML editing rules, common
pitfalls), refer to the public pptx skill's editing.md:
`/mnt/skills/public/pptx/editing.md`

---

## Brand Rules (The Big 4)

These are non-negotiable. They live on slide 1 of the template and apply to
every presentation.

1. **All type is aligned left** — never center body text
2. **Vertically center the content on each slide**
3. **No ampersands** — always write "and"
4. **Only use the gradient bar at the bottom if there is no color elsewhere on the slide** — don't double-up the bar with a colored background

---

## Brand Identity

### Colors

| Role | Hex | Usage |
|------|-----|-------|
| **Primary Black** | `000000` | Dark slide backgrounds, bold text on light slides |
| **White** | `FFFFFF` | Text on dark slides, light backgrounds |
| **Dark Navy** | `1B2F3D` | Secondary dark background, accent text |
| **MX Pink** | `FF50C5` | Accent color, highlights |
| **MX Yellow** | `FFE900` | Accent color, highlights |
| **Light Gray** | `C8C8C8` | Dividers, secondary text, borders |
| **Warm Gray** | `D7D7D7` | Subtle backgrounds |

The **gradient bar** (pink → orange → yellow, left to right) is an embedded
image (`image6.png`, 2048×15px RGBA) baked into the relevant slide layouts.
It appears automatically on white-background content slides via their layout.
Do not attempt to recreate it as shapes.

### Typography

All slides use the theme fonts via `+mj-lt` (major/headings) and `+mn-lt`
(minor/body), which resolve to:

- **Avenir Next LT Pro Demi** — both heading and body weight

**Font rules:**
- Headings: bold, left-aligned
- Body: regular weight, left-aligned
- Never center body text (brand rule #1)
- Font size follows template defaults — do not override unless necessary

### Logo

The MX logo appears in the top-right corner on most slides via the slide
master or layouts. Do not manually add or reposition the logo.

---

## Slide Layout Catalog

The template has 48 slides covering every common layout type. Map your
content to the appropriate template slide, then duplicate/delete as needed.

### Cover / Title Slides

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 2 | slide2.xml | slideLayout1 | **Photo cover** — full-bleed photo with dark overlay, large white title + italic subhead. Use for hero/opening slides. Typically only used for internal presentation. |
| 3 | slide3.xml | slideLayout2 | **Minimal black cover** — black bg, white title + italic subhead, b&w small MX logo visible in top-right. Clean/simple variant. |
| 4 | slide4.xml | slideLayout5 | **White cover with MX logo** — white bg, black title + italic subhead, MX logo top-right. Most common cover. |
| 5 | slide5.xml | slideLayout6 | **Black cover** — black bg, white title + italic subhead, MX logo. For bold/premium openers. |

### Agenda Slides

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 6 | slide6.xml | slideLayout9 | **Numbered list agenda** — black left panel with "Agenda" label, numbered items on right. |
| 7 | slide7.xml | slideLayout10 | **Time-based agenda** — black left panel, time + session name rows on right. |

### Divider / Section Break Slides

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 8 | slide8.xml | slideLayout11 | **Black bold divider** — black bg, extra-large bold white title. Maximum impact. |
| 9 | slide9.xml | slideLayout12 | **Black minimal divider** — black bg, MX logo only. Use between major sections. Gradient bar footer. |
| 10 | slide10.xml | slideLayout14 | **White bold divider** — white bg, large black bold title. Lighter section break. |
| 11 | slide11.xml | slideLayout15 | **White divider with gradient bar** — white bg, large black bold title, gradient bar footer. |

### Comparison / From–To Slides

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 12 | slide12.xml | slideLayout17 | **From–To (light)** — black left, white right bg, two columns with "From" / "To" headers, bullet rows. |
| 13 | slide13.xml | slideLayout18 | **From–To (light)** — black left, white right bg, same two-column From/To structure, except one larger text box in each column. |

### Multi-Column Non-Bulleted Content Slides

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 14 | slide14.xml | slideLayout19 | **Two-column heavy text** — black left, white right bg with diagonal split, two large text boxes in each column. Same bg as from-to slides. |
| 27 | slide27.xml | slideLayout32 | **Two-column with title** — white bg, title top-left, two equal text columns below. |
| 29 | slide29.xml | slideLayout34 | **Three-column with title** — white bg, title, three content columns. |
| 31 | slide31.xml | slideLayout36 | **Two-column with section label** — adds small section indicator. |
| 33 | slide33.xml | slideLayout38 | **Two-column statement** — white bg, small section label top-left |


### Team Slide

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 15 | slide15.xml | slideLayout20 | **Meet the Team** — white bg, 8-person grid (2 rows × 4), headshot + name + title per person. Note: Do not attempt to add the actual headshots yourself. The end user will handle this themselves. |

### Standard Content Slides (White Background)

These are the workhorses. All have the pink→yellow gradient bar at the bottom.

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 16 | slide16.xml | slideLayout21 | **Title + body (two text boxes)** — type title in top box, body content in bottom box. |
| 17 | slide17.xml | slideLayout22 | **Title + body + footnote** — same as 16 but with footnote field at bottom. |
| 18 | slide18.xml | slideLayout23 | **Single text box** — title first, then indent for body. Uses PowerPoint indent formatting. |
| 19 | slide19.xml | slideLayout24 | **Title + bullets (two text boxes)** — bulleted body content. |
| 20 | slide20.xml | slideLayout25 | **Single text box with bullets** — title then bulleted content in one box. |

### Pull Quote

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 21 | slide21.xml | slideLayout26 | **Pull quote** — white bg, large italic quote text, attribution line below. |

### Chart Slides

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 23 | slide23.xml | slideLayout28 | **Chart with section label** — white bg, title top-left, chart area below. |
| 24 | slide24.xml | slideLayout29 | **Chart with section label (alt)** — same as 23, except there is a small header box above title. |

### Table / Data Slides

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 25 | slide25.xml | slideLayout30 | **Data table (pink header)** — title, description, table with pink header row. |
| 26 | slide26.xml | slideLayout31 | **Data table (pink header)** — same structure as 25, except there is a small header box above title. |
| 45 | slide45.xml | slideLayout50 | **Budget/cost table** — itemized rows with ITEM + RANGE columns, subtotals, total row. |
| 46 | slide46.xml | slideLayout30 | **Comparison table** — three-column pros/cons or option comparison. | 

### Multi-Column Bulleted Layout Slides

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 28 | slide28.xml | slideLayout33 | **Two-column bullets** — title, two side-by-side bullet columns. |
| 30 | slide30.xml | slideLayout35 | **Three-column bullets** — white bg, title, three bullet columns. |
| 32 | slide32.xml | slideLayout37 | **Three-column with dividers** — title, three columns separated by vertical lines. |

### Image + Text Slides

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 22 | slide22.xml | slideLayout27 | **Chart with two images** — white bg, title top-left, two images side by side below. No main content text box, should be used primarily to highlight two images. |
| 34 | slide34.xml | slideLayout40 | **Image right, text left** — half image, half text with headline and body. |
| 35 | slide35.xml | slideLayout42 | **Image right, text left (with label)** — adds section label above headline. |
| 36 | slide36.xml | slideLayout44 | **Image big right, text left** — wide image, very title + body below. Mainly used to highlight an image with minimal descriptor text. |
| 37 | slide37.xml | slideLayout46 | **Image big left, text right** — mirrored version of 36. |
| 38 | slide38.xml | slideLayout47 | **Full image highlight** — Slide entirety is an image. Used to highlight an image with no descriptor. |


### Quadrant / Matrix Slides

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 39 | slide39.xml | slideLayout48 | **2×2 quadrant** — black bg, four equal quadrants (black top left & bottom right, white top right and bottom left), text in each. |

### 5-Column / Pillar Slides

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 40 | slide40.xml | slideLayout51 | **5-column pillars** — five equal columns with header and body text each. Middle column has a box around the content. Should focus on most major point. |
| 41 | slide41.xml | slideLayout51 | **5-column pillars (highlighted)** — same layout, just more MX coloring added. Text is also slightly larger, allowing for a visual pop. |

### Strategic / MX Manifesto Slides

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 42 | slide42.xml | slideLayout42 | **Strategic pillars (compact)** — MX "B2B Like It Oughta Be" manifesto, short form. |
| 43 | slide43.xml | slideLayout42 | **Strategic pillars (expanded)** — same with supporting RTBs and examples. |

### Timeline / Gantt Slide

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 44 | slide44.xml | slideLayout49 | **Timing/Gantt** — weekly columns, project phases with pink bars. |

### Closing Slides

| Slide | XML | Layout | Description |
|-------|-----|--------|-------------|
| 47 | slide47.xml | slideLayout52 | **Closing — MX tagline** — black bg, MX logo, "B2B Like It Oughta Be." |
| 48 | slide48.xml | slideLayout52 | **Closing — MX mission** — black bg, MX logo, "We Impact the Marketplace for Companies That Impact the World." |

---

## Building a Deck: Step-by-Step

### 1. Plan your slide sequence with the user

Before touching any files, map out what slides you need:

```
1. Cover (slide 2, 4, or 5)
2. Agenda (slide 6 or 7) — if needed
3. [Section divider] (slide 8, 10, or 11)
4. Content slides (slides 16–20 as primary workhorses)
5. [Charts/tables as needed]
6. Closing (slide 47 or 48)
```

Then come back to the user with that plan before making the presentation. Once intent is confirmed continue.

### 2. Unpack and build

```bash
cp /mnt/skills/user/mx-pptx/references/mx_powerpoint_template_v3.pptx /home/claude/output.pptx
python /mnt/skills/public/pptx/scripts/office/unpack.py /home/claude/output.pptx /home/claude/unpacked
```

Use `add_slide.py` to duplicate slides you need multiple copies of:

```bash
python /mnt/skills/public/pptx/scripts/add_slide.py /home/claude/unpacked slide16.xml
# Prints the <p:sldId> line to add to presentation.xml
```

Remove unwanted slides by deleting their `<p:sldId>` from `presentation.xml`,
then run `clean.py`.

### 3. Edit content

Edit each `slide{N}.xml` directly. Key rules:
- Use `b="1"` on `<a:rPr>` to bold headers
- Never use unicode bullets (`•`) — use `<a:buChar>` or `<a:buNone>`
- Separate list items into individual `<a:p>` elements — never concatenate
- Copy existing `<a:pPr>` from original paragraphs to preserve line spacing
- Use XML entities for smart quotes: `&#x201C;` / `&#x201D;`

### 4. Clean and repack

```bash
python /mnt/skills/public/pptx/scripts/clean.py /home/claude/unpacked
python /mnt/skills/public/pptx/scripts/office/pack.py /home/claude/unpacked /home/claude/output.pptx --original /mnt/skills/user/mx-pptx/mx_powerpoint_template_v3.pptx
```

---

## QA

### Visual QA (required)

```bash
python /mnt/skills/public/pptx/scripts/office/soffice.py --headless --convert-to pdf /home/claude/output.pptx --outdir /home/claude
pdftoppm -jpeg -r 150 /home/claude/output.pdf /home/claude/slide
ls -1 "$PWD"/home/claude/slide-*.jpg
```

View each slide image and check for:
- Text overflow or cutoff
- Gradient bar visible on white slides (should auto-appear from layout)
- Left-alignment on all body text (brand rule #1)
- MX logo in correct position
- No ampersands in any text
- Content vertically centered

### Content QA

```bash
extract-text /home/claude/output.pptx | grep -i '&\b'  # Should return nothing — no ampersands
```

---

## Common Mistakes to Avoid

- **Don't center text** — brand rule #1, every element left-aligned
- **Don't add the gradient bar manually** — it's in the layout, it appears automatically
- **Don't reposition the MX logo** — it's in the master/layout
- **Don't mix dark and light slides without a divider** — use a section break slide between major topic changes
- **Don't use slide 1 (brand rules reference) in output decks** — it's a reference slide only
- **Don't use "and &"** — always spell out "and"
- **Don't leave placeholder lorem ipsum text** — run `extract-text` and grep for common placeholder patterns
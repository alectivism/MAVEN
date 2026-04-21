---
name: mma-pptx-builder
description: Generates PowerPoint decks, presentations, and slides for MMA using the official template, correct slide masters, and approved shape patterns. Use when creating, modifying, or rebuilding .pptx files, decks, presentations, or slides for MMA. Also use when asked to "make a deck", "build a presentation", "create slides", "generate a PowerPoint", or any PPTX/PPT task. Covers template workflow, master selection per think tank, shape construction (accent bar cards, callouts, tables, flow diagrams), and python-pptx implementation patterns.
---

# MMA PowerPoint Builder

Generate slides that inherit MMA's real template structure, use the correct master layouts, preserve logo/header positioning, and produce visually strong shape-based slides.

> **Colors, fonts, tone, naming:** See **mma-brand-guidelines** skill. This skill covers PPTX-specific construction rules only.

---

## 1. Template — Finding It

Always start from the official MMA template. Never build from a blank Presentation().

**Canonical template (as of April 2026):** `MMA PPT Template v2.1 2026.potx`. Identical masters/layouts/colors to the Dec 2025 template, minus one duplicate (`1_Title and Content Black Bullets`) that was removed from the Core Gold master.

### Template discovery — try in this order

**Step 1 — Local project directory (Claude Code / Cowork only; skip on claude.ai).**
Search the current working directory and common project subfolders for any file matching `MMA PPT Template*.potx` or `MMA_Template*.pptx`. If the user already has a local copy in the project, use that.

```bash
# Bash tool
find . -maxdepth 4 -iname "MMA*Template*.potx" -o -iname "MMA_Template*.pptx" 2>/dev/null
```

**Step 2 — SharePoint via Microsoft connector / MCP.**
Available to anyone with a MMA account. Most staff have at least read access.

- File name: `MMA PPT Template v2.1 2026.potx`
- Path: `MMA Internal > Documents > General > Marketing + Media Alliance MMA Brand Kit 2025`
- Direct link (anyone with the link can view): https://mmaglobalcom.sharepoint.com/:p:/s/MMAGlobal/IQASzy8PH_6cTZVea1p9qoTTAUzphRo3ozdN8T_qyHznFW4?e=QploEn

Use `mcp__claude_ai_Microsoft_365__sharepoint_search` or `mcp__ms365__search-onedrive-files` with the file name as the query. For most users this is the most reliable path.

**Step 3 — Synced OneDrive app (Claude Code / Cowork only).**
If the user has the OneDrive app on macOS and syncs the MMA Internal site, the file is at:

```
/Users/<username>/Library/CloudStorage/OneDrive-MMAGlobal/MMA Internal - Documents/General/Marketing + Media Alliance MMA Brand Kit 2025/MMA PPT Template v2.1 2026.potx
```

### POTX → PPTX one-time conversion

python-pptx cannot open `.potx` directly — it throws `ValueError: not a PowerPoint file`. Patch the content type:

```python
import zipfile

def potx_to_pptx(src_potx, dst_pptx):
    with zipfile.ZipFile(src_potx, "r") as zin, \
         zipfile.ZipFile(dst_pptx, "w", zipfile.ZIP_DEFLATED) as zout:
        for item in zin.infolist():
            data = zin.read(item.filename)
            if item.filename == "[Content_Types].xml":
                data = data.replace(
                    b"presentationml.template.main+xml",
                    b"presentationml.presentation.main+xml",
                )
            zout.writestr(item, data)

# Then open normally:
from pptx import Presentation
prs = Presentation("MMA_Template.pptx")
```

Once converted and saved locally as `.pptx`, new slides inherit the real masters, logo placement, slide numbers, and footer positions.

---

## 2. Slide Masters — pick the right one for the think tank

The template ships with **six masters**. Most LLMs reach for `prs.slide_layouts[i]` which only exposes **master 0**. To use the themed masters, access them via `prs.slide_masters[i].slide_layouts`.

| Idx | Master | Brand | Layouts | Use for |
|-----|--------|-------|---------|---------|
| 0 | **MMA Core Gold** | Gold `#FFA400` | 13 (rich) | Default MMA decks. Has Section Header, Appendix, 1/3 and 2/3 splits, Two Content, Content with Caption. |
| 1 | MMA MATT | Blue `#0047BB` | 6 | Measurement & Attribution Think Tank decks. |
| 2 | MMA MOSTT | Orange `#E25700` | 6 | Marketing Org Strategy Think Tank decks. |
| 3 | MMA DATT | Teal `#00AB84` | 6 | Data & CX Think Tank decks. |
| 4 | **MMA ALTT** | Peridot `#B5C900` | 6 | AI Leadership Think Tank decks. |
| 5 | Charts and Tables Clean | neutral | 6 | Data-heavy appendix slides. |

**Think tank masters (1–4) only have 6 layouts each:** `1_Title Slide`, `Title Slide`, `2_Title Slide`, `Title and Content`, `Title Only`, `1_Title Only`. They **do not have** Section Header, Appendix, or 1/3 / 2/3 splits — those only exist on Core Gold.

**Rule:** Use the think tank master for *every slide* in a think tank deck (including section dividers — see §7). Do not mix in Core Gold slides for dividers; the visual inconsistency is worse than repurposing a `2_Title Slide` as a divider.

### Helper — resolve a layout by master and name

```python
def layout_by_name(prs, master_idx, layout_name):
    master = prs.slide_masters[master_idx]
    for l in master.slide_layouts:
        if l.name == layout_name:
            return l
    raise KeyError(f"Layout '{layout_name}' not found on master {master_idx}")

# Core Gold usage
layout = layout_by_name(prs, 0, "Title and Content Black Bullets")
# ALTT usage
layout = layout_by_name(prs, 4, "Title and Content")
```

### Layout map — Core Gold master (master 0)

| Name | Use for |
|------|---------|
| `1_Title Slide` | First slide of a Core Gold deck. |
| `Title and Content Black Bullets` | **Default workhorse** for content slides. Preserves title + accent bar; no unused body placeholder. |
| `Title and Content Gold Bullets` | Bullet-list slides only. Has body placeholder. |
| `Section Header` | Section dividers (Core Gold only). |
| `Title and Content 2/3` / `1/3` | Two-column ratio layouts. |
| `Two Content` | True two-column with native placeholders. |
| `1_Blank` | Full-bleed imagery ONLY. |
| `Appendix` | Appendix divider. |

### Remove sample slides before building

```python
from pptx.oxml.ns import qn

def strip_slides(prs):
    sldIdLst = prs.slides._sldIdLst
    for sld in list(sldIdLst):
        rId = sld.get(qn("r:id"))
        prs.part.drop_rel(rId)
        sldIdLst.remove(sld)
```

---

## 3. Placeholder Hygiene — clean up what you don't use

Layouts come with default placeholders (title, subtitle, body, footer, slide number). **If you don't use a placeholder, delete it from the slide** — empty placeholders render "Click to add..." ghost text and break shape-driven layouts.

```python
def delete_unused_placeholders(slide, keep_idxs=(0, 12)):
    """Delete every placeholder except the ones you plan to use.
    Defaults: keep title (0) and slide number (12).
    """
    for ph in list(slide.placeholders):
        if ph.placeholder_format.idx not in keep_idxs:
            sp = ph._element
            sp.getparent().remove(sp)

# Usage — after adding slide, before adding shapes:
slide = prs.slides.add_slide(layout)
delete_unused_placeholders(slide)             # keep title + slide number only
# For layouts with a subtitle you want: keep_idxs=(0, 1, 12)
```

**When to delete:**
- Title Only / 1_Title Only layouts: keep placeholder 0 (title) and 12 (slide number). Delete all others.
- Title Slide: keep 0 (title), 1 (subtitle), 12. Delete the rest.
- Anything with a body placeholder you're not filling: delete it (don't just set `ph.text = ""`).

**When to keep:**
- If the layout's native subtitle placeholder is in the right position (e.g., the Title Slide — see §5), use it instead of adding a fresh text box. That preserves the template's designed spacing and reduces the odds the subtitle lands in the wrong spot.

---

## 4. Typography (PPTX-Specific Sizes)

### The font size floor — read this first

**14pt is the preferred minimum for all text on a slide. 12pt is the absolute floor.** Never go below 12pt. If 14pt doesn't fit, the slide has too much content — cut words, don't shrink type.

This applies to *everything*: card body copy, kicker labels, contact strips, footer source lines, tag badges, status tags, mini-card descriptions. 10pt and 11pt are not acceptable for any user-facing text, ever.

### Size table

| Element | Font | Size | Notes |
|---------|------|------|-------|
| **Slide title (content slides)** | **Söhne Halbfett** | **36pt default** | 32pt only if it would otherwise wrap to 2 lines. If 32pt still wraps, the title is too long — rewrite it. |
| Title slide title | Söhne Halbfett | 44–54pt | Usually the template's default size on `Title Slide` layout — leave it unless you're certain. |
| Section header title | Söhne Halbfett | 60pt | |
| Subtitle | Söhne Leicht | 18–20pt | Gray (`#666666`). Use the layout's native subtitle placeholder when one exists. |
| Body / bullets | Söhne Leicht | 16–20pt | |
| Card titles | Söhne Halbfett | 18–22pt | |
| Card body | Söhne Leicht | 14–16pt | |
| Kicker labels / eyebrow text | Söhne Halbfett | 14pt | Uppercase, accent color. |
| Table text | Söhne Leicht | 14–16pt | Headers in Halbfett. |
| Footnotes / source lines | Söhne Leicht | **14pt preferred, 12pt absolute min** | Never below 12pt. |
| Callout labels | Söhne Halbfett | 16pt | Accent color, inline with body. |

**Default to 36pt for content slide titles.** 28pt is too small — it's a sign the title is being auto-shrunk to fit a cluttered slide. Fix the slide, not the font.

**If you find yourself wanting to use 10pt or 11pt:** the answer is always to remove content from the slide, not shrink the type. Split into two slides, drop a column, or cut the optional detail. Never make small text smaller.

Leading: +10% for headlines, +20% for body. Tracking: 0.

---

## 5. Title Slide — use the native layout, don't freelance

The template's `Title Slide` and `1_Title Slide` layouts have background art (diagonal color slash on the right, think tank brandmark top-left if applicable, MMA logo bottom-right) plus positioned placeholders for:

- Title (left side, ~3" from top)
- Subtitle (below title, with a small accent bar above it)
- Presenter / event / date meta (bottom area)

**Use the existing placeholders.** Don't add freshly positioned text boxes — they land in the wrong place and clash with the template's accent bar.

```python
def set_title_slide(slide, title, subtitle=None, meta=None):
    for ph in slide.placeholders:
        idx = ph.placeholder_format.idx
        if idx == 0:
            ph.text_frame.text = title
            for p in ph.text_frame.paragraphs:
                for r in p.runs:
                    r.font.name = "Söhne Halbfett"
                    r.font.bold = True
        elif idx == 1 and subtitle is not None:
            ph.text_frame.text = subtitle
            for p in ph.text_frame.paragraphs:
                for r in p.runs:
                    r.font.name = "Söhne Leicht"
        # Extra meta (e.g. presenter name) -> find the next text placeholder if layout has one,
        # otherwise add a single text box *only* under the subtitle block, not at random coordinates.
```

**Never add edge-to-edge accent bars to the title slide.** The template already has its own colored diagonal slash on the right side. Additional left-side bars are redundant and misaligned.

---

## 6. Section Divider Slides — no edge-to-edge bars

For section dividers inside a think tank deck, repurpose the master's `2_Title Slide` layout. **Do not add a full-height colored rectangle on the left edge** — the master already provides background accent treatment. Just place the kicker + title within the existing content zone.

```python
def section_divider(prs, master_idx, kicker, title_text):
    layout = layout_by_name(prs, master_idx, "2_Title Slide")
    s = prs.slides.add_slide(layout)
    delete_unused_placeholders(s, keep_idxs=(0,))

    for ph in s.placeholders:
        if ph.placeholder_format.idx == 0:
            ph.left = Inches(1.0)
            ph.top = Inches(3.0)
            ph.width = Inches(11)
            ph.height = Inches(2.2)
            tf = ph.text_frame
            tf.clear()
            p = tf.paragraphs[0]
            r = p.add_run()
            r.text = title_text
            r.font.name = "Söhne Halbfett"
            r.font.size = Pt(60)
            r.font.bold = True

    # Kicker goes above the title — small, colored, uppercase
    # (positioned via a single overlaid text box, NOT a full-bleed bar)
    add_textbox(s, Inches(1.0), Inches(2.4), Inches(10), Inches(0.5),
                kicker.upper(), "Söhne Halbfett", 16, accent_color, bold=True)
    return s
```

**Rule:** The template carries its own accent treatments on every master. When in doubt, add less, not more.

---

## 7. Logo Protection — white backing rect when content overlaps

The MMA logo sits at the **bottom-right, roughly 1.2" × 0.8"** on every non-title layout. Content that extends into that zone will visually collide with the logo.

**Preferred:** stop content above the logo zone (keep `bottom_of_content <= slide_h - Inches(0.9)`).

**When unavoidable** (dense tables, full-width cards that reach the bottom):

```python
def protect_logo(slide):
    """Insert a white backing rectangle behind the logo area.
    Put this at the FRONT of the shape tree so it sits behind everything else,
    but ABOVE any card/table that would overlap the logo.

    In practice: call AFTER adding your content, then bump z-order down.
    """
    sw, sh = slide.part.package.presentation_part.presentation.slide_width, \
             slide.part.package.presentation_part.presentation.slide_height
    box_w = Inches(1.4)
    box_h = Inches(0.95)
    rect = slide.shapes.add_shape(
        MSO_SHAPE.RECTANGLE,
        sw - box_w - Inches(0.1),
        sh - box_h - Inches(0.05),
        box_w, box_h,
    )
    rect.fill.solid(); rect.fill.fore_color.rgb = RGBColor(0xFF, 0xFF, 0xFF)
    rect.line.fill.background()
    # This shape must sit BELOW the logo (so logo still shows) but ABOVE
    # the overlapping content. Easiest: draw your content card, THEN call
    # protect_logo, THEN re-add the MMA logo image on top if the layout loses it.
    return rect
```

In practice the simplest pattern is:

1. Draw the content card first.
2. On top of the corner that overlaps the logo, draw a white rectangle (same fill as the slide background) sized to the logo's bounding box.
3. Leave the logo alone — it's part of the layout master and will render on top of the white rect if its shape index is higher.

If the card must visually fill the whole bottom, accept that the logo must show *inside* a neutral white zone cut into the card's bottom-right corner.

---

## 8. Color Usage in Slides

> Full hex/RGB values are in **mma-brand-guidelines**. Key PPTX rules:

- **75% tints** for shape fills, accent bars, card backgrounds, diagram nodes. Use tints far more than full-saturation fills.
- **Full-saturation colors** for text accents, borders, small bars, emphasis, title colors on accent-bar cards.
- **NEVER same-hue text on same-hue background.** No gold text on gold card. No peridot text on peridot card.
- **Card body text:** Black (`#000000`) or dark gray (`#333333`) on light fills. White (`#FFFFFF`) on dark fills.
- **Default slide bg:** White. Section dividers inherit master background — do not repaint.

### Quick reference (PPTX constants)

```python
from pptx.dml.color import RGBColor

def hex_rgb(h):
    """RGBColor from 6-char hex like 'B5C900'."""
    h = h.lstrip("#")
    return RGBColor(int(h[0:2], 16), int(h[2:4], 16), int(h[4:6], 16))

# Full saturation
GOLD      = hex_rgb("FFA400")
SAPPHIRE  = hex_rgb("0047BB")
TOPAZ     = hex_rgb("E25700")
EMERALD   = hex_rgb("00AB84")
PERIDOT   = hex_rgb("B5C900")

# 75% tints (card fills, accent bars)
LT_GOLD     = hex_rgb("FFBB4D")
LT_SAPPHIRE = hex_rgb("4079D6")
LT_TOPAZ    = hex_rgb("E98B4D")
LT_EMERALD  = hex_rgb("66CDB7")
LT_PERIDOT  = hex_rgb("D6E466")

# Neutrals
BLACK       = hex_rgb("000000")
WHITE       = hex_rgb("FFFFFF")
DARK_GRAY   = hex_rgb("333333")
MED_GRAY    = hex_rgb("666666")
LIGHT_BG    = hex_rgb("F5F5F5")
CREAM       = hex_rgb("FFF5E0")
ALT_ROW     = hex_rgb("EBEBEB")
BORDER_CLR  = hex_rgb("E0E0E0")
```

---

## 9. Shape Construction Patterns

### General rules

- Use shapes actively. Most slides should be shape-driven, not plain bullets.
- Put text inside shapes whenever possible.
- Internal margins: `margin_left=0.25"`, `margin_right=0.25"`, `margin_top=0.15"`, `margin_bottom=0.15"`.
- Keep corner radii subtle (`adj=5000`), not cartoonish.
- Keep card groups centered and balanced.

### Centering a row of cards

```python
def center_x(total_width, slide_width):
    return (slide_width - total_width) / 2

# Usage:
n = 3
card_w = Inches(4.0)
gap = Inches(0.3)
total = card_w * n + gap * (n - 1)
start_x = center_x(total, prs.slide_width)
```

### Tight corner radius helper

```python
from pptx.oxml.ns import qn
from lxml import etree

def set_tight_corners(shape, adj=5000):
    sp = shape._element
    prstGeom = sp.find(".//" + qn("a:prstGeom"))
    if prstGeom is not None:
        avLst = prstGeom.find(qn("a:avLst"))
        if avLst is None:
            avLst = etree.SubElement(prstGeom, qn("a:avLst"))
        for gd in avLst.findall(qn("a:gd")):
            avLst.remove(gd)
        gd = etree.SubElement(avLst, qn("a:gd"))
        gd.set("name", "adj")
        gd.set("fmla", f"val {adj}")

def add_rounded_card(slide, left, top, width, height, fill_color=LIGHT_BG):
    from pptx.enum.shapes import MSO_SHAPE
    s = slide.shapes.add_shape(MSO_SHAPE.ROUNDED_RECTANGLE, left, top, width, height)
    s.fill.solid(); s.fill.fore_color.rgb = fill_color
    s.line.fill.background()
    set_tight_corners(s)
    return s
```

### Text box + paragraph helpers

```python
def add_textbox(slide, left, top, width, height, text, font_name, size_pt,
                color=BLACK, bold=False, align=None, line_spacing=1.2):
    from pptx.util import Pt, Inches
    from pptx.enum.text import PP_ALIGN
    tb = slide.shapes.add_textbox(left, top, width, height)
    tf = tb.text_frame
    tf.word_wrap = True
    tf.margin_left = tf.margin_right = Inches(0.05)
    lines = text.split("\n") if isinstance(text, str) else text
    for i, line in enumerate(lines):
        p = tf.paragraphs[0] if i == 0 else tf.add_paragraph()
        if align is not None: p.alignment = align
        p.line_spacing = line_spacing
        r = p.add_run()
        r.text = line
        r.font.name = font_name
        r.font.size = Pt(size_pt)
        r.font.bold = bold
        r.font.color.rgb = color
    return tb

def add_para(tf, text, font_name="Söhne Leicht", size_pt=14,
             color=DARK_GRAY, bold=False, align=None, first_paragraph=False):
    """Add a paragraph to an existing text frame (e.g. inside a shape)."""
    from pptx.util import Pt
    p = tf.paragraphs[0] if first_paragraph else tf.add_paragraph()
    if align is not None: p.alignment = align
    r = p.add_run()
    r.text = text
    r.font.name = font_name
    r.font.size = Pt(size_pt)
    r.font.bold = bold
    r.font.color.rgb = color
    return p
```

---

## 10. Accent Bar Cards

**Top-bar cards are the canonical default.** More reliable than left-bar cards for correct corner geometry.

### Top bar card (preferred)

Sharp top corners, rounded bottom corners. Built from a rounded rectangle + a rectangle accent bar flush on top.

```python
def add_top_bar_card(slide, left, top, width, height,
                     fill_color=LIGHT_BG, bar_color=GOLD,
                     bar_height=None, border=True):
    from pptx.util import Inches, Pt
    if bar_height is None: bar_height = Inches(0.08)
    card = add_rounded_card(slide, left, top, width, height, fill_color)
    if border:
        card.line.color.rgb = BORDER_CLR
        card.line.width = Pt(0.75)
    bar = add_rect(slide, left, top, width, bar_height, bar_color)
    return card, bar
```

Text goes in separate overlaid text boxes (see §9 helpers). Keep interior padding ≥ 0.25" left/right.

### Left bar card (fallback only)

```python
card = add_rounded_card(slide, x, y, card_w, card_h, LIGHT_BG)
tf = card.text_frame
tf.word_wrap = True
tf.margin_left = Inches(0.35)   # extra to clear the bar
tf.margin_right = Inches(0.2)
# ... add paragraphs inside the card ...

# Bar AFTER the card so it sits above in z-order and masks the rounded left corners
add_rect(slide, x, y, Inches(0.07), card_h, LT_GOLD)
```

---

## 11. Callout Boxes

### Cream callout (default)

```python
box = add_rounded_card(slide, left, top, width, height, CREAM)
box.line.color.rgb = GOLD
box.line.width = Pt(2)
tf = box.text_frame
tf.word_wrap = True
tf.margin_left = Inches(0.3); tf.margin_right = Inches(0.3); tf.margin_top = Inches(0.15)
p = tf.paragraphs[0]
r1 = p.add_run()
r1.text = "Tip: "
r1.font.name = "Söhne Halbfett"; r1.font.size = Pt(16)
r1.font.color.rgb = GOLD; r1.font.bold = True
r2 = p.add_run()
r2.text = "Your tip text here."
r2.font.name = "Söhne Leicht"; r2.font.size = Pt(14)
r2.font.color.rgb = DARK_GRAY
```

- **Dark:** black fill, gold label, white body.
- **Neutral:** `#F5F5F5` fill, `#E0E0E0` 1pt border, dark gray label.

---

## 12. Tables

Always use native PowerPoint table objects. Never fake tables with shapes.

```python
def style_table(table):
    tbl_el = table._tbl
    tblPr = tbl_el.find(qn("a:tblPr"))
    if tblPr is None:
        tblPr = etree.SubElement(tbl_el, qn("a:tblPr"))
    tblPr.set("bandRow", "0"); tblPr.set("firstRow", "0"); tblPr.set("firstCol", "0")
    for ts in tblPr.findall(qn("a:tblStyle")):
        tblPr.remove(ts)

def set_cell_border(cell, color_hex="D0D0D0", width_pt=0.5):
    tc = cell._tc
    tcPr = tc.find(qn("a:tcPr")) or etree.SubElement(tc, qn("a:tcPr"))
    for side in ("lnL", "lnR", "lnT", "lnB"):
        ln = tcPr.find(qn(f"a:{side}"))
        if ln is not None: tcPr.remove(ln)
        ln = etree.SubElement(tcPr, qn(f"a:{side}"))
        ln.set("w", str(int(width_pt * 12700)))
        ln.set("cmpd", "sng")
        sf = etree.SubElement(ln, qn("a:solidFill"))
        srgb = etree.SubElement(sf, qn("a:srgbClr"))
        srgb.set("val", color_hex)
```

| Element | Style |
|---------|-------|
| Header row (A) | Dark gray `#333333` fill, white bold Halbfett. |
| Header row (B) | Accent color fill, white bold Halbfett. |
| Odd data rows | White fill. |
| Even data rows | `#EBEBEB` fill. |
| First column | Bold Halbfett on same row fill. |
| Borders | `#D0D0D0`, all sides + inner. |
| Cell margins | 0.12" left/right, 0.08" top/bottom. |
| Text | Söhne Leicht 14pt, left-aligned. |

---

## 13. Flow Diagrams

```python
from pptx.enum.shapes import MSO_SHAPE

# Nodes: rounded rectangle with 75% tint fill
node = slide.shapes.add_shape(MSO_SHAPE.ROUNDED_RECTANGLE, x, y, w, h)
node.fill.solid(); node.fill.fore_color.rgb = LT_GOLD
node.line.fill.background()
set_tight_corners(node, adj=5000)

# Connectors: real RIGHT_ARROW shapes (NOT line + triangle)
arrow = slide.shapes.add_shape(MSO_SHAPE.RIGHT_ARROW, ax, ay, Inches(0.3), Inches(0.2))
arrow.fill.solid(); arrow.fill.fore_color.rgb = MED_GRAY
arrow.line.fill.background()
```

- ~0.15" gap between arrows and nodes on both sides.
- Labels go inside nodes, centered, Halbfett 14pt.
- Below-node labels: Leicht 14pt medium gray.

---

## 14. Other Elements

### Numbered circles

```python
s = slide.shapes.add_shape(MSO_SHAPE.OVAL, left, top, Inches(0.5), Inches(0.5))
s.fill.solid(); s.fill.fore_color.rgb = GOLD; s.line.fill.background()
tf = s.text_frame; tf.word_wrap = False
tf.margin_left = tf.margin_right = tf.margin_top = tf.margin_bottom = Inches(0)
p = tf.paragraphs[0]; p.alignment = PP_ALIGN.CENTER
r = p.add_run(); r.text = "1"
r.font.name = "Söhne Halbfett"; r.font.size = Pt(18)
r.font.color.rgb = WHITE; r.font.bold = True
```

### Separator lines

Thin rectangles, not line objects:

```python
s = slide.shapes.add_shape(MSO_SHAPE.RECTANGLE, left, top, width, Pt(1))
s.fill.solid(); s.fill.fore_color.rgb = BORDER_CLR; s.line.fill.background()
```

### Bullet formatting

```python
r.text = "•  " + item_text   # bullet + 2 spaces
```

---

## 15. Slide Number Helper

```python
def set_slide_number(slide, num):
    for ph in slide.placeholders:
        if ph.placeholder_format.idx == 12:
            ph.text = str(num)
            for r in ph.text_frame.paragraphs[0].runs:
                r.font.size = Pt(10); r.font.name = "Söhne Leicht"
                r.font.color.rgb = MED_GRAY
            return
```

---

## 16. Design Behavior Rules

- Most slides should be shape-driven, not plain bullets.
- One idea per slide. Lead with the takeaway, not a generic topic label.
- Don't let 3+ consecutive slides feel identically templated.
- Prefer clean layouts with white space. Don't fill every pixel.
- Keep groups visually centered using `center_x()`.
- When in doubt, add less, not more — the template carries its own visual language.

---

## 17. Critical DO NOT Rules

| Rule | Why |
|------|-----|
| **Never use custGeom** | Renders invisible shapes in PowerPoint. |
| **Never rotate shapes 90/270°** | Width/height swap breaks layouts. |
| **Never use flipV/flipH on shapes with text** | Text renders mirrored. |
| **Never put text inside a rotated R2S card** | Text is upside-down. Use overlaid text boxes. |
| **Never hand-build arrows from line + triangle** | Use `MSO_SHAPE.RIGHT_ARROW`. |
| **Never fake tables from shapes** | Use native PowerPoint tables. |
| **Never use Blank layout for content slides** | Full-bleed imagery only. |
| **Never build from blank Presentation()** | Always use the MMA template. |
| **Never add edge-to-edge accent bars** | The masters already have background accent treatments. Adding left/right full-height bars duplicates what's already there. |
| **Never leave unused default placeholders** | Delete them (see §3). Empty "Click to add title" ghosts leak into exports. |
| **Never freelance title slide text positions** | Use the layout's native title / subtitle placeholders (see §5). |
| **Never shrink content slide titles below 32pt** | 36pt default. 32pt only if wrapping would otherwise occur. Below that, the title is too long — rewrite it. |
| **Never mix Core Gold dividers into a think tank deck** | Use the think tank master's `2_Title Slide` as a divider (§7). |
| **Never set text below 12pt** | 14pt preferred, 12pt absolute floor. If content won't fit, cut content — don't shrink type (§4). |

---

## 18. Practical Revision Rule

If the user says a slide or pattern is good, preserve it. Don't reopen solved problems unless later feedback supersedes them. Iterate only on parts the user says still need work.

---

## 19. Claude for PowerPoint Add-in

When generating custom instructions for the Claude PowerPoint add-in (not python-pptx), apply the same brand logic but note:

- Edit points manually for true sharp bar-side corners on left-bar cards.
- Subtitle belongs in the same text box as the title (second paragraph) if no native subtitle placeholder exists.
- Follow the same master-selection rules (think tank → master 1–4, default → master 0).
- The add-in can achieve geometry python-pptx cannot (e.g., perfect left-bar corners via point editing).

---

## 20. Required Imports

```python
from pptx import Presentation
from pptx.util import Inches, Pt, Emu
from pptx.dml.color import RGBColor
from pptx.enum.text import PP_ALIGN, MSO_ANCHOR
from pptx.enum.shapes import MSO_SHAPE
from pptx.oxml.ns import qn
from lxml import etree
```

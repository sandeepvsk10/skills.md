---
name: sandeep-resume-style
description: "Use this skill whenever building, editing, or regenerating Sandeep Kumar's resume. Captures exact styling constants, formatting rules, section order, and design preferences so every version is consistent. Trigger on any mention of 'my resume', 'update resume', 'new resume version', 'tailor resume', or references to Sandeep's resume documents."
---

# Sandeep Kumar — Resume Style & Formatting Guide

## Document Setup

| Property | Value |
|----------|-------|
| Page size | US Letter (12240 × 15840 DXA) |
| Margins | 0.75 in all sides (1080 DXA) |
| Default font | Calibri |
| Body text size | 9pt (18 half-points) |
| Name size | 14pt (28 half-points) |
| Section heading size | 11pt (22 half-points) |

## Color Palette

| Element | Color |
|---------|-------|
| Name & section headings | Navy `#1F3A5F` |
| Dates, locations, secondary text | Gray `#555555` |
| Hyperlinks | Blue `#1155CC`, underlined |
| Body text | Default black |

## Typography & Spacing

- Font: Calibri throughout — no font mixing.
- Section headings: bold, navy, 11pt, with a navy bottom border (`BorderStyle.SINGLE`, size 6, space 2). Spacing: 200 before, 60 after.
- Job company line: bold, 9pt black. Date right-aligned via tab stop at content edge (10080 DXA), gray.
- Job title line: italic, 9pt navy. Location right-aligned, italic gray.
- Bullet items: 9pt, spacing 20 before/20 after. Indent: left 360, hanging 180. Use `LevelFormat.BULLET` (never unicode bullets).
- Contact line: centered, 9pt, bullet separator `•`.
- Tagline: centered, 9pt gray, pipe separator `|`.

## Right-Alignment Rule

All dates and locations hug the right margin. Tab stop position = content width = page width − left margin − right margin = 10080 DXA.

## Section Order

1. **Name** — centered, bold, navy, 14pt
2. **Tagline** — centered, gray, pipe-separated keywords
3. **Contact Info** — centered, with hyperlinks for LinkedIn, GitHub, Portfolio, Data Blog
4. **Professional Summary** — bulleted points (not paragraph prose)
5. **Professional Experience** — reverse chronological, each role with company header + title + bullets
6. **Technical Skills** — category: skills format (bold category, regular skills)
7. **Open Source Contributions** — grouped by subcategory (bold italic subheadings), bulleted with hyperlinks
8. **Certifications** — grouped by subcategory (bold italic subheadings), each with credential hyperlink and optional date
9. **Education** — same header/title format as experience, with relevant coursework

## Bullet Style Rules

- Experience bullets: action-verb led, past tense for completed roles, present tense for current role.
- Professional summary: short, punchy bullets — one line each where possible.
- Indent level: single level only, no nested bullets.

## Hyperlink Style

- Color: `#1155CC`, single underline.
- Used for: LinkedIn, GitHub, Portfolio, Blog, credential links, PR links.
- Display text is descriptive (e.g., "Credential", "GitHub PR", "LaRevela"), never raw URLs.

## Certification Formatting

- Grouped under bold italic subheadings (e.g., *Data Engineering*, *Graph Databases*, *Generative AI*).
- Each line: cert name + " | " + hyperlinked "Credential" + right-aligned date (if applicable).

## Technical Skills Formatting

- Each line: **Category:** comma-separated skills.
- No bullets — plain paragraphs with bold category labels.
- Spacing: 20 before/20 after per line.

## Key Preferences

- No tables used for layout — all alignment via tab stops.
- No unicode bullet characters — always use numbering config with `LevelFormat.BULLET`.
- Em dash (—) used between company and linked product name, and in job titles.
- En dash (–) used in date ranges.
- Smart quotes and apostrophes throughout.
- Single-page target when possible; content density over whitespace.
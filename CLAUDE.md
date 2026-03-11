# Portfolio Site — Claude Instructions

## Critical Safety Rules

### NEVER delete image files
Before deleting or "cleaning up" any file in `images/`, you MUST search every `.html` file in the project root for references to that filename. Use:

```
grep -r "filename.jpg" *.html
```

In March 2026, a Claude session incorrectly deleted 10 images (`SmA-02.jpg` through `SmA-07.jpg` and `strato-swe-01.jpg` through `strato-swe-04.jpg`) while adding new case study assets, labelling them "unused." They were actively referenced by `case-study-smart-agency.html` and `case-study-strato.html`. The user had to manually restore them from a backup.

**The rule: if you are not 100% certain a file is unused across the entire codebase, do not delete it.**

---

## Project Structure

```
portfolio-site/
├── index.html                          # Homepage
├── case-study-design-annotator.html    # Design Annotator case study (1st in grid)
├── case-study-smart-agency.html        # Smart Agency case study
├── case-study-strato.html              # STRATO data viz case study
├── case-study-design-system.html       # STRATO design system case study
├── case-study-inpera.html              # Inpera construction SaaS case study
├── case-study-babbel.html              # Babbel Magazine leadership case study
├── case-study-tripper-spiral.html      # Tripper Spiral experimental project
├── styles/
│   ├── main.css                        # Global styles, design tokens, layout
│   └── case-study.css                  # Shared case study page styles
├── images/                             # All images (see safety rule above)
└── videos/                             # Video assets (tripper-spiral-example.mp4)
```

## Tech Stack

- Pure HTML/CSS/JS — no build step, no framework
- Hosted as static files
- Google Fonts: Public Sans + Playfair Display

## Accessibility Standards

The site has been audited for WCAG 2.1 AA compliance. Maintain these patterns when editing:

- Every page has `<a href="#main-content" class="skip-to-main">Skip to main content</a>` as the first element in `<body>`
- Every page's `<main>` has `id="main-content"`
- All `<nav>` elements have `aria-label="Main navigation"`
- Decorative SVGs have `aria-hidden="true" focusable="false"`
- All images have descriptive `alt` text (not generic "preview" descriptions)
- Heading hierarchy: h1 → h2 (section headings) → h3 (subsection items) — do not skip levels

## Case Study Page Conventions

- Hero: `.case-study-hero` with page-specific modifier class
- Section structure: `<section class="case-study-section [variant]">` → `<div class="container">`
- Images: always wrapped in `<div class="case-study-image">`, always include `loading="lazy"` and descriptive `alt`
- Lightbox is injected by JavaScript at the bottom of each case study page
- "Next Project" navigation uses `.next-project` section at the bottom of `<main>`

### Do not constrain paragraph width with inline styles
The `.container` already limits content width to `1200px`. Do not add `max-width` to individual paragraph elements via inline `style` attributes — it causes paragraphs to appear narrower than their container with an ugly gap on the right. If a narrower reading column is needed for a specific section, use `<div class="container narrow">` (max-width: `800px`) instead.

### Case study navigation chain
The "Next Project" link at the bottom of each case study must follow the same order as the portfolio grid in `index.html`. The current chain is:

1. Design Annotator → Smart Agency
2. Smart Agency → STRATO
3. STRATO → Design System
4. Design System → Inpera
5. Inpera → Babbel
6. Babbel → Tripper Spiral
7. Tripper Spiral → Design Annotator *(loops back to first)*

If the grid order in `index.html` ever changes, or a new case study is added, update this chain and every affected `next-project-link` accordingly.

### Adding a new case study page
Copy an existing case study page as your starting point. The following are easy to miss:

1. `<a href="#main-content" class="skip-to-main">Skip to main content</a>` — first element in `<body>`
2. `id="main-content"` on `<main>`
3. `aria-label="Main navigation"` on `<nav class="main-nav">`
4. The full lightbox JavaScript block at the bottom of `<body>` (before `</body>`)
5. A `<li>` entry in the portfolio grid in `index.html`
6. A hero background image rule in `case-study.css` (`.case-study-hero--your-page-name`)
7. Update the "Next Project" link on the previous case study page to point to the new one

## Tripper Spiral — Exception Page

`case-study-tripper-spiral.html` is the only page with a dark theme. It does not follow the standard light case study layout. Key differences:

- Has a page-specific `<style>` block in `<head>` — this is intentional, not a mistake to clean up
- Uses its own dark colour palette (`#111` backgrounds, `#f0ede6` text, `#ff4e1a` accent, `#00d4aa` teal)
- Depends on `videos/tripper-spiral-example.mp4` — do not delete that file
- The dark section class `.ts-dark-section` and all `.ts-*` utility classes live only in that page's `<style>` block, not in `case-study.css`

## Design Tokens (key values)

- Primary colour: `#E63946`
- Container max-width: `1200px` (`--max-width`)
- Content width (narrow): `800px` (`--content-width`)
- Border radius: `8px` (`--border-radius`)
- Case study pages override spacing to be tighter than the homepage

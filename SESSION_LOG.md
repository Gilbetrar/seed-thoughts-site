# Session Log — Seed Thoughts Site

---

## Agent Session - Issue #1

**Worked on:** Issue #1 - Repo Setup + GitHub Pages Deployment Pipeline

**What I did:**
- Created all initial repo files: README.md, .gitignore, LICENSE (MIT), CNAME, index.html, CLAUDE.md
- Created `.github/workflows/deploy.yml` for GitHub Pages deployment via GitHub Actions
- index.html is a minimal placeholder with "Coming Soon" messaging and the book's design palette (golds, purples, cream)
- CLAUDE.md contains full project context for future agents

**What I learned:**
- This is a pure static site — no npm, no build tools, no frameworks
- GitHub Pages deployment uses the newer actions/deploy-pages@v4 approach (not the legacy gh-pages branch)
- The deploy workflow needs Pages enabled in repo settings with source set to "GitHub Actions"

**Mistakes made:**
- Initial CI failed because GitHub Pages wasn't enabled on the repo. Fixed by enabling via API: `gh api repos/Gilbetrar/seed-thoughts-site/pages -X POST -f build_type=workflow`, then re-ran the workflow.

**Codebase facts discovered:**
- Empty repo at start — first commit creates everything
- Remote is HTTPS (not SSH): https://github.com/Gilbetrar/seed-thoughts-site.git

**Verification:**
- HTTP 200 from https://gilbetrar.github.io/seed-thoughts-site/
- Page content contains "Seed Thoughts" text
- All acceptance criteria from issue #1 met

---

## Agent Session - Issue #2

**Worked on:** Issue #2 - Design System — Color Palette, Typography, Base Styles

**What I did:**
- Created `styles/variables.css` with CSS custom properties for colors (gold, amber, purple, mauve, cream, earth, text), typography (Cormorant Garamond headings, Source Sans 3 body), spacing (4px base unit), and layout tokens
- Created `styles/base.css` with reset, body defaults, heading styles, link styles with focus-visible, responsive font sizing at 768px and 375px breakpoints, smooth scrolling
- Created `styles/layout.css` with `.section`, `.container`, `.grid--2`, `.grid--3` classes and responsive padding
- Updated `index.html` to load Google Fonts (Cormorant Garamond + Source Sans 3), link the three stylesheets, and use CSS custom properties for the placeholder page
- Replaced inline `<style>` with design system variables; kept minimal page-specific styles inline

**What I learned:**
- Google Fonts renamed "Source Sans Pro" to "Source Sans 3" — use the new name but include Pro as fallback
- The `<hr>` element makes a clean divider using `var(--gradient-brand)` — no need for custom divider divs

**Mistakes made:**
- None

**Codebase facts discovered:**
- Styles now live in `styles/` directory with three files (variables, base, layout)
- All colors and spacing available via CSS custom properties (e.g., `var(--color-gold)`, `var(--space-8)`)

---

## Agent Session - Issue #3

**Worked on:** Issue #3 - Homepage Content — Hero, Book, About Joan, Contact

**What I did:**
- Converted book cover PDF to optimized JPG (194KB) and small version (54KB) using macOS `qlmanage` + `sips`
- Created `styles/page.css` with all page-specific component styles (nav, hero, book, about, contact, footer)
- Rebuilt `index.html` with full content: sticky nav, hero with book cover, book description with "Coming Soon" badge and genre tags, about Joan with botanical photo placeholder, contact with mailto link, footer
- Added meta description for SEO
- Responsive design tested at 375px (mobile) and 1200px (desktop)

**What I learned:**
- `magick` (ImageMagick) requires Ghostscript (`gs`) for PDF conversion — not installed on this system
- macOS `qlmanage -t -s 1200` generates high-quality PNG thumbnails from PDFs, then `sips` converts to JPG
- The `scroll-behavior: smooth` in base.css handles smooth scrolling for anchor links — no JS needed
- Joan's exact bio/book description text was not available; used placeholder content based on issue hints. Will need substitution during Issue #6 review.

**Mistakes made:**
- Tried ImageMagick first for PDF conversion, which failed due to missing Ghostscript. Switched to macOS native tools.

**Codebase facts discovered:**
- New file: `styles/page.css` — all page-level component styles
- New directory: `images/` — contains `book-cover.jpg` (194KB, 927x1200) and `book-cover-sm.jpg` (54KB, 464x600)
- Content is placeholder text, not Joan's exact email copy — needs substitution in Issue #6

---

## Agent Session - Issue #4

**Worked on:** Issue #4 - Stock Imagery + Visual Polish

**What I did:**
- Sourced 4 stock images from Unsplash CDN:
  - Forest path with golden sunlight (hero background)
  - Golden wheat field at sunset (section divider)
  - Majestic solitary tree (about section — evokes Joan's sycamore)
  - Seedlings sprouting from soil (book section — "seed thoughts" metaphor)
- Converted all images to WebP using `cwebp` with responsive sizes (sm/lg)
- Also converted existing book cover JPGs to WebP
- Used `<picture>` elements with `<source>` for WebP + responsive media queries
- Added `loading="lazy"` on all below-fold images
- Created `styles/animations.css` with:
  - Scroll fade-in via IntersectionObserver (`.fade-in` class)
  - Nav hover underline animation
  - CTA button glow on hover
  - Book tag lift on hover
  - Image scale on hover
  - `prefers-reduced-motion` support
- Added hero background image (subtle 12% opacity overlay)
- Added full-width section divider between book and about sections
- Replaced about section placeholder SVG with tree image
- Added Unsplash attribution in footer

**What I learned:**
- Unsplash renders client-side, so WebFetch can't scrape photo IDs — use direct CDN URLs: `images.unsplash.com/photo-{ID}?w=WIDTH&q=QUALITY&auto=format`
- `cwebp -q 65 -resize WIDTH 0` (height 0 = auto) produces good quality WebP under 200KB for most photos
- Forest/tree photos compress poorly in WebP compared to sky/field photos — may need lower resolution to hit size targets
- IntersectionObserver with threshold 0.15 gives a nice reveal timing

**Mistakes made:**
- Several Unsplash photo IDs were wrong (404s) — had to try multiple IDs and visually verify each download
- Initial hero bg was 308KB at q75/1600w — reduced to 1200w at q65 to get under 200KB

**Codebase facts discovered:**
- New file: `styles/animations.css` — scroll and hover animations
- New images: hero-bg, golden-field, tree, seedlings (each in .webp + -sm.webp)
- Book cover now has WebP variants alongside original JPGs

---

## Agent Session - Issue #5

**Worked on:** Issue #5 - Joan Review — Send Preview Email

**What I did:**
- Found existing full-page screenshots (desktop 1200px + mobile 375px) from a previous Playwright session
- Moved screenshots to `review/desktop.png` and `review/mobile.png`
- Drafted complete review email in HANDOFF.md with Gmail MCP parameters
- Committed screenshots, pushed to main, CI passed
- Added `needs-human` label to issue #5
- HANDOFF.md is gitignored (stays local for interactive session pickup)

**What I learned:**
- HANDOFF.md and SIGNAL.txt are already in .gitignore — they're local-only coordination files
- Playwright MCP can't launch when Chrome is already running ("Opening in existing browser session" error)
- Previous agent sessions may leave useful artifacts (screenshots in this case) that can be reused

**Mistakes made:**
- Tried two Playwright MCP providers, both failed due to existing Chrome process. Worked around by using pre-existing screenshots.

**Codebase facts discovered:**
- `review/` directory now contains desktop.png and mobile.png for Joan's preview
- .gitignore excludes HANDOFF.md and SIGNAL.txt by design

# Learnings — Seed Thoughts Site

## Project Overview
- Static HTML/CSS/JS site for Joan Woods Jackson's book
- Hosted on GitHub Pages, deployed via GitHub Actions on push to main
- No frameworks, no build tools, no npm

## Project Structure
```
index.html              — Main page (links all stylesheets)
styles/variables.css    — CSS custom properties (colors, fonts, spacing)
styles/base.css         — Reset, element defaults, responsive font sizing
styles/layout.css       — Section containers, grid, responsive structure
CNAME                   — Custom domain config
CLAUDE.md               — Agent instructions
.github/workflows/      — GitHub Actions deploy pipeline
styles/page.css         — Page-level component styles (nav, hero, sections)
styles/animations.css   — Scroll reveal + hover animations
images/book-cover.jpg   — Book cover (927x1200, 194KB)
images/book-cover-sm.jpg — Book cover small (464x600, 54KB)
images/*.webp           — WebP versions of all images (responsive -sm variants)
```

## Page Sections
- Sticky nav with smooth-scroll anchor links (#book, #about, #contact)
- Hero: book cover + title + CTA
- Book: description + "Coming Soon" badge + genre tags
- About: bio + botanical photo placeholder (awaiting author photo)
- Contact: mailto link to jacksonjo@aol.com
- Footer: copyright + attribution

## Content Status
- Bio and book description are Joan's real words (from her 2026-03-14 email), implemented in commit 6db066b
- Book cover is Joan's real cover (converted from her PDF attachment)
- Author photo not provided — tree/botanical image is a placeholder
- Site shared with Joan via confirmation email (2026-03-14); waiting for her feedback
- To check for feedback: search Gmail for `from:jacksonjo@aol.com` (any recent reply)

## Design System
- CSS custom properties in `styles/variables.css` — use `var(--color-gold)` etc.
- Colors: gold (#D4A853), amber (#C4843A), purple (#8B7B9E), mauve (#B5A0B5), cream (#F5F0E8), earth (#3A2E2A), text (#2C2420)
- Headings: Cormorant Garamond (Google Fonts), fallback Georgia
- Body: Source Sans 3 (Google Fonts), fallback system sans-serif
- Spacing: 4px base unit via `--space-N` tokens
- Layout: `.container`, `.section`, `.grid--2`, `.grid--3` classes available

## Commands
- No build/test/lint commands — static site
- Preview: open index.html in browser
- Deploy: push to main

## Image Pipeline
- All images served as WebP with `<picture>` elements for fallback
- Each image has two sizes: full (800-1200w) and small (-sm, 400-800w)
- Use `cwebp -q 65 -resize WIDTH 0 input.jpg -o output.webp` for conversion
- Keep all images under 200KB; reduce resolution before reducing quality
- Unsplash CDN: `images.unsplash.com/photo-{ID}?w=WIDTH&q=QUALITY&auto=format`
- Animations use IntersectionObserver with `.fade-in` class; `prefers-reduced-motion` respected

## Gotchas
- CNAME file must exist for custom domain; GitHub Pages needs it
- The deploy workflow uses actions/deploy-pages@v4 which requires Pages to be enabled in repo settings (Settings > Pages > Source: GitHub Actions)
- Pages was enabled via API: `gh api repos/Gilbetrar/seed-thoughts-site/pages -X POST -f build_type=workflow`
- Deployed site URL: https://gilbetrar.github.io/seed-thoughts-site/
- No npm/node — don't try to run npm commands; they will fail
- ImageMagick PDF conversion needs Ghostscript (`gs`) — not installed. Use `qlmanage -t -s SIZE` + `sips` instead for PDF→image on macOS
- Unsplash is client-rendered — can't scrape with WebFetch. Use direct CDN URLs instead

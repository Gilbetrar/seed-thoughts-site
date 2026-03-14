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
```

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

## Gotchas
- CNAME file must exist for custom domain; GitHub Pages needs it
- The deploy workflow uses actions/deploy-pages@v4 which requires Pages to be enabled in repo settings (Settings > Pages > Source: GitHub Actions)
- Pages was enabled via API: `gh api repos/Gilbetrar/seed-thoughts-site/pages -X POST -f build_type=workflow`
- Deployed site URL: https://gilbetrar.github.io/seed-thoughts-site/
- No npm/node — don't try to run npm commands; they will fail

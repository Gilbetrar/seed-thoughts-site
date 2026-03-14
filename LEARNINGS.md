# Learnings — Seed Thoughts Site

## Project Overview
- Static HTML/CSS/JS site for Joan Woods Jackson's book
- Hosted on GitHub Pages, deployed via GitHub Actions on push to main
- No frameworks, no build tools, no npm

## Project Structure
```
index.html          — Main page
CNAME               — Custom domain config
CLAUDE.md           — Agent instructions
.github/workflows/  — GitHub Actions deploy pipeline
```

## Design System
- Colors: golds (#C9A84C, #8B6914), purples (#7D5A8A), cream (#FFF8F0), earth (#4A3728, #6B5344)
- Typography: Georgia, Times New Roman (serif)
- Tone: warm, spiritual, accessible to older readers

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

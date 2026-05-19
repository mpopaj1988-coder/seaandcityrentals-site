# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this site is

Static HTML website for **Sea & City Rentals** — a vacation rental company in Tampa Bay, FL (seaandcityrentals.com). There is no build system, no framework, no package.json. Every page is a self-contained `.html` file with all CSS embedded inline via `<style>` tags.

## Site structure

| Path | Purpose |
|---|---|
| `index.html` | Homepage |
| `listing-*.html` | Individual property listing pages (9 properties across Tampa, Clearwater, IRB, Largo, St. Pete) |
| `guide-stpete.html`, `guide-clearwater.html`, `guide-tampa.html` | Area guides (restaurants, activities, etc.) for guests |
| `str-freedom-sales.html` | Sales landing page targeting other STR operators |
| `blog/index.html` | Blog index page |
| `blog/blog/week{N}-*.html` | Individual blog posts (pre-written, published on schedule) |
| `sitemap.xml` | Site sitemap |
| `publish-schedule.json` | Maps week numbers to publication dates for blog posts |
| `ga4 file/` | Archived older copies of pages (do not edit) |
| `images/tampa/` | Property images |

The `.lnk` files are Windows shortcuts from the original author's machine — ignore them.

## Blog publishing pipeline

All 26 blog posts are pre-written and live in `blog/blog/`. They are **not** published all at once — the GitHub Actions workflow (`.github/workflows/publish-weekly.yml`) runs every Monday at 8am UTC and **deletes posts whose `publish-schedule.json` date is still in the future**. This means:

- To add a new post: write it in `blog/blog/` and add an entry to `publish-schedule.json`
- The workflow does not copy or move files — it only removes not-yet-due posts and commits the deletion
- The `blog/.gitkeep` keeps the `blog/` directory tracked when all posts are removed

## Design system

Every page embeds the same design tokens inline. When editing or creating pages, use these values:

**Fonts** (loaded from Google Fonts):
- `Cormorant Garamond` — headings, pull quotes
- `Jost` — body copy, nav, labels

**Colors (main site palette)**:
```css
--sand: #F5EFE4
--sand-dark: #EDE4D4
--ocean: #1A3A4A
--ocean-mid: #2C5364
--teal: #3D8B8B
--teal-light: #5AAFAF
--gold: #C9A84C
--gold-light: #E2C068
--white: #FDFBF8
--text-dark: #1C1C1C
--text-mid: #4A4A4A
--text-light: #7A7A7A
```

Blog post articles use a slightly different palette (lighter `--ocean: #1a5f7a`, `--coral: #c9614a`).

## Dark mode handling

Pages force light mode explicitly with `color-scheme: light` and `!important` overrides on backgrounds and text. When editing pages, maintain the `@media (prefers-color-scheme: dark)` blocks that explicitly re-assert light colors — this pattern is intentional to prevent OS dark mode from inverting the design.

## Google Analytics

Some pages include GA4 tracking via `G-DSFX8E7CKB`. The `ga4 file/` directory contains older versions of pages that had GA4 added. Root-level pages are the canonical versions.

## Canonical URLs

Pages use `https://seaandcityrentals.com` as the base. Blog post URLs omit the `.html` extension in `og:url` and `rel=canonical` (e.g., `/blog/week1-st-pete-weekend-guide`), even though the actual file is `.html`.

## Making changes

Since there is no dev server or build step, edits are made directly to HTML files. To preview locally, open the file in a browser. To deploy, commit and push to `main` — the site is served directly from this repository.

When the GitHub Actions workflow runs, it only touches `blog/` — all other pages are live as committed.

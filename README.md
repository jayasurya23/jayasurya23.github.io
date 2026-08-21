# Jayasurya Bhaskar — Portfolio

Personal portfolio site. AI Engineer specializing in computer vision,
vision-language models, and production AI pipelines.

**Live:** https://jayasurya23.github.io

## Structure

- `index.html` — the entire site. Self-contained: all CSS, JS, and the
  profile photo (as a data URI) are inlined. No build step, no dependencies
  except Google Fonts (with a system-font fallback).
- `resume.pdf` — linked from the Resume button, and embedded in `index.html`
  as a blob fallback so it opens in any context.
- `.nojekyll` — tells GitHub Pages to serve the files as-is.

## Editing

Open `index.html` in any editor. Section content is marked with
`<!-- ═══ 01 · HOME ═══ -->` style comments.

Colour lives in two blocks at the top of the `<style>` element: a light
palette (six variables, with four alternates commented beneath) and a dark
palette directly under it. Everything else on the page is painted from those
tokens, so restyling either theme means editing one block.

## Theme

Light and dark, with three states: explicit light, explicit dark, and
"follow the OS" for anyone who has not pressed the toggle. Only an explicit
press is stored in `localStorage`, so the site keeps tracking the OS until
the visitor overrides it. A small inline script in `<head>` stamps the theme
before first paint so there is no white flash, and a `prefers-color-scheme`
rule covers visitors with JavaScript off.

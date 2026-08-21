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

Open `index.html` in any editor. The colour palette is six CSS variables at
the top of the `<style>` block, with four alternate palettes commented
beneath it. Section content is marked with `<!-- ═══ 01 · HOME ═══ -->`
style comments.

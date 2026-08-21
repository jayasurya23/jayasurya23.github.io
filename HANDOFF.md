# Handoff notes

Working notes so a new machine (or a fresh Claude Code session) can pick this up
without re-deriving anything.

## What's in here

| File | What it is |
| --- | --- |
| `index.html` | The entire portfolio site. Self-contained: CSS, JS, the profile photo, and the resume PDF are all inlined. No build step. |
| `resume.tex` | LaTeX source for the resume. |
| `resume.pdf` | Compiled resume. Linked from the site and embedded inside `index.html`. |
| `photo.jpg` | Original profile photo. Source backup only — the site uses an embedded copy. Gitignored. |
| `README.md` | Public-facing repo readme. |
| `.nojekyll` | Stops GitHub Pages running the files through Jekyll. |

Live site: **https://jayasurya23.github.io** (GitHub Pages, publishes from `main`).

## Site structure

Six tabbed panels, no scrolling on desktop: **Home → About → Experience → Why
Hire Me → Projects → Skills**. Panels swap in place; `#projects`-style deep
links work and so do arrow keys.

Things worth knowing before editing:

- **Palette** is two blocks at the top of the `<style>` element: a light one
  (six variables, four alternates commented beneath) and a dark one under it.
  Nothing on the page hardcodes a colour any more, so both themes restyle from
  those two blocks.
- **The dark block is written out twice** on purpose: once inside
  `@media (prefers-color-scheme:dark)` for visitors with JavaScript off, once
  under `:root[data-theme="dark"]` for an explicit choice. Plain CSS cannot
  share a declaration list across a media-query boundary and there is no build
  step, so **edit both copies or the two paths drift apart.**
- **Theme has three states**, not two: explicit light, explicit dark, and
  follow-the-OS. Only a button press writes to `localStorage`; until then the
  site tracks the OS live. The `<head>` carries a tiny inline script that
  stamps `data-theme` before first paint — keep it inline, an external file
  loads too late and the page flashes white.
- **Experience is the tightest panel on the site.** Nine bullets plus the
  education row leave almost no slack at 1440x820, which is why it has no
  header subtitle and uses a text link instead of the big pill CTA. Adding a
  bullet means taking height back somewhere else.
- **Sections** are marked with `<!-- ═══ 01 · HOME ═══ -->` style comments.
- **Photo** is a base64 data URI in the `.pfp, .brand__pic` rule. The source is an
  uncropped 3:2 landscape, so `background-size`/`background-position` are tuned to
  frame the face. If you swap in a square-cropped photo, change those to `cover`
  and `center`.
- **Resume button** builds a `blob:` URL from an embedded base64 copy of the PDF.
  This is deliberate: a plain relative link fails in sandboxed viewers, and the
  `download` attribute is blocked on `file://` and in sandboxes.
- **Every panel is sized to fit one viewport.** After editing, check that no panel
  overflows at roughly 1440x820, **in both themes**, or the no-scroll design
  breaks. Quickest check, no dependencies beyond Playwright:

  ```js
  // per panel: document.documentElement.scrollHeight - innerHeight  // must be <= 0
  ```

## Resume structure

11pt, one page. Sections: Summary, Areas of Specialization, Experience, Projects,
Education.

- **Spacing knobs** are grouped at the top of `resume.tex` (`\bulletgap`,
  `\listgap`, `\secgapbefore`, `\secgapafter`, `\jobgap`, `\projgap`). Adjust these
  before cutting content. `\bulletgap` gives the most visible payoff per point.
- Bullets follow **XYZ**: did X using tools Y, producing concrete outcome Z.
  Outcomes are specific, not generic percentages.
- **Bold sparingly** — only product names, tool/model names, and hard numbers.
  Bold does nothing for ATS parsing; it's purely a human scanning aid, so heavy
  bolding costs readability and buys nothing.
- No em dashes anywhere. The `--` in date ranges are en dashes, which is correct.

## Open items

1. **`resume.tex` is behind `resume.pdf`.** The PDF in this repo was supplied
   already compiled and is a later revision than the LaTeX: it adds an "Areas
   of Specialization" section and a portfolio link, tightens the summary, and
   **says YOLOv5 where `resume.tex` says YOLOv8.** The site now follows the
   PDF. Decide which is right, then bring the other two into line — right now
   regenerating `resume.pdf` from `resume.tex` would silently undo the update.
2. **BLIP's LLM is unnamed.** Both the site and the resume say "LLM layer" /
   "LLM search" because the specific model was never confirmed. Name it.
3. **Dropped skill keywords.** The site's Skills panel carries the full list,
   but the resume's one-line specializations section still omits scikit-learn,
   XGBoost, Docker, SageMaker, Bedrock, Vertex AI, RAG, React/TypeScript,
   SQLAlchemy, and Entra ID. Consider a compact "Also:" line there if
   keyword-filtered screens are a concern.
4. **Repo names don't match project names.** The site calls them SecondSight and
   RootCause; the GitHub repos are still `Warning-system-for-automobile-drivers`
   and `Dental-X-Ray-Detection`. Renaming the repos would fix the mismatch
   (old URLs auto-redirect).

## Updating the live site

Edit `index.html`, commit, push to `main`. Pages redeploys automatically, usually
within a minute.

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

Six tabbed panels, no scrolling on desktop: **Home → About → Why Hire Me →
Projects → Skills → Experience**. Panels swap in place; `#projects`-style deep
links work and so do arrow keys.

The order lives in `ORDER` in the script and must match three other things:
the tab buttons, the panel order in the DOM (which is what print output
follows), and the `data-goto` chain — each panel's closing button points at
the next one. Reordering panels means updating all four, and the last panel
in the walk carries no hand-off button at all.

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
- **Experience and Skills are the tightest panels.** Experience carries nine
  bullets plus the education row and has no header subtitle as a result; it
  only fits because, being last in the walk, it needs no hand-off button.
  Skills carries a full 2x2 card grid plus the standard pill CTA, and every
  spacing value in `.sk` and `.skillgrid` is tuned to buy that pill its ~69px
  — it went 39px over the fold before that pass. Adding content to either
  panel means taking the height back somewhere else.
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
- The dental X-ray pipeline is **YOLOv5**, confirmed. Do not "correct" it to v8.

Bullets follow **XYZ**: did X using tools Y, producing concrete outcome Z.
  Outcomes are specific, not generic percentages.
- **Bold sparingly** — only product names, tool/model names, and hard numbers.
  Bold does nothing for ATS parsing; it's purely a human scanning aid, so heavy
  bolding costs readability and buys nothing.
- No em dashes anywhere. The `--` in date ranges are en dashes, which is correct.

## Open items

1. **`resume.pdf` still calls the product "Castillo Planset QC".** Everywhere
   editable — the site and `resume.tex` — it is now **Engineering Planset QC**,
   in both the Experience bullet and the Projects entry. The PDF was supplied
   already compiled and cannot be edited in place, so its Experience bullet
   still carries the old name. Fix it whenever the PDF is next regenerated,
   but read the item below before doing that.
2. **`resume.tex` is still behind `resume.pdf`.** The PDF was supplied
   already compiled and is a later revision than the LaTeX: it adds an "Areas
   of Specialization" section and a portfolio link, and tightens the summary.
   Those edits exist only in the PDF, so **regenerating `resume.pdf` from
   `resume.tex` today would silently undo them.** Port them into the LaTeX
   before recompiling. (The YOLOv5/YOLOv8 disagreement is settled: YOLOv5 is
   correct and every file now says so.)
3. **BLIP's LLM is unnamed.** Both the site and the resume say "LLM layer" /
   "LLM search" because the specific model was never confirmed. Name it.
4. **Dropped skill keywords.** The site's Skills panel carries the full list,
   but the resume's one-line specializations section still omits scikit-learn,
   XGBoost, Docker, SageMaker, Bedrock, Vertex AI, RAG, React/TypeScript,
   SQLAlchemy, and Entra ID. Consider a compact "Also:" line there if
   keyword-filtered screens are a concern.
5. **Repo names don't match project names.** The site calls them SecondSight and
   RootCause; the GitHub repos are still `Warning-system-for-automobile-drivers`
   and `Dental-X-Ray-Detection`. Renaming the repos would fix the mismatch
   (old URLs auto-redirect).

## Analytics

Off by default. The whole thing hangs off one constant in the script block:

```js
var GOATCOUNTER = '';   // paste your counter URL to switch it on
```

While that is empty **nothing is loaded and no request leaves the page** — the
script tag is injected from JS rather than sitting in `<head>`, precisely so
that a visitor who has opted out, or a site with tracking off, never contacts
the analytics host at all. The footer note stays hidden in that state too, so
the site never claims to collect something it isn't.

To switch it on: sign up at goatcounter.com, then set the constant to
`https://<yourcode>.goatcounter.com/count`. The data lives on GoatCounter's
servers and is read at `https://<yourcode>.goatcounter.com`. Nothing is stored
in this repo.

What it sends:

| Kind | Looks like |
| --- | --- |
| Panel view (pageview) | `/#projects` |
| Furthest panel reached | `depth/4-projects` |
| Resume | `resume/open` |
| Project link | `project/BLIP/Live app` |
| Contact | `contact/LinkedIn` |
| Navigation | `nav/tab`, `nav/cta`, `nav/prevnext`, `nav/arrow-keys` |
| Theme | `theme/dark` |

Things that are load-bearing and easy to break:

- **`no_onload` must stay set.** This is a single-page app; the automatic
  pageview would only ever count the first panel, so `show()` sends them by
  hand. Without `no_onload` the first panel is counted twice.
- **Events are queued.** `count.js` is async and may never arrive at all
  (ad blockers routinely stop it, and this site's audience runs them). The
  queue flushes when it appears and is dropped after ~10s if it does not.
- **Global Privacy Control and Do Not Track are both honoured** before
  anything loads. GPC is legally binding in several US states.
- Tracking is the site's only third-party dependency besides Google Fonts.

## Updating the live site

Edit `index.html`, commit, push to `main`. Pages redeploys automatically, usually
within a minute.

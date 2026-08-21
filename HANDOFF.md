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

Live site: **https://jayasurya23.github.io** (GitHub Pages, publishes from `main`).

## Site structure

Five tabbed panels, no scrolling on desktop: **Home → About → Why Hire Me →
Projects → Skills**. Panels swap in place; `#projects`-style deep links work and
so do arrow keys.

Things worth knowing before editing:

- **Palette** is six CSS variables at the top of the `<style>` block, with four
  alternate palettes commented directly beneath.
- **Sections** are marked with `<!-- ═══ 01 · HOME ═══ -->` style comments.
- **Photo** is a base64 data URI in the `.pfp, .brand__pic` rule. The source is an
  uncropped 3:2 landscape, so `background-size`/`background-position` are tuned to
  frame the face. If you swap in a square-cropped photo, change those to `cover`
  and `center`.
- **Resume button** builds a `blob:` URL from an embedded base64 copy of the PDF.
  This is deliberate: a plain relative link fails in sandboxed viewers, and the
  `download` attribute is blocked on `file://` and in sandboxes.
- **Every panel is sized to fit one viewport.** After editing, check that no panel
  overflows at roughly 1440x820, or the no-scroll design breaks.

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

1. **BLIP's LLM is unnamed.** Both the site and the resume say "LLM layer" /
   "LLM search" because the specific model was never confirmed. Name it.
2. **Resume has not been compiled.** It was written and page-budgeted by hand
   (estimated ~694pt against ~760pt available, roughly 9% headroom) but never
   rendered. Compile it and check it lands on one page.
3. **Dropped skill keywords.** The resume's tool table was replaced with a short
   specializations line. Tools named in bullets survive (PyTorch, OpenCV, YOLOv8,
   Gemini, PySpark, MLflow, CI/CD). These no longer appear anywhere:
   scikit-learn, XGBoost, Docker, SageMaker, Bedrock, Vertex AI, RAG,
   React/TypeScript, SQLAlchemy, Entra ID. Consider a compact "Also:" line if
   keyword-filtered screens are a concern.
4. **Repo names don't match project names.** The site calls them SecondSight and
   RootCause; the GitHub repos are still `Warning-system-for-automobile-drivers`
   and `Dental-X-Ray-Detection`. Renaming the repos would fix the mismatch
   (old URLs auto-redirect).

## Updating the live site

Edit `index.html`, commit, push to `main`. Pages redeploys automatically, usually
within a minute.

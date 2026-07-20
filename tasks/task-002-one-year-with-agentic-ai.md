# One year with agentic AI

- **ID:** task-002
- **Slug:** one-year-with-agentic-ai
- **Status:** active
- **Created:** 2026-07-20
- **Last updated:** 2026-07-20

> Task status values: `active` → `blocked` → `done`. A task owns exactly one post and tracks it from draft to a published, quality-gated article.

## Owned post

- **Section:** genai
- **Post slug:** one-year-with-agentic-ai
- **State:** draft
- **Publication date:** _(empty until promoted)_

*The post lives at `<section>/_drafts/<post-slug>.md` while in `draft` state, and moves to `<section>/_posts/<YYYY-MM-DD>-<post-slug>.md` when promoted to `published` (via the `post-utils` skill). Update **State** to `published` and fill **Publication date** on promotion.*

## Research grounding

*No grounding recorded.*

- **Research subjects:** _(none)_
- **Observations:** _(none)_
- **Bibliography:** _(none)_

## LinkedIn memo

*Optional; created via the `linkedin-memo` skill once the post is published. Not a quality gate.*

- **Memo:** _(none)_

## Quality gates

*Four gates a post must pass before it is fully published. The first three are **pre-promotion** gates — turn them green **before** `post-utils` promotes the draft. `build-clean` is the **post-promotion** gate: Jekyll excludes `_drafts/` by default, so it can only be evaluated on the real `_posts/` file after the `git mv`. Tick each box and record the evidence/date in its note when the gate goes green.*

- [ ] **language-check** — the `english-correction` skill has been run over the post and its corrections applied.
  - _Note: was green on the original combined draft, but reset — the draft was split and reworked on 2026-07-20, so it must be re-run on the retrospective-only content._
- [ ] **adversarial-challenge** — the `adversarial-challenge` skill has been run and its findings addressed.
  - _Note: was green on the original combined draft, but reset — re-run required after the 2026-07-20 split and rework._
- [ ] **frontmatter-and-links** — no scaffold placeholders remain: `title` present and real, `categories`/`description`/`pic` replaced with genuine values (none still carrying the `TODO` marker; `categories` a curated set, not the bare section name), and all links resolve.
  - _Note: NOT green — `pic: /genai/img/TODO.jpg` is still a scaffold placeholder; needs a real hero image._
- [ ] **build-clean** _(post-promotion)_ — `JEKYLL_ENV=production bundle exec jekyll build` succeeds with the promoted post present in `<section>/_posts/`.
  - _Note: not run yet (post still in `_drafts/`)._

## Notes / next actions

Draft at `genai/_drafts/one-year-with-agentic-ai.md` — the retrospective half of the original one-year draft (see plan-007), split off on 2026-07-20 when the remote-control half was published as task-001. Content: a year of agentic coding (vibe coding → plan-and-execute → skills) and a grading of last year's predictions; thesis that automation made the work harder, not easier. Before publishing: (1) replace the `pic` placeholder with a real hero image, (2) re-run language-check and adversarial-challenge on the split content, (3) promote via post-utils, (4) run build-clean. Once published, uncomment the `[one-year]` cross-link in the sibling article (task-001).

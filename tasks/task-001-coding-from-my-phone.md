# Coding from my phone: a remote agentic setup

- **ID:** task-001
- **Slug:** coding-from-my-phone
- **Status:** active
- **Created:** 2026-07-16
- **Last updated:** 2026-07-20 (split from original one-year task; article published)

> Task status values: `active` → `blocked` → `done`. A task owns exactly one post and tracks it from draft to a published, quality-gated article.

## Owned post

- **Section:** genai
- **Post slug:** coding-from-my-phone
- **State:** published
- **Publication date:** 2026-07-20

*The post lives at `<section>/_drafts/<post-slug>.md` while in `draft` state, and moves to `<section>/_posts/<YYYY-MM-DD>-<post-slug>.md` when promoted to `published` (via the `post-utils` skill). Update **State** to `published` and fill **Publication date** on promotion.*

## Research grounding

*No grounding recorded.*

- **Research subjects:** _(none)_
- **Observations:** _(none)_
- **Bibliography:** _(none)_

## LinkedIn memo

*Optional; created via the `linkedin-memo` skill once the post is published. Not a quality gate.*

- **Memo:** [2026-07-20-coding-from-my-phone](../linkedin-memos/2026-07-20-coding-from-my-phone.md)

## Quality gates

*Four gates a post must pass before it is fully published. The first three are **pre-promotion** gates — turn them green **before** `post-utils` promotes the draft. `build-clean` is the **post-promotion** gate: Jekyll excludes `_drafts/` by default, so it can only be evaluated on the real `_posts/` file after the `git mv`. Tick each box and record the evidence/date in its note when the gate goes green.*

- [x] **language-check** — the `english-correction` skill has been run over the post and its corrections applied.
  - _Note: run 2026-07-20 on the split article; prose already British and on-tone, one minor clarity nudge applied_
- [x] **adversarial-challenge** — the `adversarial-challenge` skill has been run and its findings addressed.
  - _Note: run 2026-07-20; verdict holds-with-caveats; Finding 1 (two-kinds-of-questions ordering) applied; Findings 2-3 low-severity, left by design_
- [x] **frontmatter-and-links** — no scaffold placeholders remain: `title` present and real, `categories`/`description`/`pic` replaced with genuine values (none still carrying the `TODO` marker; `categories` a curated set, not the bare section name), and all links resolve.
  - _Note: green 2026-07-20. categories: genai arch; real description; pic: /genai/img/mini-pc.jpg present on disk. The [one-year] cross-link stays commented out until the sibling retrospective (task-002) is published._
- [ ] **build-clean** _(post-promotion)_ — `JEKYLL_ENV=production bundle exec jekyll build` succeeds with the promoted post present in `<section>/_posts/`.
  - _Note: NOT verified locally — Ruby/Bundler unavailable in this environment. Deferred to the deploy workflow's production build on merge to main (PR #7)._

## Notes / next actions

Published 2026-07-20 at `genai/_posts/2026-07-20-coding-from-my-phone.md`. This is the first of two articles split from the original one-year draft (see plan-007). Covers the remote agentic setup: Android phone + Claude Code remote control driving an always-on mini PC 1500 km away, and the full automation (robustness + pre-approved mechanics) it demands. Three pre-promotion gates green; build-clean deferred to CI (PR #7). The retrospective half is now tracked by task-002. Remaining: confirm CI build passes on PR #7; optionally add a LinkedIn memo once merged.

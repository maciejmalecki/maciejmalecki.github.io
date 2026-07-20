<!--
Post-authoring task template. Copied by the `task-utils` skill when a new task is created. Do not edit an actual task by hand from this template — invoke the skill. Fill every `{{placeholder}}`. Delete any bracketed guidance in italics once the section is populated.
-->

# {{Task title — usually the post's working title}}

- **ID:** task-{{NNN}}
- **Slug:** {{task-slug}}
- **Status:** active
- **Created:** {{YYYY-MM-DD}}
- **Last updated:** {{YYYY-MM-DD}}

> Task status values: `active` → `blocked` → `done`. A task owns exactly one post and tracks it from draft to a published, quality-gated article.

## Owned post

- **Section:** {{one of `arch` / `casual` / `cbm` / `genai` / `sh`}}
- **Post slug:** {{post-slug}}
- **State:** draft
- **Publication date:** _(empty until promoted)_

*The post lives at `<section>/_drafts/<post-slug>.md` while in `draft` state, and moves to `<section>/_posts/<YYYY-MM-DD>-<post-slug>.md` when promoted to `published` (via the `post-utils` skill). Update **State** to `published` and fill **Publication date** on promotion.*

## Research grounding

*What this post is grounded in, from the sibling **`../research`** repository (read-only). Three optional link lists — all optional; a post may be grounded in a subject only, or carry **no grounding at all** (leave the lists empty / delete their placeholders). These links are populated and validated via the `task-utils` skill (which invokes `research-access`); the links point into the sibling research repo and are never written back to. Note the `../../` prefix: a task file lives in `tasks/`, so reaching the sibling `research/` repo takes **two** levels up, not one.*

- **Research subjects:** *{{zero or more subject names, e.g. `knowledge-systems` — link each as [`<subject>`](../../research/<subject>/README.md)}}*
- **Observations:** *{{zero or more **subject-qualified** `<subject>/obs-NNNN` references — the subject qualifier is **required** (observation numbering restarts per subject, so a bare `obs-NNNN` is ambiguous) — link each as [`<subject>/obs-NNNN`](../../research/<subject>/observations/obs-NNNN.md)}}*
- **Bibliography:** *{{zero or more citation keys `AuthorYYYY_Tag` — referenced **bare** since they are globally unique — link each as [`AuthorYYYY_Tag`](../../research/bibliography/AuthorYYYY_Tag.md)}}*

## LinkedIn memo

*An optional, ready-to-paste LinkedIn summary + advertisement for this post, ending in a link back to the published article. Optional — most posts carry **no** memo (leave the list empty / delete the placeholder). A memo only exists once the post is **published** (its filename mirrors the post's dated `_posts/` name), and is created and cross-linked here via the `linkedin-memo` skill — not a quality gate. Note the `../` prefix: a task file lives in `tasks/`, and memos live in the sibling `linkedin-memos/` directory, so reaching a memo is **one** level up.*

- **Memo:** *{{zero or one memo file — link it as [`<YYYY-MM-DD>-<post-slug>`](../linkedin-memos/<YYYY-MM-DD>-<post-slug>.md), matching the post's publication date and slug}}*

## Quality gates

*Four gates a post must pass before it is fully published. The first three are **pre-promotion** gates — turn them green **before** `post-utils` promotes the draft. `build-clean` is the **post-promotion** gate: Jekyll excludes `_drafts/` by default, so it can only be evaluated on the real `_posts/` file after the `git mv`. Tick each box and record the evidence/date in its note when the gate goes green.*

- [ ] **language-check** — the `english-correction` skill has been run over the post and its corrections applied.
  - _Note: {{evidence / date, e.g. "run 2026-07-04, corrected in place"}}_
- [ ] **adversarial-challenge** — the `adversarial-challenge` skill has been run and its findings addressed.
  - _Note: {{evidence / date}}_
- [ ] **frontmatter-and-links** — no scaffold placeholders remain: `title` present and real, `categories`/`description`/`pic` replaced with genuine values (none still carrying the `TODO` marker; `categories` a curated set, not the bare section name), and all links resolve.
  - _Note: {{evidence / date}}_
- [ ] **build-clean** _(post-promotion)_ — `JEKYLL_ENV=production bundle exec jekyll build` succeeds with the promoted post present in `<section>/_posts/`.
  - _Note: {{evidence / date}}_

## Notes / next actions

{{Free-form context so switching to this task restores where things stand — what is done, what is blocked, what to do next. Keep it current.}}

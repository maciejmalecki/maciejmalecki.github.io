# Optional LinkedIn summary memos for blog posts

- **ID:** plan-005
- **Slug:** optional-linkedin-summary-memos
- **Status:** implemented
- **Created:** 2026-07-04
- **Last updated:** 2026-07-05

> Status values: `draft` → `accepted` → `in-progress` → `implemented`. A plan may be moved back from `implemented` to `in-progress` only when the human explicitly asks for changes to a historical plan.

## Summary

Add an optional, per-post **LinkedIn memo** — a short standalone Markdown file holding a LinkedIn-ready summary and advertisement blurb for a blog article, ending in a link back to the published post on `maciejmalecki.github.io`. Memos live in a new top-level `linkedin-memos/` directory that is **excluded from the Jekyll build** (they are authoring artifacts, never site content). Each memo is **cross-linked to its owning post via the post's task**: a new optional *LinkedIn memo* section in the task template records the memo path, and the memo's own front-matter points back to the post's permalink. A dedicated **`linkedin-memo` skill** (Haiku-delegated, mirroring `post-utils`) scaffolds a memo from a template and records the cross-link on the task; creation is entirely opt-in, so most posts will never have a memo.

## Rationale

For a subset of posts the author wants to promote them on LinkedIn with a standardised summary + call-to-action rather than composing one ad hoc each time. Requirements the author stated: the summary is an **optional add-on** (only some posts get one), it is a **standalone Markdown file** in a **dedicated `linkedin-memos/` folder**, and it is **cross-linked to the blog article via the task**. The blog's public base URL is `https://maciejmalecki.github.io/`, and with the `permalink: /:title` config a post is reachable at `https://maciejmalecki.github.io/<post-slug>` — the memo's back-link target.

This plan deliberately reuses the repository's established patterns instead of inventing new ones: memos mirror the `post-utils` file-lifecycle model (a template plus a Haiku-delegated skill), and the task cross-link mirrors the existing **Research grounding** section on the task template (an optional link block, populated by a skill, validated during task reconciliation). Alternatives considered and rejected: (a) storing the summary inline in post front-matter — rejected because it is not site content and would couple ad copy to the published article; (b) relying on Jekyll's default ignoring of front-matter-less files instead of an explicit `exclude` — rejected as too implicit and fragile (the memos **do** carry front-matter, so they must be excluded explicitly); (c) folding memo management into `task-utils` — rejected to keep `task-utils` focused on the post lifecycle and gates, matching how `post-utils` is a separate file-lifecycle skill.

Decisions confirmed with the author up front: `linkedin-memos/` at the **repo root, build-excluded**; cross-link via a **new task-template section**; memos created and managed by a **new dedicated skill**.

## Open Questions

*All questions in this section must be answered by a human before the plan can move from `draft` to `accepted`. The `plan` skill will not promote status while unanswered questions remain.*

- [x] **Q1.** Memo filename convention: should a memo be named purely for its post slug (`linkedin-memos/<post-slug>.md`), matching the draft/post slug and giving a one-to-one, discoverable mapping? Or is a dated prefix wanted?
  - **Answer:** Use a **dated prefix consistent with the published article** — `linkedin-memos/<YYYY-MM-DD>-<post-slug>.md`, matching the post's `_posts/` filename (same publication date and slug). This reinforces the one-to-one mapping and aligns with Q3 (a memo only exists once the post is published, so its publication date is known).
- [x] **Q2.** Memo front-matter and body contract: what fields should the template carry (proposed: `post` = back-link permalink, `post_task` = owning task file, `created` date), and what body sections (proposed: a ~1–3 sentence **summary**, a short **advertisement / call-to-action** blurb, and an explicit **post URL** line `https://maciejmalecki.github.io/<post-slug>`)? Any LinkedIn-specific conventions to bake in (hashtags block, character-count guidance)?
  - **Answer:** **Front-matter:** `post` (back-link permalink `https://maciejmalecki.github.io/<post-slug>`), `post_task` (owning task file), `created` (date). **Body:** a **single ready-to-paste block** that *is* the LinkedIn post — summary sentences, a call-to-action line, the `Read more:` post URL, and a hashtags line, flowing together for one-shot copy-paste — wrapped in an HTML comment marking it as the paste block. Bake in **character-count guidance** as an HTML comment: LinkedIn's ~3000-character limit and the ~140-character "see more" fold, so the author writes to the medium.
- [x] **Q3.** Back-link timing: a memo's permalink target only exists once the post is **published** (the slug is stable from draft, but the live URL only resolves post-promotion). Should the skill allow scaffolding a memo against a still-draft post (writing the prospective URL from the known slug), or refuse until the post is promoted?
  - **Answer:** **Require the post to be published** — the `linkedin-memo` skill refuses to scaffold a memo for a post whose owning task is still in `draft` state; the post must be promoted (a `_posts/<YYYY-MM-DD>-<slug>.md` file exists and the task records `published` + a publication date) first. This also supplies the dated filename prefix from Q1.
- [x] **Q4.** Task-side validation: should the memo cross-link participate in the task **reconciliation / orphan check** (flag a task whose referenced memo file has vanished, and/or a memo whose owning post no longer exists), the way research-grounding links are validated? If so, this extends `task-utils`'s reconciliation; if not, the link is recorded but never validated.
  - **Answer:** **Yes, validate it.** Extend `task-utils`'s reconciliation / orphan check so a task whose linked memo file has vanished is flagged, exactly as a vanished draft/post or an unresolved research-grounding link is. It remains **not a quality gate** — validation only, no gate.

## Steps

*Each step is a self-contained, reviewable unit of work. Steps are numbered and executed in order unless the executor is explicitly asked to run a subset. Per-step execution status values: `pending`, `in-progress`, `done`, `skipped`, `blocked`.*

### Step 1 — Create the `linkedin-memos/` directory and exclude it from the build

- **Execution status:** done
- **Intent:** Establish the dedicated memo folder and guarantee Jekyll never publishes memos. Add `linkedin-memos` to `_config.yml`'s `exclude` list (adding an `exclude:` key if none exists), so the folder is omitted from `_site/` even though its files carry front-matter. Seed the folder with a `README.md` explaining what it holds and that it is authoring-only, so the directory is tracked by git and self-documenting.
- **Touches:** `_config.yml` (add/extend `exclude:`); new `linkedin-memos/README.md`.
- **Acceptance:** `JEKYLL_ENV=production bundle exec jekyll build` succeeds and `_site/` contains **no** `linkedin-memos` output; the folder and its README are committed.
- **Execution summary:** Added an `exclude:` key to `_config.yml` (there was none). Crucial correctness fix discovered during execution: an explicit `exclude` **replaces** Jekyll's built-in default exclude list, so the defaults (`.sass-cache`, `.jekyll-cache`, `gemfiles`, `Gemfile`, `Gemfile.lock`, `node_modules`, `vendor/…`) are restated alongside `linkedin-memos` — otherwise `Gemfile`/`Gemfile.lock` (static, front-matter-less files) would start being copied into `_site/`. Created `linkedin-memos/README.md` documenting the folder as authoring-only. **Build not run:** the Ruby/Bundler/Jekyll toolchain is not installed in this execution environment (`bundle`/`ruby` absent from PATH), so the acceptance build could not be executed here; correctness was verified by reasoning about Jekyll's exclude semantics and the source-root file inventory. **Follow-up (out of scope, pre-existing):** other authoring-only roots — `plans/`, `tasks/`, `meta/`, `templates/`, `docs/`, `CLAUDE.md`, `README.md`, `Rakefile` — are copied into `_site/` today and remain so; they were never in Jekyll's default exclude, so this change is scope-neutral for them. Whether to also exclude those is a separate cleanup decision, not part of plan-005. The final `build-clean` verification is deferred to a machine with the Jekyll toolchain (see Step 6 note).

### Step 2 — Add the LinkedIn-memo template

- **Execution status:** done
- **Intent:** Provide `templates/linkedin-memo-template.md` as the single source the skill copies, mirroring how `templates/task-template.md` and `templates/plan-template.md` work. Per Q2: front-matter `post` (back-link permalink), `post_task` (owning task file), `created` (date); body is a **single ready-to-paste block** wrapped in an HTML comment marking it as the paste target — summary sentences, a CTA line, a `Read more:` post-URL line, and a hashtags line flowing together for one-shot copy-paste — preceded by an HTML comment giving LinkedIn character-count guidance (~3000-char limit, ~140-char "see more" fold). Include the same "do not hand-edit; invoke the skill" header comment the other templates carry.
- **Touches:** new `templates/linkedin-memo-template.md`.
- **Acceptance:** Template exists with every field/section a scaffolded memo needs, all placeholders clearly marked; resolves the fields named in Q2.
- **Execution summary:** Created `templates/linkedin-memo-template.md` with the standard "do not hand-edit; invoke the skill" header comment. Front-matter per Q2: `post` (back-link permalink `https://maciejmalecki.github.io/<post-slug>`), `post_task` (owning task, linked `../tasks/task-NNN-slug.md` — `../` because a memo lives in `linkedin-memos/`, a sibling of `tasks/`), and `created` date. Body is a single ready-to-paste block delimited by explicit `PASTE BEGIN`/`PASTE END` HTML-comment markers — opening hook, 1–3 summary sentences, a CTA line, a `Read more:` post-URL line, and a hashtags line — preceded by an HTML comment giving LinkedIn char-count guidance (~3000-char limit, ~140-char "see more" fold). All authorial content is `{{placeholder}}`-marked. Post-review hardening (high-effort code-review): replaced the `{{#hashtags …}}` placeholder with a non-Liquid `[hashtags — …]` marker (a `{{#…}}` token would throw a Liquid build error if the file were ever processed) and strengthened the header comment to state that the `_config.yml` `exclude` entry is the sole guard against publishing memo ad copy — do not remove it.

### Step 3 — Add the optional *LinkedIn memo* section to the task template

- **Execution status:** done
- **Intent:** Extend `templates/task-template.md` with a new optional **LinkedIn memo** section — a single link to the memo file at `../linkedin-memos/<YYYY-MM-DD>-<post-slug>.md` (relative to `tasks/`; dated prefix per Q1), explicitly marked optional and absent for posts without a memo, mirroring the wording/shape of the existing **Research grounding** section. This is where the memo is cross-linked to the article via the task.
- **Touches:** `templates/task-template.md`.
- **Acceptance:** New section present, clearly optional, with a correctly-relative link placeholder; existing sections and gates unchanged.
- **Execution summary:** Inserted a new `## LinkedIn memo` section between **Research grounding** and **Quality gates**, mirroring the optional-link wording of Research grounding. It carries a single **Memo** link placeholder to `../linkedin-memos/<YYYY-MM-DD>-<post-slug>.md` — the `../` (one level up) is correct because memos live in `linkedin-memos/`, a sibling of `tasks/`, unlike research grounding's `../../` two-levels-up into the separate `../research` repo. The italic note states the memo is optional, exists only once the post is published, is populated via the `linkedin-memo` skill, and is **not a quality gate**. Existing sections and the four gates are unchanged.

### Step 4 — Add the `linkedin-memo` skill

- **Execution status:** done
- **Intent:** Add `.claude/skills/linkedin-memo/SKILL.md`, a Haiku-delegated skill mirroring `post-utils`' execution model. The **main agent decides** (which post/task, the back-link URL from the post slug, the summary/CTA text if supplied); a **Haiku subagent** does the mechanical file work — copying the template to `linkedin-memos/<YYYY-MM-DD>-<post-slug>.md` (dated prefix per Q1) and filling front-matter — and reports back. Operations: **scaffold** a memo for a given post — **refuse unless the post is published** (owning task in `published` state with a publication date and the `_posts/<date>-<slug>.md` file present), per Q3, and refuse if a memo already exists — and **cross-link** it on the owning task (delegating the task-file edit, respecting `task-utils`'s ownership of task files — i.e. the skill writes the memo but hands the task-section edit through the same Haiku-subagent discipline). The dated filename prefix and the back-link permalink both derive from the post's known publication date and slug. State the boundaries with `post-utils` (owns post files), `task-utils` (owns task state/gates/reconciliation), and this skill (owns memo files).
- **Touches:** new `.claude/skills/linkedin-memo/SKILL.md`.
- **Acceptance:** Skill file is well-formed (name/description/allowed-tools front-matter), documents scaffold + cross-link operations and guardrails (never overwrite an existing memo, never publish memos, delegate file work to Haiku), and is consistent with `post-utils`/`task-utils` conventions.
- **Execution summary:** Created `.claude/skills/linkedin-memo/SKILL.md` with `name`/`description`/`allowed-tools: Agent Bash Read Grep Glob` front-matter matching `post-utils`. Documents the Haiku-delegated execution model, the **published-post precondition** (Q3: refuse for a draft post — owning task must record `published` + a publication date and the `_posts/<date>-<slug>.md` file must exist), the memo path rule `linkedin-memos/<YYYY-MM-DD>-<post-slug>.md` (Q1) and the back-link `https://maciejmalecki.github.io/<post-slug>` (noting `permalink: /:title` means the date is in the filename, not the URL), two operations (scaffold from `templates/linkedin-memo-template.md`; cross-link on the owning task's LinkedIn-memo section via `../linkedin-memos/…`), and guardrails (published-only, never overwrite, never publish memos, don't touch post files or task gates/state, delegate to Haiku). Boundaries with `post-utils` and `task-utils` are stated explicitly and non-overlapping.

### Step 5 — Wire memo cross-link into `task-utils` (record + optional validation)

- **Execution status:** done
- **Intent:** Update `.claude/skills/task-utils/SKILL.md` so the task board is aware of the memo cross-link: document that a task may carry a **LinkedIn memo** link (recorded by the `linkedin-memo` skill), state the boundary that `task-utils` records but does not create memo files, and — per Q4 — extend the reconciliation / orphan check to flag a task whose linked memo file has vanished, exactly as a vanished draft/post or unresolved research-grounding link is flagged. Keep memos **not a quality gate** (exactly as research grounding is optional and not a gate).
- **Touches:** `.claude/skills/task-utils/SKILL.md` (and a note in `CLAUDE.md` if the memo concept warrants a mention in the "Plans & meta" / authoring overview).
- **Acceptance:** `task-utils` documents the memo link and its (optional) validation without adding a gate; boundaries with `linkedin-memo` are explicit and non-overlapping.
- **Execution summary:** Edited `.claude/skills/task-utils/SKILL.md`: (1) added a `linkedin-memo` bullet to the **Boundaries** block; (2) added a new **§6 LinkedIn memo (optional cross-link)** — memo is optional and *not* a gate, `task-utils` records-but-does-not-create it, and reconciliation flags a memo file that no longer exists on disk (`linkedin-memos/<YYYY-MM-DD>-<post-slug>.md`); (3) renumbered the former §6 promotion-gating section to **§7**; (4) extended the §2 orphan-check bullet to mention memo-link validation; (5) added a guardrail mirroring the research-grounding one. Memos stay out of the four-gate model entirely. The `CLAUDE.md` note is handled in Step 6 (docs) to keep documentation edits in one place.

### Step 6 — Update documentation and meta changelog

- **Execution status:** done
- **Intent:** Reflect the new capability in `CLAUDE.md` (a sentence in the authoring overview: some posts may carry an optional build-excluded LinkedIn memo, created via the `linkedin-memo` skill and cross-linked via the task) and log the Claude-asset changes (new skill, template, task-template and task-utils edits, CLAUDE.md edit) as a MET-nnnn entry via the `claude-meta-changelog` skill.
- **Touches:** `CLAUDE.md`; `meta/` (new MET-nnnn entry).
- **Acceptance:** `CLAUDE.md` mentions the memo capability; a MET entry records the asset changes; `plans/README.md` row for plan-005 reflects final status.
- **Execution summary:** Extended the post-authoring paragraph in `CLAUDE.md` with a sentence describing the optional LinkedIn memo (published-post-only, ready-to-paste summary + advertisement + live back-link, created via `linkedin-memo` under the build-excluded `linkedin-memos/`, cross-linked and validated via the task, not a quality gate). Logged the Claude-asset changes as **MET-0019** (`meta/MET-0019_add-linkedin-memo.md`) via the `claude-meta-changelog` skill and added its `meta/README.md` index row; the entry lists the new skill, the `task-utils` and `CLAUDE.md` edits, and — for context — the out-of-changelog-scope template/config/folder changes. `plans/README.md` status is set to `implemented` alongside this run. **Note:** the `build-clean`-style verification of the `_config.yml` exclude (Step 1) remains deferred — no Ruby/Jekyll toolchain in this environment — and should be confirmed by a production `jekyll build` on a machine that has it, or by CI on push.

## Change Log

- 2026-07-04 — Plan created (status: draft).
- 2026-07-05 — Answered Q1–Q4 (dated memo filename; front-matter + single ready-to-paste body block with char-count guidance; memo requires the post to be published; task-side memo-link validation). Updated steps 2–5 accordingly.
- 2026-07-05 — Status draft → accepted (all open questions answered).
- 2026-07-05 — Status accepted → in-progress (execution started).
- 2026-07-05 — Plan implemented. (build-clean verification of the _config.yml exclude deferred — no Jekyll toolchain in the execution environment; confirm via CI/production build.)

# Relocate Jekyll blog source into -src and publish built site via GitHub Actions

- **ID:** plan-001
- **Slug:** relocate-jekyll-source-and-ci-publish
- **Status:** implemented
- **Created:** 2026-07-03
- **Last updated:** 2026-07-03

> Status values: `draft` → `accepted` → `in-progress` → `implemented`. A plan may be moved back from `implemented` to `in-progress` only when the human explicitly asks for changes to a historical plan.

## Summary

This plan repurposes `maciejmalecki.github.io-src` from an empty scaffold into the authoring home for the Jekyll blog. The complete Jekyll source currently living in the sibling published repo `../maciejmalecki.github.io` (`main` branch) is **copied** (never moved) into this repo so that all future authoring — posts, drafts, layouts, config — happens here. A GitHub Actions workflow in this repo then builds the site with Jekyll and pushes **only the built output** (`_site`) to the published repo's `gh-pages` branch, which GitHub Pages already serves.

The published repo's `main` branch is left untouched by this plan; it remains as a stale copy of the source that can be cleaned up in a later, separate effort. The net effect: author in `-src`, publish to `../maciejmalecki.github.io@gh-pages`.

## Rationale

Today the published repo does double duty — it holds the Jekyll source on `main` and the built site on `gh-pages`, with CircleCI (`.circleci/config.yml` + `setup-github.sh`) building `main` and force-pushing `_site` to `gh-pages`. The user wants a clean separation: a dedicated source repo (`-src`) for authoring, and the published repo reduced (eventually) to a pure Pages output.

Constraints and decisions established up front:

- **Copy, not move.** Source files are copied out of `../maciejmalecki.github.io`; that repo is not modified by this plan. This keeps the existing site live and provides a rollback path if the new pipeline misbehaves.
- **Publish flow: GitHub Actions in `-src`** (chosen over local Rake deploy or porting CircleCI). CI builds on push to `main` and deploys `_site` to the published repo's `gh-pages` via a secret credential (deploy key or PAT). This keeps the published repo a pure output and makes publishing fully automated.
- **Old `main` left untouched.** Lowest-risk option; avoids destructive changes to the published repo. A follow-up plan can reduce the published repo to `gh-pages`-only and point Pages at it explicitly.

The existing CircleCI pipeline in the published repo is not disabled by this plan (that repo is untouched). Because `main` there will no longer receive new commits once authoring moves here, CircleCI will simply stop firing — but this is worth calling out as an open question to avoid two pipelines racing on `gh-pages` if the old `main` is ever pushed to.

## Open Questions

*All questions in this section must be answered by a human before the plan can move from `draft` to `accepted`. The `plan` skill will not promote status while unanswered questions remain.*

- [x] **Q1.** What credential should the GitHub Actions workflow use to push to `../maciejmalecki.github.io`'s `gh-pages` branch — a **deploy key** (SSH key whose public half is a write-enabled deploy key on the published repo, private half stored as a secret in `-src`), or a **fine-grained Personal Access Token** with `contents: write` on the published repo stored as a secret in `-src`? (Recommendation: deploy key — scoped to exactly one repo, no account-wide blast radius.)
  - **Answer:** Deploy key (SSH). Generate an SSH keypair; register the public half as a write-enabled deploy key on `maciejmalecki.github.io`, store the private half as a secret in `-src`. Scoped to exactly one repo, no account-wide blast radius, no expiry to manage.
- [x] **Q2.** Should the CircleCI pipeline in the published repo be explicitly retired as part of this work (e.g. a note/PR to delete `.circleci/` on that repo's `main`), or left in place on the untouched `main`? Leaving it is harmless as long as nothing pushes to that `main`, but two systems can write `gh-pages`. (Recommendation: leave it now, retire it in the same follow-up that reduces `main`.)
  - **Answer:** Leave it in place. The published repo stays untouched; CircleCI is dormant as long as nothing pushes its `main`. Retire it later in the same follow-up that reduces `main` to `gh-pages`-only.
- [x] **Q3.** Ruby version for the build. The published repo's CircleCI used `ruby:3.2.3`. Pin the GitHub Actions build to the same, or bump? (Recommendation: match `3.2.3` first to reproduce the current build exactly, bump later if desired.)
  - **Answer:** Match `3.2.3` exactly to reproduce the current working build. Bump later as a deliberate separate change if desired.
- [x] **Q4.** Confirm the deploy should trigger on push to `main` in `-src` (matching the old CircleCI trigger), rather than on tags or manual dispatch. (Recommendation: push to `main`, plus `workflow_dispatch` for manual re-runs.)
  - **Answer:** Trigger on push to `main` plus `workflow_dispatch` for manual re-runs (retry a transient failure or re-deploy after rotating the deploy key without an empty commit).

## Steps

*Each step is a self-contained, reviewable unit of work. Steps are numbered and executed in order unless the executor is explicitly asked to run a subset. Per-step execution status values: `pending`, `in-progress`, `done`, `skipped`, `blocked`.*

### Step 1 — Copy the Jekyll source tree into -src

- **Execution status:** done
- **Intent:** Bring the full Jekyll source from `../maciejmalecki.github.io` (`main`) into this repo so authoring can happen here. Copy every tracked source file and directory: `_config.yml`, `_layouts/`, `_includes/`, `_sass/`, `css/`, `js/`, `img/`, top-level pages (`index.html`, `about.md`, `404.md`, `*.html`, `atom.xml`, `license.md`, `privacy.html`), the section content trees (`arch/`, `casual/`, `cbm/`, `genai/`, `sh/`, `jsd/`, `jse/`, `fritzing/` and their `_posts/`, `_drafts/`, `img/`, `diagrams/` subdirs), `Gemfile`, `Gemfile.lock`, and `Rakefile`. **Copy only** — do not modify the source repo.
- **Touches:** New files throughout this repo's root (the Jekyll tree). Does **not** touch `../maciejmalecki.github.io`.
- **Acceptance:** `git -C ../maciejmalecki.github.io status` is clean (source repo untouched). All source files present in `-src`. `bundle exec jekyll build` succeeds locally against the copied tree (or is confirmed to build in CI at Step 3).
- **Execution summary:** Extracted the source repo's tracked tree via `git -C ../maciejmalecki.github.io archive main` into the scratchpad, then copied every top-level entry into `-src` **except** `.circleci/` (superseded by Step 3's GitHub Actions workflow), `LICENSE`, and `README.md` (both already present in `-src`; README is rewritten in Step 4). 33 new top-level entries landed, including the full Jekyll tree: `_config.yml`, `_layouts/`, `_includes/`, `_sass/`, `css/`, `js/`, `img/`, `Gemfile`, `Gemfile.lock`, `Rakefile`, `.gitignore`, `.vscode/`, all top-level pages, and the section content trees (`arch/`, `casual/`, `cbm/`, `genai/`, `sh/`, `jsd/`, `jse/`, `fritzing/`). Verified all 126 source-tracked files minus the 3 excluded are present in `-src` (0 missing). Confirmed `git -C ../maciejmalecki.github.io status` is clean — source repo untouched. Local `bundle exec jekyll build` was **not** run: no Ruby/bundler toolchain is installed on this machine (`ruby: command not found`); per the acceptance criteria this defers build verification to CI at Step 3.

### Step 2 — Reconcile config, ignores, and CI artifacts for the new home

- **Execution status:** done
- **Intent:** Adapt the copied files to the new repo. Merge the copied `.gitignore` (`_site`, `.sass-cache`, `.jekyll-metadata`, `.jekyll-cache`, `.idea/`) with anything already here. Do **not** copy the published repo's `.circleci/` into `-src` (it is superseded by Step 3). Review `_config.yml`: `url`/`baseurl`/`repository` still describe the *published* site (`maciejmalecki.github.io`) and remain correct since that is where the site is served — confirm no change needed. Decide the fate of the copied `Rakefile` deploy task (kept for local use, or trimmed) per plan discussion.
- **Touches:** `.gitignore`, possibly `_config.yml`, `Rakefile` in `-src`.
- **Acceptance:** No CircleCI config lands in `-src`. `.gitignore` excludes build artifacts. `_config.yml` reviewed and its publish URL confirmed correct for the published site.
- **Execution summary:** **`.gitignore`** — the copied file already contained exactly the specified excludes (`_site`, `.sass-cache`, `.jekyll-metadata`, `.jekyll-cache`, `.idea/`) and `-src` had no prior `.gitignore`, so no merge was necessary; left as-is. **`.circleci/`** — confirmed absent from `-src` (`ls .circleci` → No such file; it was deliberately excluded during the Step 1 copy). **`_config.yml`** — reviewed; `url: https://maciejmalecki.github.io/`, `baseurl: /`, and `repository: maciejmalecki/maciejmalecki.github.io` all correctly describe the *published* site where Pages serves from, so no change made. **`Rakefile`** — trimmed the deploy path per the plan's decision to publish via GitHub Actions rather than local Rake: removed the legacy Travis-based `site:deploy` task, the `check_destination` helper (which cloned the published repo via `GH_TOKEN`), the `SOURCE_BRANCH`/`DESTINATION_BRANCH` constants, and the now-dead `CONFIG`/`USERNAME`/`REPO` reads plus the `yaml` require. Kept the authoring/local tasks `post:create`, `page:create`, `site:build`, `site:serve`, `site:watch` (and dropped the `check_destination` call from build/serve so they run without a published-repo clone). Added a NOTE comment pointing to `.github/workflows/deploy.yml` as the deploy mechanism. Rakefile Ruby syntax was not machine-verified (no Ruby toolchain on this machine); edits were whole-block removals leaving balanced structure.

### Step 3 — Add GitHub Actions workflow to build and publish _site to gh-pages

- **Execution status:** done
- **Intent:** Create `.github/workflows/deploy.yml` in `-src` that, on push to `main` (and `workflow_dispatch`), checks out the source, sets up Ruby (version per Q3), runs `bundle install` and `JEKYLL_ENV=production bundle exec jekyll build`, then pushes the contents of `_site/` to the `gh-pages` branch of `../maciejmalecki.github.io` (i.e. `github.com/maciejmalecki/maciejmalecki.github.io`) using the credential chosen in Q1. Behaviour should mirror the old `setup-github.sh`: replace the branch contents with the fresh build and force-push, carrying the source commit message.
- **Touches:** `.github/workflows/deploy.yml` (new), CI secret configured in the `-src` repo settings (deploy key or PAT per Q1 — configured out-of-band, referenced by the workflow).
- **Acceptance:** Workflow is syntactically valid; a test run (manual `workflow_dispatch`) builds the site and updates the published repo's `gh-pages` with the new `_site` content, and the live Pages site reflects it. The published repo's `main` is unchanged.
- **Execution summary:** Created `.github/workflows/deploy.yml`. Triggers: `push` to `main` plus `workflow_dispatch` (Q4). Steps: `actions/checkout@v4`; `ruby/setup-ruby@v1` pinned to `3.2.3` (Q3) with `bundler-cache: true` (does `bundle install` + gem caching keyed on `Gemfile.lock`); build via `JEKYLL_ENV=production bundle exec jekyll build`; deploy via `peaceiris/actions-gh-pages@v4` using `deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}` (SSH deploy key, Q1), `external_repository: maciejmalecki/maciejmalecki.github.io`, `publish_branch: gh-pages`, `publish_dir: ./_site`, `force_orphan: true` (single fresh commit replacing prior gh-pages contents, mirroring the old `setup-github.sh` wipe-and-force-push), and `full_commit_message` carrying `github.event.head_commit.message` with a `Manual deploy of <sha>` fallback for `workflow_dispatch` runs. Added a `concurrency` group (`deploy-gh-pages`, no cancel) so overlapping runs don't race on the published branch, and a header comment documenting the deploy-key setup. Confirmed `-src` has an `origin` remote at `git@github.com:maciejmalecki/maciejmalecki.github.io-src.git` and the published repo already has an `origin/gh-pages` branch. **Deferred (not blocking the artifact):** the live `workflow_dispatch` test run in the acceptance criteria was NOT performed — it requires (a) the `ACTIONS_DEPLOY_KEY` secret + write-enabled deploy key configured out-of-band (your task, per the execution decision), and (b) committing/pushing this workflow to `-src`'s `main` on GitHub, which the skill does not do unprompted. YAML was not machine-validated (no ruby/python-yaml/node/actionlint on this machine); structure was reviewed manually and conforms to the GitHub Actions schema. **Follow-up for the user:** register the deploy key + secret, push `-src`, then run the workflow via `workflow_dispatch` and confirm the live Pages site updates and the published repo's `main` is unchanged.

### Step 4 — Update repo documentation (CLAUDE.md, README) for the new workflow

- **Execution status:** done
- **Intent:** Replace the "newly initialized / stack not chosen" narrative in `CLAUDE.md` with the real architecture: Jekyll source authored here, build/dev/serve commands (`bundle exec jekyll serve`, `rake post:create`, etc.), and the deployment flow (push to `main` → GitHub Actions → published repo `gh-pages` → Pages). Update `README.md` accordingly. Note the copy-not-move relationship and that the published repo's `main` is a stale source copy pending future cleanup.
- **Touches:** `CLAUDE.md`, `README.md`.
- **Acceptance:** `CLAUDE.md` documents build/dev/test commands and the deploy flow accurately; no stale "no scaffolding exists" text remains.
- **Execution summary:** Rewrote `CLAUDE.md`: removed the stale "newly initialized / no scaffolding / stack not chosen / TODO for future sessions" sections (verified none remain via grep) and replaced them with real content — Purpose (Jekyll blog authored here, built output published to the separate repo's `gh-pages`, copy-not-move relationship, dormant published-repo CircleCI); Build/dev/serve commands (`bundle install`, `bundle exec jekyll serve`/`build`, production build, and the retained `rake post:create`/`page:create`/`site:*` tasks; noted there is no test suite); Architecture (Jekyll/kramdown, plugins, layouts/includes/sass, the topic content sections `arch`/`casual`/`cbm`/`genai`/`sh` each with `_posts`/`_drafts`/`img`, and the `_config.yml` publish-URL rationale); Deployment flow (push to `main` → GitHub Actions → force-push `_site` to published `gh-pages` via `ACTIONS_DEPLOY_KEY`, plus one-time deploy-key setup); and a Plans & meta pointer. Rewrote `README.md` from a bare title into a contributor-facing overview: local dev quickstart, Rake helpers, structure, and the deployment summary, including the copy-not-move note. Content section list and `_posts`/`_drafts` layout were verified against the actual tree before documenting.

## Change Log

- 2026-07-03 — Plan created (status: draft).
- 2026-07-03 — All open questions (Q1–Q4) answered; status promoted to `accepted`.
- 2026-07-03 — Execution started; status promoted to `in-progress`.
- 2026-07-03 — Plan implemented. (Live `workflow_dispatch` deploy verification in Step 3 deferred pending out-of-band deploy-key/secret setup and a push to `-src`.)

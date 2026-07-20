# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This repository is the **authoring source** for the `maciejmalecki.github.io` GitHub Pages site — a Jekyll blog ("()=>{} | Void's blog"). All content and templating is edited here; only the *built* output is published, to the `gh-pages` branch of the separate published repo `maciejmalecki/maciejmalecki.github.io`, which GitHub Pages serves.

The Jekyll source was **copied** (not moved) from the published repo's `main` branch into this repo. The published repo's `main` is therefore a now-stale copy of the source, left untouched pending a future cleanup effort; its dormant CircleCI pipeline (`.circleci/`) is not used by this repo.

This file owns the **build and architecture** layer. The **editorial** layer — house style, tone, front-matter contract, structure, and voice for posts — lives in [`EDITORIAL.md`](EDITORIAL.md); read it before drafting or revising a post. The `english-correction` and `adversarial-challenge` skills enforce it (voice/British-English, and the post contract, respectively).

## Build / dev / serve commands

This is a standard Jekyll site managed with Bundler. Ruby is pinned to **3.2.3** (matched by CI).

- `bundle install` — install gem dependencies (from `Gemfile` / `Gemfile.lock`).
- `bundle exec jekyll serve` — build and serve locally with live reload at `http://localhost:4000`.
- `bundle exec jekyll build` — one-off build into `_site/` (git-ignored).
- `JEKYLL_ENV=production bundle exec jekyll build` — production build, as CI runs it.

A `Rakefile` provides authoring conveniences (thin wrappers around the above plus content scaffolding):

- `rake post:create title="My Post" [date=YYYY-MM-DD]` — create a new post under `_posts/`.
- `rake page:create title="My Page" [folder=<dir>]` — create a new page.
- `rake site:build` / `rake site:serve` / `rake site:watch` — build / serve / serve-and-watch.

For post authoring, prefer the section-aware `post-utils` skill (`.claude/skills/`) over `rake post:create`: it scaffolds a draft into the chosen content section's `_drafts/` with a complete Jekyll front-matter block and promotes it (via `git mv`) into that section's `_posts/` with a dated filename. The `task-utils` skill maintains a task board under `tasks/` so several in-flight posts can be tracked at once (see "Plans & meta"). Posts can be **grounded in the sibling research repository** at `../research` (read-only): the `research-access` skill browses and collates that repo's research subjects, observations, and bibliography as grounding for a draft, and `task-utils` records which grounding a post uses on its task (and validates those links). A **published** post may also carry an optional **LinkedIn memo** — a ready-to-paste LinkedIn summary + advertisement ending in a link back to the live article — created by the `linkedin-memo` skill under the build-excluded `linkedin-memos/` directory and cross-linked to the post via its task (`task-utils` records and validates that link; it is not a quality gate).

There is no test suite. "Verification" is building the site cleanly and viewing it via `jekyll serve`.

## Architecture

- **Generator:** Jekyll (kramdown Markdown), configured in `_config.yml`. Plugins: `jekyll-paginate`, `jekyll-sitemap`. Permalink style `/:title`; pagination 8 per page.
- **Templating:** `_layouts/` (page/post templates), `_includes/` (partials), `_sass/` + `css/` (styles), `js/`, `img/` (shared assets).
- **Content sections:** the blog is split into topic sections, each a top-level directory with its own `_posts/`, `_drafts/`, and often `img/`: `arch/`, `casual/`, `cbm/`, `genai/`, `sh/`. Additional content lives under `fritzing/`, `jsd`, `jse`, and top-level pages (`index.html`, `about.md`, `404.md`, `atom.xml`, `privacy.html`, `license.md`, and the section landing pages `*.html`).
- **Config note:** `_config.yml`'s `url` (`https://maciejmalecki.github.io/`), `baseurl` (`/`), and `repository` (`maciejmalecki/maciejmalecki.github.io`) intentionally describe the **published** site, since that is where the built output is served from — do not point these at the `-src` repo.

## Deployment flow

Publishing is fully automated via GitHub Actions — see `.github/workflows/deploy.yml`.

1. Push to `main` in this `-src` repo (or trigger the workflow manually via `workflow_dispatch`).
2. The workflow checks out the source, sets up Ruby 3.2.3, `bundle install`s, and runs `JEKYLL_ENV=production bundle exec jekyll build`.
3. It publishes the contents of `_site/` to the **`gh-pages` branch of `maciejmalecki/maciejmalecki.github.io`** (a force-pushed single commit that replaces prior contents, carrying the source commit message), using an SSH deploy key stored as the `ACTIONS_DEPLOY_KEY` secret in this repo.
4. GitHub Pages serves that `gh-pages` branch as the live site.

**Deploy-key setup (one-time, out-of-band):** generate an SSH keypair; register the **public** half as a write-enabled deploy key on `maciejmalecki/maciejmalecki.github.io`; store the **private** half as the `ACTIONS_DEPLOY_KEY` secret in this `-src` repo's Actions secrets.

## Plans & meta

Repository changes are driven by plan documents in `plans/` (see the `plan` and `execute-plan` skills under `.claude/skills/`). Post-authoring work is driven by task documents in `tasks/` (see the `task-utils` skill), much as `plans/` is driven by `plan`/`execute-plan`; each task tracks one post's draft-vs-published state, its four quality gates, and optionally its **research grounding** — read-only links into the sibling `../research` repository (research subjects, observations, and bibliography keys) surfaced by the `research-access` skill. The post files themselves are scaffolded and promoted by the `post-utils` skill. Changes to Claude Code assets (`CLAUDE.md`, `.claude/**`) are logged as MET-nnnn entries in `meta/` via the `claude-meta-changelog` skill.

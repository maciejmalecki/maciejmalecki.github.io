# maciejmalecki.github.io-src

Authoring source for **[maciejmalecki.github.io](https://maciejmalecki.github.io/)** — a Jekyll blog ("()=>{} | Void's blog").

Content and templates are edited here. The built site is published automatically to the `gh-pages` branch of the separate [`maciejmalecki/maciejmalecki.github.io`](https://github.com/maciejmalecki/maciejmalecki.github.io) repo, which GitHub Pages serves.

## Local development

Requires Ruby 3.2.3 and Bundler.

```sh
bundle install
bundle exec jekyll serve   # http://localhost:4000
```

Authoring helpers (via `Rakefile`):

```sh
rake post:create title="My Post" [date=YYYY-MM-DD]
rake page:create title="My Page" [folder=<dir>]
rake site:build | site:serve | site:watch
```

## Structure

Standard Jekyll layout: `_config.yml`, `_layouts/`, `_includes/`, `_sass/` + `css/`, `js/`, `img/`. Blog content is organized into topic sections — `arch/`, `casual/`, `cbm/`, `genai/`, `sh/` — each with its own `_posts/` and `_drafts/`.

## Deployment

Push to `main` → GitHub Actions (`.github/workflows/deploy.yml`) builds the site with `JEKYLL_ENV=production` and force-pushes `_site/` to the published repo's `gh-pages` branch via an SSH deploy key (`ACTIONS_DEPLOY_KEY` secret). See [`CLAUDE.md`](CLAUDE.md) for the full flow and one-time deploy-key setup.

> Note: the Jekyll source was copied (not moved) from the published repo's `main`, which remains as a stale copy pending a future cleanup.

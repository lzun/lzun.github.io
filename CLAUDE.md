# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Site Is

Personal academic website for Luis N. Zuñiga Morales (PhD in Computer Science, Adjunct Professor at Universidad Iberoamericana Ciudad de México). Research focus: multimodal sentiment analysis. Built on the [al-folio](https://github.com/alshedivat/al-folio) Jekyll theme, deployed to `https://lzun.github.io`.

## Essential Commands

```bash
# Start dev server (recommended)
docker compose pull && docker compose up
# Site runs at http://localhost:8080

# Rebuild after Dockerfile/dependency changes
docker compose up --build

# Stop and free port 8080
docker compose down
```

**Before every commit:**

```bash
# Install once
npm install --save-dev prettier @shopify/prettier-plugin-liquid
# Format all files (mandatory — CI fails without this)
npx prettier . --write
```

## Architecture

This is a Jekyll static site. Content is data-driven:

- **`_config.yml`** — Primary config: site URL, feature flags (`enabled: true/false`), nav settings. `url` and `baseurl` must be set consistently (personal site: `url: https://lzun.github.io`, `baseurl:` empty).
- **`_data/`** — YAML data files driving dynamic content: `cv.yml` (RenderCV format, auto-rendered by `render-cv.yml` workflow), `socials.yml`, `repositories.yml`, `coauthors.yml`.
- **`_pages/`** — Static pages (`about.md` is the homepage at `/`).
- **`_teachings/`** — Course entries (Spanish-language academic resources for students).
- **`_bibliography/papers.bib`** — BibTeX publications list; al-folio supports custom fields (`pdf`, `code`, `preview`, `selected`, etc.).
- **`_includes/` / `_layouts/`** — Liquid templates. Don't edit unless doing theme customization.
- **`assets/pdf/cv_lzun.pdf`** — Generated CV PDF (not committed manually; rendered by CI).

## Commit Message Format

```
<type>: <subject>
```

Types: `feat`, `fix`, `docs`, `style`, `config`, `chore`

## Key Pitfalls

- **YAML special characters** in `_config.yml` values must be quoted: `title: "My: Site"`
- **CSS/JS not loading after deploy** → wrong `url`/`baseurl` in `_config.yml`
- **Prettier CI failure** → run `npx prettier . --write` before committing
- **Port 8080 busy** → `docker compose down`

## CI/CD

- `deploy.yml` — builds with `JEKYLL_ENV=production`, runs purgecss, pushes to `gh-pages` branch
- `render-cv.yml` — auto-renders `_data/cv.yml` → `assets/pdf/cv_lzun.pdf`
- `prettier.yml` — blocks PRs with unformatted code

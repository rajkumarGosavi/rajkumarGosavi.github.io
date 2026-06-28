# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Start local dev server (live reload at http://localhost:1313)
hugo server

# Build site to ./public (gitignored)
hugo

# Initialize theme submodule after a fresh clone
git submodule update --init --recursive
```

Hugo version in use: **0.146.3 extended** (required for Sass/SCSS processing).

## Architecture

This is a Hugo static site deployed to GitHub Pages via a GitHub Actions workflow (`.github/workflows/hugo.yaml`). Pushes to `main` trigger an automatic build and deploy.

**Theme**: [ananke](https://github.com/theNewDynamic/gohugo-theme-ananke) installed as a git submodule at `themes/ananke/`.

**Config layout**: Hugo's multi-file config directory is used — `config/_default/` holds:
- `config.toml` — baseURL, theme, menus, markup/math settings
- `params.toml` — site params, social links, TOC, reading time
- `languages.toml` — per-language params (reading speed)

> `config copy.yaml` in the repo root is an unused draft referencing PaperMod-style params; it is not loaded by Hugo.

**Custom layouts** in `layouts/` override the theme's templates. The CSS pipeline (see `layouts/partials/func/style/GetMainCSS.html`) concatenates Tachyons + theme CSS files via Hugo Pipes and processes them with Sass — this requires Hugo Extended.

## Content Sections

Each section maps to a `content/` subdirectory:

| Section | Path | Format |
|---|---|---|
| Blog posts | `content/posts/<slug>/index.md` | Page bundle |
| Algorithm practice | `content/practice/<n>-<slug>/index.md` | Page bundle |
| Readings | `content/readings/_index.md` | Single list file |
| Job essentials series | `content/software-job-essentials/` | Section with `_index.md` |
| Projects | `content/projects/` | In progress |
| About | `content/about.md` | Standalone page |

**Page bundles**: posts and practice problems use leaf bundles — put the `index.md` and any images/assets in the same directory. Hugo resolves image references relative to the bundle.

## Front Matter

Standard fields for posts:

```yaml
---
title: "Post Title"
date: "2024-01-15"
slug: "url-slug"          # overrides directory name in URL
tags: [golang, backend]
draft: false
description: "..."        # used for SEO meta and summaries
---
```

`toc` and `show_reading_time` default to `true` via `params.toml` and can be overridden per page. Math (LaTeX) is enabled via goldmark passthrough — use `\(...\)` for inline and `\[...\]` or `$$...$$` for block math.

## Deployment

The site deploys automatically. There is no manual publish step — merging to `main` is sufficient.

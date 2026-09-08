# kyleries.github.io

Source for [kyleries.com](https://kyleries.com) — Kyle Ries's hub for field notes on data and AI leadership work.

Built by GitHub Pages with Jekyll and the `minima` theme (2.5.x, the version the `github-pages` gem pins). No build step to run locally unless you want a preview.

## Layout

```
_config.yml          site settings, nav order, permalink scheme, analytics token
index.md             home — intro + latest posts (layout: home)
field-notes.md       the series index, ordered by period covered (oldest era first)
work.md              career timeline — the spine that ties posts to eras
about.md             long-form About (mirrors the LinkedIn About, expanded)
now.md               what I'm focused on this month — update monthly
_posts/              one markdown file per post
_layouts/post.html   minima's post layout + the "Field notes · Period covered · Written" line
_layouts/home.html   minima's home layout + the period badge in listings
_includes/head.html  minima's head + optional Cloudflare Web Analytics beacon
assets/main.scss     minima's stylesheet + badge, timeline, and series-list styles
```

## Writing a post

Create `_posts/YYYY-MM-DD-slug.md`. The slug becomes the URL (`/posts/slug/`). Front matter for a field-notes post:

```yaml
---
layout: post
title: "Title"
date: 2026-09-08 09:00:00 -0600      # the real publish date — never backdate
series: field-notes                  # puts it on /field-notes/ and adds the period line
period: "2023–2024"                  # the era the notes cover (display)
period_sort: "2023-04"               # sort key for /field-notes/ (oldest era first)
employer_label: "E Source · NYSERDA IEDR"
tags: [lineage, data-quality]
description: "One or two sentences — used for the OG/SEO description and the series index."
---
```

Put `<!--more-->` after the opening paragraph; everything above it is the excerpt shown on the home page and in the feed.

Ordinary (non-series) posts only need `layout`, `title`, `date`, `tags`, `description`.

## Local preview

```
bundle install          # uses the Gemfile's github-pages gem set
bundle exec jekyll serve
```

## Distribution

kyleries.com is the canonical home. LinkedIn, dev.to and any other copy point back here (canonical URL where the platform supports it, "originally published at kyleries.com" footer otherwise) with a UTM-tagged link so the analytics show which spoke sent the reader.

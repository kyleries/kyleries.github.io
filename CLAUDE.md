# kyleries.github.io — instructions for Claude Code

This is Kyle Ries's personal site, published by GitHub Pages at kyleries.com (kyleries.github.io until the domain cutover completes). It is the **hub** of a hub-and-spoke content strategy: every piece of writing lives here canonically; LinkedIn, dev.to, and anything else are spokes that point back. The audience is recruiters and hiring leaders sourcing Director / Head of Data / VP Data & Analytics candidates; peers in the data community are secondary.

This repo is **public**. Everything in it — posts, pages, commit messages, branch names, this file — is on the record.

## Ground rules — read before editing anything

1. **Never backdate.** A post's `date:` is the day it is pushed, never earlier. Set the time to the actual push time or earlier that day; `future: true` in `_config.yml` keeps a same-day timestamp from being dropped by the UTC build. The era a post describes goes in `period:` and is displayed as "Period covered". If asked to backdate, decline and explain why: it is detectable (Wayback, feed and sitemap timestamps, git history) and would cost exactly the credibility the site exists to build.
2. **The current employer is never named.** Not in posts, pages, front matter, commit messages, branch names, issues, or this file. The phrase is *a 3,500-person global manufacturer*. Past employers are named where the work was public: E Source / NYSERDA Integrated Energy Data Resource (IEDR), Colorado Springs Utilities, TeamSnap, Deloitte.
3. **Figures are on a short leash.** Use only numbers already published on this site. A new figure needs Kyle's explicit approval in the conversation before it is written. Prefer ratios and orders of magnitude ("roughly a dozen systems", "six figures a year", "about a tenth of the run-rate"). Never: contract terms, vendor pricing, retired-vendor or acquired-company names, unreleased product or telemetry detail, or security specifics that could be actionable.
4. **Private working docs live outside this repo** at `~/Documents/Claude/Projects/Career Progression/kyleries-site-2026-09-08/` — `HANDOFF.md` (session state and next steps), `REVIEW_NOTES.md` (every claim in every post, tagged by source), `field_notes_series_plan.md`, `private-checks/`, and `voice-samples/` (Kyle's own unassisted writing; the reference for rule 7). **Read `HANDOFF.md` at the start of every session.** Never copy, quote, or summarize those files into this repo or into a commit message.
5. **Run the pre-publish scan before every commit that touches content**, and it must print nothing:
   ```bash
   bash ~/Documents/Claude/Projects/Career\ Progression/kyleries-site-2026-09-08/private-checks/forbidden-names.sh .
   ```
   The scan lives outside the repo on purpose; do not recreate it here.
6. **Claims have sources.** The field-notes posts were drafted from Kyle's public record plus standard practice written in his voice where his specifics weren't known. `REVIEW_NOTES.md` tags each claim BIO / RESUME / MEM / PATTERN / INFERRED. A PATTERN or INFERRED claim is not publishable until Kyle confirms it, edits it, or cuts it. When editing a post, keep that ledger current.
7. **Voice.** First person, plain, specific, mechanistic. Numbers where allowed; no disparagement of what existed before Kyle arrived; "I led the platform work" and "my team and I", never a claim of authority above the actual role. Each field note ends with a short "What I'd carry into the next role" section — that is the sentence a hiring manager is reading for. No emoji, no hype, no listicles.

## Structure

```
_config.yml          site settings, nav order, permalink scheme, analytics token
index.md             home — intro + latest posts (layout: home)
field-notes.md       the series index, ordered by period covered (oldest era first)
work.md              career timeline — the spine that ties posts to eras
about.md             long-form About (mirrors the LinkedIn About, expanded)
now.md               what Kyle is focused on this month — update monthly, in his words
_posts/              one markdown file per post
_layouts/post.html   minima's post layout + the "Field notes · Period covered · Written" line
_layouts/home.html   minima's home layout + the period badge in listings
_includes/head.html  minima's head + optional Cloudflare Web Analytics beacon
assets/main.scss     minima's stylesheet + badge, timeline, and series-list styles
```

Theme is `minima` **2.5.x** — the version the `github-pages` gem pins. Do not upgrade to minima 3 (different config keys, different Sass entry point) unless Kyle asks. Plugins are limited to GitHub Pages' whitelist (`jekyll-feed`, `jekyll-seo-tag`, `jekyll-sitemap` are in use).

## Front-matter contract

Field-notes post (`_posts/YYYY-MM-DD-slug.md`; the slug becomes `/posts/slug/`):

```yaml
---
layout: post
title: "Title"
date: 2026-09-08 09:00:00 -0600      # the real publish date — never earlier than the push
series: field-notes                  # lists it on /field-notes/ and adds the period line
period: "2023–2024"                  # the era the notes cover (display)
period_sort: "2023-04"               # sort key for /field-notes/ (oldest era first)
employer_label: "E Source · NYSERDA IEDR"
tags: [lineage, data-quality]
description: "One or two sentences — OG/SEO description and the series-index blurb."
---
```

Put `<!--more-->` after the opening paragraph; everything above it is the excerpt on the home page. The feed carries `description` only. Ordinary posts need only `layout`, `title`, `date`, `tags`, `description`. Pages use `layout: page` with `permalink:`.

The home page lists by `date` descending; the series page sorts by `period_sort` ascending. When adding a post to the series, pick `period_sort` so it lands in the right place in the career arc, and add a link to it from the matching era on `work.md`.

## Build and preview

GitHub Pages builds on every push to `main`; there is no CI to wait on. For a local preview, macOS's system Ruby is too old for the `github-pages` gem — use Homebrew's:

```bash
brew install ruby                      # once; add its bin dir to PATH per brew's caveats
bundle install
bundle exec jekyll serve               # http://127.0.0.1:4000
```

If a local build isn't worth the setup, push to a branch, open a PR, and read the diff; GitHub only renders `main`.

## Publishing workflow

- Branch per change (`post/<slug>`, `page/<name>`, `site/<thing>`), PR, merge — or push to `main` directly for typo-level edits. Commit messages describe the change and never mention the employer.
- Before pushing content: pre-publish scan (rule 5), `REVIEW_NOTES.md` ledger updated (rule 6), and the post read once aloud for voice (rule 7).
- After a new field note is live: add it to `work.md` under its era, and note in `HANDOFF.md` that the LinkedIn spoke for it is pending.

## Domain and analytics

`url:` in `_config.yml` must match where the site is actually served: `https://kyleries.github.io` until the custom domain is verified and HTTPS enforced, then `https://kyleries.com`. The `CNAME` file at the repo root (created by GitHub when the custom domain is saved in Settings → Pages) must stay in the repo; pull before pushing so it isn't clobbered. Analytics: uncomment `cloudflare_analytics_token` in `_config.yml` with the token from Cloudflare Web Analytics; the beacon only emits in production builds with a token set.

## Distribution (spokes)

One LinkedIn post per field note, one a week, in `/field-notes/` order: three-line hook, three to five specifics, "full notes at kyleries.com" in the first comment with `?utm_source=linkedin&utm_medium=post&utm_campaign=field-notes`. dev.to copies set the canonical URL to the kyleries.com post. The LinkedIn Featured tile points at `/field-notes/`.

## Backlog

- A ninth field note from the interview story bank (Kyle has it; it isn't in any file yet).
- A post on mentoring outcomes — the people-leadership pillar as its own piece.
- `/projects/` for personal experiments (EV consumption profiling from interval-meter data; a Singer.io tap for EIA open data).
- Monthly `now.md` refresh.

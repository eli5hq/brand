# ELI5 HQ

Short-form animated STEM video for chronically online Gen Z.
Explained like you're 5. The science is still for grown-ups.

## Repo contents

- **`brand-bible.md`** — the operating manual. Voice, visual system,
  editorial formula, channel kit, pre-publish checklist. Read this before
  producing any video, caption, or channel asset.
- **`docs/superpowers/specs/`** — design briefs and spec history. The bible
  expands the brief at
  [`2026-04-22-eli5hq-brand-brief.md`](docs/superpowers/specs/2026-04-22-eli5hq-brand-brief.md).

## Quick orientation

- **Who we're for:** Gen Z / young adults, 16–24. STEM-curious. Allergic
  to lectures.
- **What we make:** 15–60s animated shorts. No talking heads. No politics.
- **Where we post:** TikTok, Instagram Reels, YouTube Shorts.
- **Handle:** `@eli5hq` (same handle across all platforms).
- **Cadence:** new videos Tuesday and Friday.

## Working in this repo

- Voice, visual system, editorial formula: change `brand-bible.md` first.
  The bible is the spec; every downstream asset executes against it.
- Version the bible (v1.0 → v1.1, etc.) when a locked element changes.
  Section 7 of the bible spells out what's locked vs. flexible.
- New briefs or strategic changes live under `docs/superpowers/specs/`,
  dated.

## Branching

- `main` — the source of truth. Only merged, reviewed work lands here.
- `develop` — integration branch. All work lands here first via PR.

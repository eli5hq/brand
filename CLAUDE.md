# CLAUDE.md — ELI5 HQ Brand Repo

Orientation for future Claude sessions working in this repository.

## What this is

Brand system and tooling for **ELI5 HQ** — a short-form animated STEM
video brand for Gen Z (16–24). This repo is *not* product code; it's the
operating manual for everything voice, visual, and editorial, plus a
deployable style guide.

## Source of truth

- **`brand-bible.md`** is the operating manual. Any brand, voice, visual,
  or editorial question — check here first. If a claim contradicts the
  bible, the bible wins (unless the user is explicitly changing it, in
  which case update the bible in the same change).
- **`index.html`** is the style guide, *rendered* from the bible. It is
  downstream of the bible — the bible drives it, not the other way around.
  If the bible changes, update `index.html` to match.

## Deployed

- Published at **https://eli5hq.github.io/brand/** via GitHub Pages,
  serving from `main` branch root.
- Repo is public at https://github.com/eli5hq/brand.

## File layout

```
/
├── CLAUDE.md                       ← this file
├── README.md                       ← human-facing repo orientation
├── brand-bible.md                  ← the operating manual (source of truth)
├── index.html                      ← the rendered style guide (deployed)
└── docs/
    └── superpowers/
        ├── specs/                  ← design specs (YYYY-MM-DD-<topic>-design.md)
        └── plans/                  ← implementation plans (YYYY-MM-DD-<topic>.md)
```

Specs live under `docs/superpowers/specs/`, plans under
`docs/superpowers/plans/`. Do not create parallel `design/` or `planning/`
directories — use the existing paths.

## Working conventions

### Branching

- `main` — source of truth. GitHub Pages publishes from here.
- `develop` — integration branch. All work lands here first.
- Feature branches optional for complex work; direct commits to `develop`
  are fine for small changes per the user's preference.
- Never push to `main` directly. Always PR `develop` → `main`.

### Commits

- Imperative tense, no conventional-commits prefix required (match
  existing log).
- Always include the `Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>`
  footer on Claude-authored commits.
- Stage specific files by name, not `git add -A`.

### PRs

- Use `gh pr create --base main --head develop`.
- Body uses the template: Summary / What's in here / Test plan.
- Close the PR loop — if the user merges, verify with `gh pr view <n>`
  before moving on.

## Locked design decisions — do not re-litigate

These are settled. Only change them if the user explicitly asks for a
rebrand. They're locked in `brand-bible.md` §7 "Governance → Locked."

| Decision         | Value                                              |
| ---------------- | -------------------------------------------------- |
| Primary color    | Hazard Pink `#FF2E63`                              |
| Anchor color     | Off-Black `#0E0E12`                                |
| Warm alt color   | Paper Cream `#F5F1E8`                              |
| Display font     | Space Grotesk (500, 700)                           |
| Body font        | Inter (400, 500, 600)                              |
| Primary handle   | `@eli5hq`                                          |
| Editorial formula | 4-beat: dumb premise → whiplash pivot → real payoff → stinger |
| Video length     | 15–60s (sweet spot 22–45s)                         |
| Format rule      | No talking heads. Ever.                            |
| Scope            | STEM only — no politics, no news, no self-help     |
| Audience         | Gen Z / young adults 16–24                         |

The secondary sticker-pack palette (Lab Lime, Plasma Violet, Signal
Orange, Beaker Blue, Moss Green) is mapped to pillars in the bible but
these mappings are marked flexible — they can evolve with analytics.

## Style guide technical constraints

- **No build step.** Tailwind via CDN, Google Fonts via `<link>`, inline
  `<style>` for custom CSS. Do not introduce a bundler, PostCSS config,
  or Node dependency without asking first.
- **No JavaScript** beyond the Tailwind CDN script.
- **Single file.** `index.html` at the repo root. If it grows too big
  to hold in context, split is a last resort — discuss first.
- **Hazard Pink rule:** every content section must include Hazard Pink
  somewhere. This is a bible-level rule; check for compliance before
  shipping visual changes.
- **Fonts:** never introduce a third font family. Space Grotesk + Inter
  only. Fallbacks are Archivo Black / Bagel Fat One (display) and
  Manrope / DM Sans (body).

## Brainstorm → spec → plan → implement workflow

This repo has been built using the `superpowers` skills flow:

1. **Brainstorm** creative work → write a spec to
   `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`.
2. **Writing-plans** turns the spec into
   `docs/superpowers/plans/YYYY-MM-DD-<topic>.md` (13-task-style, checkboxes).
3. **Implement** — inline or via subagents.

Follow the flow for new creative work. Skip it for trivial edits (typos,
copy fixes). When in doubt, ask the user whether to go light or heavy.

## Things to check before declaring done

- Visual-only work: confirm the user has eyes on the result. The user
  verifies visuals; Claude does not have a browser.
- Lighthouse score ≥ 95 on `index.html` is the accessibility gate. If
  Lighthouse isn't available locally, say so and name it as the user's
  manual step.
- GitHub Pages deploys take ~60s for the first build, ~30s for subsequent
  ones. `gh api /repos/eli5hq/brand/pages/builds/latest` returns build
  status (`building` / `built` / `errored`).

## Non-obvious

- The wordmark's `5` is the load-bearing personality character. `HQ` is
  a minor stamp. Do not resize `HQ` up for "balance" — that's an
  anti-pattern flagged explicitly in the bible.
- The "Hazard Pink + Signal Orange" pairing is **banned** (the bible
  says they fight). Any design that needs both is wrong.
- Chalk White on Hazard Pink fails WCAG AA for body text (3.3:1). Only
  use that pairing on hook text ≥ 32pt where large-text AA applies.

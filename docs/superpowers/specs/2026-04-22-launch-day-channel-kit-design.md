# Launch-Day Channel Kit — Design Spec

**Status:** Draft (pending implementation plan)
**Owner:** Steve Harmeyer
**Date:** 2026-04-22
**Governing source:** `brand-bible.md` v1.0 (especially §3 Visual Identity, §5 Channel Kit)

---

## 1. Summary

The minimum artifact set needed to stand up `@eli5hq` on TikTok, Instagram,
and YouTube on launch day. Every file is checked into Git so the repo is the
single source of truth for what gets uploaded.

The kit is scoped to the three short-form vertical-video platforms. It does
not cover Threads, X, BlueSky, Pinterest, Snapchat, Reddit — those are
defensive handle claims only (see §2 Scope out).

## 2. Scope

### In scope

1. `assets/social/avatar.svg` + `.png` — primary avatar (the `5` monogram on
   a Hazard Pink tile), used on TikTok, Instagram, YouTube.
2. `assets/social/avatar-alt.svg` + `.png` — alternate avatar (Pink `5` on
   Off-Black tile) for pinned stories / temporary events per bible §5.
3. `assets/social/youtube-banner.svg` + `.png` — 2560×1440 YouTube channel
   banner with embedded mobile-safe-zone guide layer.
4. `assets/social/youtube-watermark.svg` + `.png` — 150×150 video watermark
   overlay for YouTube Shorts (transparent BG).
5. `assets/social/instagram-highlights/{why,math,tech,nature,space}.svg`
   + `.png` — five Instagram highlight covers, one per content pillar.
6. `assets/social/README.md` — short doc covering the SVG→PNG export
   command and file-use notes.
7. `platform-setup.md` (repo root) — launch-day runbook: paste-ready copy,
   asset paths, and a defensive-claims checklist.
8. New `#channel-kit` section in `index.html`, placed between `#logo` and
   `#hooks`, rendering every asset with download links.

### Scope out (explicitly not in this spec)

- Intro short video (bible §5 calls for one; produce separately).
- Linktree / linkin.bio aggregator page.
- Sticker pack, character-blob library, script templates (future specs).
- Thumbnail templates for YouTube Shorts (future spec).
- Defensive handle claims on non-vertical-video platforms (Threads, X,
  BlueSky, Pinterest, Snapchat, Reddit) — listed in the runbook as
  checkboxes but we produce no assets for them.

## 3. Architecture

### File layout

```
brand/
├── index.html                          ← gains new #channel-kit section
├── platform-setup.md                   ← NEW, repo root
└── assets/
    └── social/                         ← NEW directory
        ├── README.md
        ├── avatar.svg / .png
        ├── avatar-alt.svg / .png
        ├── youtube-banner.svg / .png
        ├── youtube-watermark.svg / .png
        └── instagram-highlights/
            ├── why.svg / .png
            ├── math.svg / .png
            ├── tech.svg / .png
            ├── nature.svg / .png
            └── space.svg / .png
```

### Source-of-truth flow

- SVGs are hand-authored. They are the canonical source.
- PNGs are rendered from SVGs via a documented one-liner (see §7) and
  committed alongside the SVG. PNGs exist because TikTok, Instagram, and
  YouTube do not accept SVG uploads.
- `index.html` references SVGs directly via `<img src="assets/social/…">`.
  Browsers render SVG natively — no JS, no duplication between style guide
  and source files.
- `platform-setup.md` references PNG paths (what actually gets uploaded)
  and duplicates copy blocks from `brand-bible.md` §5 verbatim. If §5
  changes, the runbook updates in the same commit.

## 4. Shared element: the `5` glyph

The `5` is the load-bearing personality character per bible §3. It is
reused across avatar (primary + alt), watermark, and the banner wordmark.

- **Authoring style:** hand-scrawled sticker-digit, per bible §3 Logo
  Concept Brief option (a). Flat color, no gradients, no strokes
  (fills only).
- **Canvas:** authored on a 1000×1000 viewBox for clean scaling.
- **Fill:** uses `currentColor` so the same path set is recolorable by CSS
  without editing the file.
- **Minimum legibility:** must remain readable at 40×40px per bible §3
  hard rules.

A single `<symbol id="eli5-five">` (or equivalent reusable path set) should
live in one place and be referenced from avatar/watermark/banner files, so
a later glyph refinement updates everywhere in one edit.

## 5. Asset specs

### 5.1 Avatar (`avatar.svg`)

- **Canvas:** 1024×1024.
- **Background:** full-bleed Hazard Pink `#FF2E63`.
- **`5` glyph:** Off-Black `#0E0E12`, centered, glyph height ≈ 64% of canvas.
- **No wordmark** (bible §5 — too small to read at avatar size).
- **Clear space:** height of the `5` on all sides (bible §3 hard rule).
- **Raster export:** `avatar.png` at 1024×1024. Platforms downscale cleanly
  from 1024 (covers TikTok 1080, IG 320, YouTube 800).

### 5.2 Avatar alt (`avatar-alt.svg`)

- Same dimensions and layout as primary.
- Colors inverted: Off-Black `#0E0E12` background, Hazard Pink `#FF2E63`
  glyph.
- **Use:** pinned stories / event surfaces only, per bible §5.

### 5.3 YouTube banner (`youtube-banner.svg`)

- **Canvas:** 2560×1440.
- **Background:** full-bleed Off-Black `#0E0E12`.
- **Mobile-safe zone (1546×423, centered):** the only region guaranteed
  visible on every device. All critical copy sits inside.
  - Left third: wordmark `ELI5 HQ`. Space Grotesk 700, tight tracking
    (approx -3%). Chalk White `#FAFAF5` for `ELI` and `HQ`; the `5` renders
    as the hand-scrawled sticker glyph in Hazard Pink `#FF2E63` (this is
    the required Hazard Pink signature for the banner).
  - Middle third: tagline in two lines:
    - Line 1: `STEM explained like you're 5.`
    - Line 2: `the science is still for grown-ups.`
    - Inter 500, Chalk White.
  - Right third: cadence callout `new drops  tue + fri` in Space Grotesk
    700, Lab Lime `#C6F24E`. The `+` character gets a small Hazard Pink
    scribble underline for sticker energy.
- **Bleed zone (outside mobile-safe):** four sticker-style elements, one
  per pillar, positioned so that at each device crop tier one or two
  elements peek past the safe-zone edge:
  - Plasma Violet worried-photon blob (Space pillar).
  - Lab Lime oversized `52! =` number fragment (Math pillar).
  - Beaker Blue sweaty-CPU character (Tech pillar).
  - Moss Green platypus outline (Nature pillar).
- The "Why does X do that?" pillar is **intentionally omitted** from the
  bleed sticker set. Its accent color is Signal Orange, and bible §3
  forbids pairing Hazard Pink with Signal Orange — they would be
  co-visible at TV/desktop crop with the Pink `5` in the safe zone. The
  Why pillar is instead represented editorially by the tagline itself
  ("STEM explained like you're 5").
- Each bleed sticker is off-register by 2–3px between fill and outline
  for the sticker-sheet feel called out in bible §3 art direction.
- **Guide layer:** the SVG contains a `<g id="guides" display="none">`
  with pink-outline rectangles for the 1546×423 mobile-safe zone and
  1855×423 tablet zone. The exported PNG does not show guides; the style
  guide page unhides them via scoped CSS (see §8).
- **Raster export:** `youtube-banner.png` at 2560×1440, guide layer hidden.

### 5.4 YouTube watermark (`youtube-watermark.svg`)

- **Canvas:** 150×150.
- **Background:** transparent (not a tile — overlays video).
- **`5` glyph only:** Chalk White `#FAFAF5` fill. Glyph fills ≈80% of
  canvas height.
- **Legibility assist:** Off-Black `#0E0E12` drop shadow at ~30% opacity
  and ~2px offset, so the glyph survives both dark and bright video frames.
  This is a deliberate, documented exception to the bible's "no gradients
  / flat only" preference — a watermark that vanishes against bright video
  fails its job. The shadow is scoped to this file only.
- **Raster export:** `youtube-watermark.png` at 300×300 (2× for crispness;
  YouTube downscales).

### 5.5 Instagram highlight covers (5 files)

All five covers share identical structure; only background color and word
differ.

- **Canvas:** 1080×1920.
- **Icon-safe square:** Instagram crops highlights to a circle; meaningful
  content sits in a centered ~500×500 square (≈38% radius from center).
- **Background:** full-bleed pillar color.
- **Word:** pillar name in Space Grotesk 700, Off-Black `#0E0E12`, tight
  tracking. Font size set so the longest word (`NATURE`) fits in the
  icon-safe square with margin; all five covers use the same size so the
  set reads as a cohesive system.
- **Hazard Pink signature:** a 4px-tall Hazard Pink bar centered directly
  beneath the word (small but mandatory — bible rule that every surface
  includes Pink).

| File | Background | Word | Pillar |
|---|---|---|---|
| `why.svg` | Signal Orange `#FF8A1F` | WHY | Why does X do that? |
| `math.svg` | Lab Lime `#C6F24E` | MATH | The math is insane |
| `tech.svg` | Beaker Blue `#2EC4FF` | TECH | Tech, explained dumb |
| `nature.svg` | Moss Green `#4F8A3C` | NATURE | Nature is unhinged |
| `space.svg` | Plasma Violet `#7A3BFF` | SPACE | Space stuff |

**Contrast check (Off-Black text on pillar BG):**

| Cover | Contrast | AA status |
|---|---|---|
| why (Signal Orange) | ~9:1 | Passes normal-text AA |
| math (Lab Lime) | ~14:1 | Passes normal-text AA |
| tech (Beaker Blue) | ~7.5:1 | Passes normal-text AA |
| nature (Moss Green) | ~6:1 | Passes normal-text AA |
| space (Plasma Violet) | ~3.9:1 | Below normal AA; passes large-text AA at the cover's word size |

The SPACE cover relies on large-text AA (3:1 minimum) because the word is
set large enough to qualify. Do not reuse Off-Black on Plasma Violet at
small sizes elsewhere.

- **Raster export:** each cover exports to PNG at 1080×1920.

## 6. `platform-setup.md` runbook

Lives at repo root. Structure:

```
# ELI5 HQ — Platform Setup Runbook

## Before you start (10 min)
- Sign-in prep, 2FA, handle-fallback rule (same fallback on ALL three
  platforms — unified handle beats per-platform optimization, bible §5).

## Pre-register checklist (defensive claims — bible §5)
Checkboxes for Threads, X, BlueSky, Pinterest, Snapchat, Reddit
(claim only — no population).

## TikTok
Display name, handle, avatar path, bio (paste block, 64 chars from bible §5),
link placeholder, category. Note: pin-3 strategy deferred until content ships.

## Instagram
Display name, handle, avatar path, bio (paste block, 133 chars from bible §5),
link placeholder, category. Highlight cover paths listed but flagged: only
create the highlight once 2+ videos exist in that pillar (empty highlights
age a profile).

## YouTube
Channel name, handle, avatar path, banner path, watermark upload
instructions (Studio → Customization → Branding → display time "Entire
video"), short bio (100 chars from bible §5), About description (~370
chars from bible §5), category, featured video placeholder.

## Post-launch verification
Checkboxes for: avatars render uncropped, banner text survives mobile crop,
watermark appears on test upload, bios render correct line breaks, handle
identical across all three.
```

**Copy rule:** every paste block duplicates bible §5 verbatim. Do not edit
copy in the runbook — edit the bible and the runbook in the same commit.

## 7. PNG export workflow

- **Tool:** `rsvg-convert` (via `brew install librsvg` on macOS, or distro
  package on Linux). Chosen because it renders SVG deterministically at
  arbitrary sizes without a headless browser.
- **Command pattern per asset:** documented in `assets/social/README.md`
  with exact width/height flags per file. Example:
  `rsvg-convert -w 1024 -h 1024 avatar.svg -o avatar.png`.
- **When to re-run:** whenever an SVG changes. PNGs and SVGs are committed
  together so the repo never ships drift between source and raster.
- **No CI automation in this spec.** Running the command is a manual step
  on the same commit that edits the SVG. A future spec can add a pre-
  commit hook or CI check if drift becomes a real problem.

## 8. `index.html` Channel Kit section

- **Placement:** new `<section id="channel-kit">` inserted between the
  existing `#logo` section and the existing `#hooks` section, making it
  the 5th of 9 sections. Follows the existing pattern: `py-16`,
  `max-w-6xl` container, kicker + h2 headline.
- **Content blocks:**
  1. Avatar: primary and alt rendered at 240px round via CSS
     `border-radius: 50%` on `<img>`. Labels beneath each. Download links
     to `.svg` and `.png`.
  2. YouTube banner: rendered at full container width with guide layer
     **visible** (scoped CSS under `#channel-kit .banner-preview svg
     #guides { display: block; }`). Caption labels the safe-zone boxes.
     Download links.
  3. Watermark: rendered at 120px on a two-up preview — left tile
     Off-Black, right tile Paper Cream — to demonstrate the watermark
     surviving both ends of the brightness range. Download links.
  4. Instagram highlights: row of five 120px circles with pillar name
     captions beneath. Download links.
  5. Setup runbook callout: linking to `platform-setup.md` (GitHub blob
     URL, or relative when viewed on GitHub Pages — the former is more
     robust).
- **SVG rendering:** `<img src="assets/social/avatar.svg" alt="…">`. No
  JS. Every image has meaningful alt text.
- **Accessibility:** section h2 gets `id="channel-kit"` for anchor links.
  Color contrast for section text reuses existing style-guide patterns
  (Chalk White on Off-Black). Must preserve the Lighthouse ≥95 gate from
  the CLAUDE.md workflow.

## 9. Constraints to honor

- **No build step.** No bundler, no PostCSS, no Node dependency added to
  the repo. `rsvg-convert` runs locally on demand; its outputs are
  committed.
- **No JavaScript** beyond the existing Tailwind CDN script.
- **Single `index.html` file.** One new section added; no splitting.
- **Hazard Pink is mandatory on every standalone surface.** Avatar has
  full-bleed Pink; banner has the Pink `5`; highlight covers use the Pink
  underscore bar. The watermark is an exception — it is a video overlay,
  not a standalone surface, and the bible's Pink requirement is satisfied
  by the Pink in the video content it appears on top of (bible §3 rule
  applies per-video, not per-overlay-element).
- **No third font family.** Space Grotesk and Inter only.
- **Never pair Hazard Pink with Signal Orange** (bible §3 forbids it).
  The banner's bleed stickers use Signal Orange; the Pink signature lives
  in the safe zone. They do not touch.
- **No science-cliché iconography** for the logo/primary marks. The
  banner bleed stickers are the "blob with eyes" anthropomorphized style
  allowed by bible §3 art direction, not textbook atoms/beakers/gears.

## 10. Success criteria

Launch day is considered unblocked when:

1. All 18 asset files exist in `assets/social/` (9 SVG sources + 9 PNG
   exports: avatar + avatar-alt + youtube-banner + youtube-watermark + 5
   highlight covers, each as SVG and PNG) plus the `README.md` export
   doc, and the PNGs render visually identical to the SVGs.
2. `platform-setup.md` exists at repo root and includes every paste block
   from bible §5 verbatim.
3. `index.html` renders a `#channel-kit` section showing every asset with
   working download links, and still passes Lighthouse ≥95.
4. A user following `platform-setup.md` top-to-bottom can register
   `@eli5hq` on TikTok, Instagram, and YouTube with correct avatars,
   YouTube banner, YouTube watermark, bios, and descriptions — without
   opening any other document.
5. All assets pass their contrast checks documented in §5.5 (highlight
   covers) and do not violate any bible §3 hard rules.

## 11. Open items to resolve during implementation

- Final visual design of the hand-scrawled `5` glyph (paths). The spec
  locks the intent; the plan commits the geometry.
- Final bleed-sticker illustrations for the banner. Spec locks colors and
  which pillar each represents; the plan commits the SVG geometry.
- GitHub Pages link behavior for `platform-setup.md` download link —
  whether to use relative (`./platform-setup.md`) or absolute GitHub blob
  URL. Decide during implementation based on how GitHub Pages serves
  `.md` files from this repo.

---

**Version:** 1.0 · **Date:** 2026-04-22

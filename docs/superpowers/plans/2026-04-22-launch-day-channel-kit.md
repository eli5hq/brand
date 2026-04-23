# Launch-Day Channel Kit — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Produce every SVG/PNG asset, the launch-day runbook, and a new `#channel-kit` section in the style guide so `@eli5hq` can be stood up on TikTok, Instagram, and YouTube with only the files committed in this repo.

**Architecture:** Hand-authored SVGs are the canonical source. PNGs are rendered from SVGs via `rsvg-convert` and committed alongside (social platforms require raster uploads). `index.html` references the SVGs directly via `<img>` tags; safe-zone guides are drawn as HTML overlays — not baked into the SVG — so the banner SVG stays clean for upload. `platform-setup.md` at repo root duplicates copy from `brand-bible.md` §5 verbatim; the bible is the source of truth.

**Tech Stack:**
- SVG 1.1 (no web-only features; must render through `rsvg-convert`)
- `rsvg-convert` (librsvg) for PNG export
- Space Grotesk + Inter fonts (Google Fonts in browser; system-installed via Homebrew for `rsvg-convert` fidelity)
- Tailwind CDN + existing custom color/font config in `index.html` (unchanged)

---

## Governing spec

`docs/superpowers/specs/2026-04-22-launch-day-channel-kit-design.md`

## Canonical constants (referenced by multiple tasks)

### The `5` glyph path — single source of truth

Drawn on a 1000×1000 viewBox, filled only (no strokes), uses `fill="currentColor"` so the same path recolors via CSS/`color` attribute. Every asset uses this **exact** path string verbatim. If the path is later refined, update every file in a single commit.

```
M 210 165 L 745 155 L 735 295 L 355 305 L 345 445 Q 505 405 660 455 Q 840 515 855 695 Q 840 870 640 895 Q 430 905 225 830 L 285 695 Q 425 765 575 755 Q 705 745 700 640 Q 695 560 560 555 Q 430 560 335 605 L 200 595 Z
```

### Palette constants

| Name | Hex |
|---|---|
| Hazard Pink | `#FF2E63` |
| Off-Black | `#0E0E12` |
| Paper Cream | `#F5F1E8` |
| Chalk White | `#FAFAF5` |
| Lab Lime | `#C6F24E` |
| Plasma Violet | `#7A3BFF` |
| Signal Orange | `#FF8A1F` |
| Beaker Blue | `#2EC4FF` |
| Moss Green | `#4F8A3C` |
| Smoke 900 | `#1A1A20` |
| Smoke 500 | `#6B6B78` |

---

## Task 1: Create `assets/social/` directory and README

**Files:**
- Create: `assets/social/README.md`

- [ ] **Step 1: Verify `rsvg-convert` is installed**

Run: `which rsvg-convert && rsvg-convert --version`
Expected: a path + version string (e.g., `rsvg-convert version 2.57.x`).

If missing on macOS: `brew install librsvg`. On Debian/Ubuntu: `sudo apt install librsvg2-bin`.

- [ ] **Step 2: Verify Space Grotesk and Inter are installed system-wide**

Run: `fc-list | grep -iE "space grotesk|inter" | head`
Expected: at least `Space Grotesk` and `Inter` families appear.

If missing on macOS: `brew install --cask font-space-grotesk font-inter`. Re-run the verify command.

- [ ] **Step 3: Create the `assets/social/` directory**

Run: `mkdir -p assets/social/instagram-highlights`
Expected: directory exists, no error.

Verify: `ls -d assets/social/instagram-highlights` prints the path.

- [ ] **Step 4: Write `assets/social/README.md`**

Create `assets/social/README.md` with exactly this content:

````markdown
# Social assets — `@eli5hq` launch-day channel kit

This directory is the canonical source for every social-media asset the
brand ships to TikTok, Instagram, and YouTube. SVGs are the source of
truth; PNGs are rendered from the SVGs via `rsvg-convert` and committed
alongside (the social platforms do not accept SVG uploads).

See also:
- Spec: `docs/superpowers/specs/2026-04-22-launch-day-channel-kit-design.md`
- Runbook: `platform-setup.md` (repo root) — what to paste where
- Style guide: the `#channel-kit` section of `index.html`

## Prerequisites

```bash
# librsvg provides rsvg-convert
brew install librsvg

# Brand fonts (required for PNG rendering fidelity via rsvg-convert)
brew install --cask font-space-grotesk font-inter
```

Verify fonts with: `fc-list | grep -iE "space grotesk|inter"`.

## Files

| File | Canvas | Used for |
| --- | --- | --- |
| `avatar.svg` / `.png` | 1024×1024 | TikTok, Instagram, YouTube profile photo |
| `avatar-alt.svg` / `.png` | 1024×1024 | Pinned stories / event surfaces |
| `youtube-banner.svg` / `.png` | 2560×1440 | YouTube channel banner |
| `youtube-watermark.svg` / `.png` | 150×150 source · 300×300 PNG | YouTube Shorts video watermark |
| `instagram-highlights/why.svg` / `.png` | 1080×1920 | IG highlight cover — Why does X |
| `instagram-highlights/math.svg` / `.png` | 1080×1920 | IG highlight cover — The math is insane |
| `instagram-highlights/tech.svg` / `.png` | 1080×1920 | IG highlight cover — Tech, explained dumb |
| `instagram-highlights/nature.svg` / `.png` | 1080×1920 | IG highlight cover — Nature is unhinged |
| `instagram-highlights/space.svg` / `.png` | 1080×1920 | IG highlight cover — Space stuff |

## Exporting PNGs

Run from `assets/social/`:

```bash
rsvg-convert -w 1024 -h 1024 avatar.svg     -o avatar.png
rsvg-convert -w 1024 -h 1024 avatar-alt.svg -o avatar-alt.png
rsvg-convert -w 2560 -h 1440 youtube-banner.svg    -o youtube-banner.png
rsvg-convert -w 300  -h 300  youtube-watermark.svg -o youtube-watermark.png

cd instagram-highlights
for f in why math tech nature space; do
  rsvg-convert -w 1080 -h 1920 "$f.svg" -o "$f.png"
done
cd ..
```

Whenever an SVG changes, re-export its PNG and commit both together. The
repo ships source and raster in lockstep — no drift allowed.

## Rules to honor (from `brand-bible.md` §3)

- **Hazard Pink** (`#FF2E63`) appears on every standalone surface.
- **Never** place Hazard Pink adjacent to Signal Orange (`#FF8A1F`).
- **No third font family** — Space Grotesk and Inter only.
- **No gradients** in logo/primary identity work (watermark drop-shadow
  is the single documented exception for video-overlay legibility).
- **No science-cliché iconography** (atoms, beakers, gears, lightbulbs).
  Anthropomorphized "blob with eyes" characters are fine per bible §3
  art direction.
````

- [ ] **Step 5: Commit**

```bash
git add assets/social/README.md
git commit -m "$(cat <<'EOF'
Scaffold assets/social/ with README and export instructions

Introduces the directory for the launch-day channel kit plus its
prerequisites, file manifest, and PNG-export workflow.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

Expected: one commit created; `git status` shows clean working tree; `ls assets/social/` shows `README.md` and the `instagram-highlights/` subdirectory.

---

## Task 2: Primary avatar (`avatar.svg` + `avatar.png`)

**Files:**
- Create: `assets/social/avatar.svg`
- Create: `assets/social/avatar.png` (rendered output)

- [ ] **Step 1: Define the expected outcome**

A 1024×1024 square image: full-bleed Hazard Pink (`#FF2E63`) background with an Off-Black (`#0E0E12`) hand-scrawled `5` centered and filling roughly 64% of the canvas height. No wordmark. No gradients. The `5` remains legible at 40×40 px.

- [ ] **Step 2: Write `assets/social/avatar.svg`**

Create `assets/social/avatar.svg` with exactly this content:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1024 1024" width="1024" height="1024">
  <title>ELI5 HQ avatar — Off-Black 5 on Hazard Pink tile</title>
  <!-- Background tile -->
  <rect width="1024" height="1024" fill="#FF2E63"/>
  <!-- Hand-scrawled 5 glyph, centered, glyph height ~64% of canvas.
       Source glyph is authored on a 1000x1000 viewBox; scaled to 655px
       here (65.5% of 1024) and translated to center. -->
  <g transform="translate(184.5 184.5) scale(0.655)" fill="#0E0E12">
    <path d="M 210 165 L 745 155 L 735 295 L 355 305 L 345 445 Q 505 405 660 455 Q 840 515 855 695 Q 840 870 640 895 Q 430 905 225 830 L 285 695 Q 425 765 575 755 Q 705 745 700 640 Q 695 560 560 555 Q 430 560 335 605 L 200 595 Z"/>
  </g>
</svg>
```

- [ ] **Step 3: Render `avatar.png`**

Run from repo root:

```bash
rsvg-convert -w 1024 -h 1024 assets/social/avatar.svg -o assets/social/avatar.png
```

Expected: no errors. File `assets/social/avatar.png` created.

- [ ] **Step 4: Verify PNG dimensions**

Run: `file assets/social/avatar.png`
Expected: output contains `1024 x 1024`.

- [ ] **Step 5: Visual check**

Open `assets/social/avatar.svg` in a browser (drag-drop into any tab). Confirm:
- Pink background fills the whole square.
- The `5` is legible, centered, and has visible hand-drawn irregularity (not a perfect geometric 5).
- No accidental clipping at the edges — at least ~10% of canvas is clear-space on every side of the glyph.

Open `assets/social/avatar.png` in a previewer. Same checks.

If the glyph looks off — too sharp, too centered-perfect, clipped — adjust the `M ... Z` path coordinates in `avatar.svg` and re-run Step 3 before committing. The path is a hand-drawn element; small iteration is expected.

- [ ] **Step 6: Commit**

```bash
git add assets/social/avatar.svg assets/social/avatar.png
git commit -m "$(cat <<'EOF'
Add primary avatar: Off-Black 5 on Hazard Pink tile

SVG is the source; PNG rendered at 1024x1024 via rsvg-convert. Used
across TikTok, Instagram, and YouTube profile photos.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 3: Alternate avatar (`avatar-alt.svg` + `avatar-alt.png`)

**Files:**
- Create: `assets/social/avatar-alt.svg`
- Create: `assets/social/avatar-alt.png`

- [ ] **Step 1: Define the expected outcome**

Same 1024×1024 canvas as the primary avatar, with colors inverted: Off-Black background, Hazard Pink `5`. Used for pinned stories and event surfaces only per bible §5.

- [ ] **Step 2: Write `assets/social/avatar-alt.svg`**

Create `assets/social/avatar-alt.svg` with exactly this content:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1024 1024" width="1024" height="1024">
  <title>ELI5 HQ avatar alt — Hazard Pink 5 on Off-Black tile</title>
  <rect width="1024" height="1024" fill="#0E0E12"/>
  <g transform="translate(184.5 184.5) scale(0.655)" fill="#FF2E63">
    <path d="M 210 165 L 745 155 L 735 295 L 355 305 L 345 445 Q 505 405 660 455 Q 840 515 855 695 Q 840 870 640 895 Q 430 905 225 830 L 285 695 Q 425 765 575 755 Q 705 745 700 640 Q 695 560 560 555 Q 430 560 335 605 L 200 595 Z"/>
  </g>
</svg>
```

- [ ] **Step 3: Render `avatar-alt.png`**

Run: `rsvg-convert -w 1024 -h 1024 assets/social/avatar-alt.svg -o assets/social/avatar-alt.png`

Expected: no errors; file created.

- [ ] **Step 4: Visual check**

Open both in a browser. Confirm:
- Dark background, Pink `5`.
- Glyph shape is identical to primary (only colors differ).

- [ ] **Step 5: Commit**

```bash
git add assets/social/avatar-alt.svg assets/social/avatar-alt.png
git commit -m "$(cat <<'EOF'
Add alternate avatar: Hazard Pink 5 on Off-Black tile

Same glyph as primary with colors inverted. Reserved for pinned stories
and event surfaces per bible §5.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 4: YouTube banner — safe-zone composition

**Files:**
- Create: `assets/social/youtube-banner.svg`
- Create: `assets/social/youtube-banner.png`

This task builds the banner with the mobile-safe zone contents only. Task 5 adds the bleed-zone stickers on top of the same file.

- [ ] **Step 1: Define the expected outcome**

A 2560×1440 image, Off-Black full-bleed. Inside the centered 1546×423 mobile-safe zone, three blocks:
- Left third: wordmark `ELI`+`5`+`HQ` — `ELI` and `HQ` in Chalk White Space Grotesk 700, the `5` in Hazard Pink using the canonical glyph path.
- Middle third: two-line tagline in Inter 500 Chalk White.
- Right third: "new drops · tue + fri" in Space Grotesk 700 Lab Lime, with a Hazard Pink scribble under the `+`.

No bleed-zone art yet — that comes in Task 5.

- [ ] **Step 2: Write `assets/social/youtube-banner.svg`**

Create `assets/social/youtube-banner.svg` with exactly this content:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 2560 1440" width="2560" height="1440">
  <title>ELI5 HQ — YouTube channel banner</title>

  <!-- Full-bleed background -->
  <rect width="2560" height="1440" fill="#0E0E12"/>

  <!-- Mobile-safe zone content. Safe zone is 1546x423 centered at (1280, 720).
       Left = 507, Right = 2053, Top = 508.5, Bottom = 931.5. Three thirds of
       507px width each, separated by small gutters. -->

  <!-- LEFT THIRD: wordmark "ELI 5 HQ" at y ~720 baseline -->
  <g transform="translate(600 560)">
    <!-- "ELI" in Chalk White -->
    <text x="0" y="110" font-family="'Space Grotesk', sans-serif" font-weight="700"
          font-size="160" fill="#FAFAF5" letter-spacing="-4">ELI</text>
    <!-- Custom 5 glyph in Hazard Pink, sized to match the E/L/I cap height -->
    <g transform="translate(260 -20) scale(0.16)" fill="#FF2E63">
      <path d="M 210 165 L 745 155 L 735 295 L 355 305 L 345 445 Q 505 405 660 455 Q 840 515 855 695 Q 840 870 640 895 Q 430 905 225 830 L 285 695 Q 425 765 575 755 Q 705 745 700 640 Q 695 560 560 555 Q 430 560 335 605 L 200 595 Z"/>
    </g>
    <!-- "HQ" as a smaller stamp in Chalk White on a Chalk White tile — here
         the bible calls for HQ as a minor stamp. Implemented as tightly-set
         Space Grotesk 700 in a thin rounded rect to read as a stamp. -->
    <g transform="translate(0 180)">
      <rect x="0" y="0" width="180" height="60" rx="4" fill="#FAFAF5"/>
      <text x="90" y="44" font-family="'Space Grotesk', sans-serif" font-weight="700"
            font-size="40" fill="#0E0E12" text-anchor="middle" letter-spacing="4">HQ</text>
    </g>
  </g>

  <!-- MIDDLE THIRD: tagline -->
  <g transform="translate(1165 620)">
    <text x="0" y="0" font-family="'Inter', sans-serif" font-weight="500"
          font-size="48" fill="#FAFAF5">STEM explained like you're 5.</text>
    <text x="0" y="72" font-family="'Inter', sans-serif" font-weight="500"
          font-size="48" fill="#FAFAF5">the science is still for grown-ups.</text>
  </g>

  <!-- RIGHT THIRD: cadence callout -->
  <g transform="translate(1830 620)">
    <text x="0" y="0" font-family="'Space Grotesk', sans-serif" font-weight="700"
          font-size="44" fill="#FAFAF5" letter-spacing="2">new drops</text>
    <text x="0" y="80" font-family="'Space Grotesk', sans-serif" font-weight="700"
          font-size="72" fill="#C6F24E" letter-spacing="2">tue <tspan fill="#C6F24E">+</tspan> fri</text>
    <!-- Hazard Pink scribble underline beneath the + character. The + sits
         around x ~80 based on glyph widths; the scribble is a short
         hand-drawn path. -->
    <path d="M 82 100 Q 98 90 114 102 Q 126 110 142 98"
          stroke="#FF2E63" stroke-width="6" fill="none" stroke-linecap="round"/>
  </g>
</svg>
```

- [ ] **Step 3: Render the PNG**

Run: `rsvg-convert -w 2560 -h 1440 assets/social/youtube-banner.svg -o assets/social/youtube-banner.png`

Expected: no errors; PNG created.

- [ ] **Step 4: Verify dimensions**

Run: `file assets/social/youtube-banner.png`
Expected: output contains `2560 x 1440`.

- [ ] **Step 5: Visual check**

Open the SVG in a browser. Confirm:
- Off-Black background full bleed.
- "ELI5 HQ" wordmark legible in left third.
- Tagline two lines in middle.
- "tue + fri" cadence visible in right third with Hazard Pink mark under the `+`.
- Bleed area outside the center is still empty Off-Black (we fill that in Task 5).

If any text overflows or wraps badly, iterate font sizes. Commit once it looks right.

- [ ] **Step 6: Commit**

```bash
git add assets/social/youtube-banner.svg assets/social/youtube-banner.png
git commit -m "$(cat <<'EOF'
Add YouTube banner — safe-zone composition

Off-Black full bleed with wordmark, two-line tagline, and cadence
callout sized to fit inside YouTube's 1546x423 mobile-safe zone. Bleed
stickers added in the next commit.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 5: YouTube banner — bleed-zone pillar stickers

**Files:**
- Modify: `assets/social/youtube-banner.svg`
- Modify: `assets/social/youtube-banner.png`

Adds four sticker-style pillar illustrations in the bleed area around the mobile-safe zone. The Why-does-X pillar (Signal Orange) is deliberately omitted per spec §5.3 — Hazard Pink in the safe zone cannot be co-visible with Signal Orange per bible §3.

- [ ] **Step 1: Define the expected outcome**

Four sticker illustrations appear in the banner's bleed area (outside the center 1855×423 tablet zone so they don't crowd the safe content):
- **Plasma Violet worried-photon blob** (Space pillar) — top-left bleed corner.
- **Lab Lime `52! =` number fragment** (Math pillar) — top-right bleed corner.
- **Beaker Blue sweaty-CPU character** (Tech pillar) — bottom-left bleed corner.
- **Moss Green platypus outline** (Nature pillar) — bottom-right bleed corner.

Each sticker is off-register by 2–3 px (an outline layer offset from a fill layer) for the "drawn on, then stamped" sticker-sheet feel.

- [ ] **Step 2: Add the bleed-sticker block to `assets/social/youtube-banner.svg`**

Open `assets/social/youtube-banner.svg`. Before the closing `</svg>` tag, insert the following block:

```xml
  <!-- BLEED ZONE stickers. Positioned in the four corners outside the
       tablet-safe zone (left edge of tablet-safe = 352, right = 2207,
       top of safe-zone band = 508, bottom = 932). Stickers sit in the
       outer bands. All stickers use a two-layer offset: base fill + a
       slightly-offset outline/shadow for sticker-sheet feel. -->

  <!-- TOP-LEFT: Plasma Violet worried-photon blob (Space pillar) -->
  <g transform="translate(220 220)">
    <!-- shadow layer -->
    <ellipse cx="4" cy="6" rx="110" ry="130" fill="#0E0E12" opacity="0.6"/>
    <!-- body blob -->
    <ellipse cx="0" cy="0" rx="110" ry="130" fill="#7A3BFF"/>
    <!-- eyes -->
    <circle cx="-32" cy="-18" r="11" fill="#0E0E12"/>
    <circle cx="32" cy="-18" r="11" fill="#0E0E12"/>
    <!-- worried mouth -->
    <path d="M -22 32 Q 0 18 22 32" stroke="#0E0E12" stroke-width="5"
          fill="none" stroke-linecap="round"/>
    <!-- sweat bead, slightly off the photon -->
    <path d="M -95 -90 Q -82 -125 -70 -90 Q -70 -78 -82 -74 Q -95 -80 -95 -90 Z"
          fill="#2EC4FF"/>
  </g>

  <!-- TOP-RIGHT: Lab Lime "52! =" number fragment (Math pillar) -->
  <g transform="translate(2340 230)">
    <!-- shadow -->
    <text x="3" y="3" font-family="'Space Grotesk', sans-serif" font-weight="700"
          font-size="160" fill="#0E0E12" opacity="0.6" text-anchor="end">52! =</text>
    <!-- fill -->
    <text x="0" y="0" font-family="'Space Grotesk', sans-serif" font-weight="700"
          font-size="160" fill="#C6F24E" text-anchor="end">52! =</text>
  </g>

  <!-- BOTTOM-LEFT: Beaker Blue sweaty-CPU character (Tech pillar) -->
  <g transform="translate(230 1180)">
    <!-- shadow -->
    <rect x="-95" y="-85" width="190" height="170" rx="18" fill="#0E0E12" opacity="0.6" transform="translate(4 5)"/>
    <!-- body (rounded square) -->
    <rect x="-95" y="-85" width="190" height="170" rx="18" fill="#2EC4FF"/>
    <!-- pin legs: three short bars out each side -->
    <g fill="#2EC4FF">
      <rect x="-110" y="-55" width="15" height="10"/>
      <rect x="-110" y="-20" width="15" height="10"/>
      <rect x="-110" y="15"  width="15" height="10"/>
      <rect x="95"  y="-55" width="15" height="10"/>
      <rect x="95"  y="-20" width="15" height="10"/>
      <rect x="95"  y="15"  width="15" height="10"/>
    </g>
    <!-- eyes -->
    <circle cx="-30" cy="-20" r="10" fill="#0E0E12"/>
    <circle cx="30"  cy="-20" r="10" fill="#0E0E12"/>
    <!-- nervous straight-line mouth -->
    <path d="M -28 30 L 28 30" stroke="#0E0E12" stroke-width="5" stroke-linecap="round"/>
    <!-- sweat bead off the top-right corner -->
    <path d="M 70 -100 Q 80 -125 90 -100 Q 90 -90 80 -86 Q 70 -90 70 -100 Z"
          fill="#FAFAF5"/>
  </g>

  <!-- BOTTOM-RIGHT: Moss Green platypus outline (Nature pillar). Stylized
       side-view silhouette: body + bill + tail + foot. -->
  <g transform="translate(2280 1200)">
    <!-- shadow -->
    <g fill="#0E0E12" opacity="0.6" transform="translate(4 5)">
      <path d="M -120 0 Q -100 -55 -20 -55 Q 60 -55 90 -20 Q 120 -20 140 -10 Q 130 10 95 10 Q 70 35 10 40 Q -80 40 -105 30 Q -125 45 -140 35 Q -130 15 -120 0 Z"/>
    </g>
    <!-- body -->
    <path d="M -120 0 Q -100 -55 -20 -55 Q 60 -55 90 -20 Q 120 -20 140 -10 Q 130 10 95 10 Q 70 35 10 40 Q -80 40 -105 30 Q -125 45 -140 35 Q -130 15 -120 0 Z"
          fill="#4F8A3C"/>
    <!-- bill separator line -->
    <path d="M 88 -18 Q 115 -10 135 -8" stroke="#0E0E12" stroke-width="3" fill="none"/>
    <!-- tiny eye -->
    <circle cx="50" cy="-30" r="5" fill="#0E0E12"/>
    <!-- foot -->
    <path d="M -30 38 L -28 55 L -15 55 L -18 38 Z" fill="#4F8A3C"/>
  </g>
```

- [ ] **Step 3: Re-render the PNG**

Run: `rsvg-convert -w 2560 -h 1440 assets/social/youtube-banner.svg -o assets/social/youtube-banner.png`

- [ ] **Step 4: Visual check**

Open `youtube-banner.svg` in a browser at actual size (`<img src=...>` or the file directly). Confirm:
- Four pillar stickers visible in the four corner bleed areas.
- None of them intrudes into the centered 1546×423 mobile-safe region (imagine a box from x=507 to x=2053, y=508 to y=932 — stickers should be outside).
- Signal Orange is NOT present anywhere on the banner.
- Overall composition reads as one poster, not five separate elements.

Iterate sticker transforms if any one is clipped at the banner edge or too close to the safe zone.

- [ ] **Step 5: Commit**

```bash
git add assets/social/youtube-banner.svg assets/social/youtube-banner.png
git commit -m "$(cat <<'EOF'
Add bleed-zone pillar stickers to YouTube banner

Four sticker illustrations in the bleed corners: Plasma Violet photon
(Space), Lab Lime 52!= fragment (Math), Beaker Blue sweaty-CPU (Tech),
Moss Green platypus outline (Nature). The Why-does-X pillar is
represented editorially in the tagline — Signal Orange is omitted here
to avoid pairing with the Hazard Pink 5 per bible §3.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 6: YouTube watermark (`youtube-watermark.svg` + `.png`)

**Files:**
- Create: `assets/social/youtube-watermark.svg`
- Create: `assets/social/youtube-watermark.png`

- [ ] **Step 1: Define the expected outcome**

A 150×150 SVG with transparent background and only the `5` glyph in Chalk White, filling roughly 80% of the canvas height, with a low-opacity Off-Black drop shadow for legibility against bright video frames. The PNG is rendered at 300×300 (2× for crispness).

- [ ] **Step 2: Write `assets/social/youtube-watermark.svg`**

Create `assets/social/youtube-watermark.svg` with exactly this content:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 150 150" width="150" height="150">
  <title>ELI5 HQ YouTube video watermark — 5 glyph, transparent BG</title>
  <!-- No background — transparent. Glyph source is 1000x1000; scaled
       to ~120px here and centered. The shadow is rendered as a
       duplicate offset copy of the path at 30% opacity rather than an
       SVG filter, because librsvg's filter support is incomplete and
       would break rsvg-convert PNG rendering. -->
  <g transform="translate(15 15) scale(0.12)">
    <path d="M 210 165 L 745 155 L 735 295 L 355 305 L 345 445 Q 505 405 660 455 Q 840 515 855 695 Q 840 870 640 895 Q 430 905 225 830 L 285 695 Q 425 765 575 755 Q 705 745 700 640 Q 695 560 560 555 Q 430 560 335 605 L 200 595 Z"
          fill="#0E0E12" opacity="0.3" transform="translate(18 18)"/>
    <path d="M 210 165 L 745 155 L 735 295 L 355 305 L 345 445 Q 505 405 660 455 Q 840 515 855 695 Q 840 870 640 895 Q 430 905 225 830 L 285 695 Q 425 765 575 755 Q 705 745 700 640 Q 695 560 560 555 Q 430 560 335 605 L 200 595 Z"
          fill="#FAFAF5"/>
  </g>
</svg>
```

- [ ] **Step 3: Render the PNG at 2×**

Run: `rsvg-convert -w 300 -h 300 assets/social/youtube-watermark.svg -o assets/social/youtube-watermark.png`

- [ ] **Step 4: Verify dimensions**

Run: `file assets/social/youtube-watermark.png`
Expected: output contains `300 x 300`.

- [ ] **Step 5: Visual check on two backgrounds**

To confirm legibility, run this ad-hoc test in the browser:

```bash
cat > /tmp/watermark-legibility.html <<'HTML'
<!doctype html>
<html><body style="margin:0">
  <div style="display:flex">
    <div style="background:#0E0E12; padding:40px"><img src="file:///REPLACE_WITH_ABSOLUTE_PATH/assets/social/youtube-watermark.svg" width="150"></div>
    <div style="background:#F5F1E8; padding:40px"><img src="file:///REPLACE_WITH_ABSOLUTE_PATH/assets/social/youtube-watermark.svg" width="150"></div>
    <div style="background:#FF2E63; padding:40px"><img src="file:///REPLACE_WITH_ABSOLUTE_PATH/assets/social/youtube-watermark.svg" width="150"></div>
    <div style="background:#FAFAF5; padding:40px"><img src="file:///REPLACE_WITH_ABSOLUTE_PATH/assets/social/youtube-watermark.svg" width="150"></div>
  </div>
</body></html>
HTML
```

Replace `REPLACE_WITH_ABSOLUTE_PATH` with the absolute path to this repo (run `pwd` to get it), then open `/tmp/watermark-legibility.html` in a browser.

Confirm the watermark reads clearly on all four backgrounds (Off-Black, Paper Cream, Hazard Pink, Chalk White). The white `5` + Off-Black shadow should survive every background. If it vanishes on any (especially Chalk White), increase shadow opacity in the SVG and re-render.

- [ ] **Step 6: Commit**

```bash
git add assets/social/youtube-watermark.svg assets/social/youtube-watermark.png
git commit -m "$(cat <<'EOF'
Add YouTube video watermark — Chalk White 5 with shadow, transparent BG

150x150 source rendered to 300x300 PNG. Off-Black drop-shadow at 30%
opacity provides legibility against both dark and bright video frames —
a narrow, documented exception to the "flat only" rule for this
overlay-only asset per spec §5.4.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 7: Instagram highlight covers (5 files)

**Files:**
- Create: `assets/social/instagram-highlights/why.svg` + `.png`
- Create: `assets/social/instagram-highlights/math.svg` + `.png`
- Create: `assets/social/instagram-highlights/tech.svg` + `.png`
- Create: `assets/social/instagram-highlights/nature.svg` + `.png`
- Create: `assets/social/instagram-highlights/space.svg` + `.png`

- [ ] **Step 1: Define the expected outcome**

Five 1080×1920 PNG-ready covers, identical structure with pillar-color background, the pillar word in Space Grotesk 700 Off-Black centered on the canvas, and a 180 px × 4 px Hazard Pink bar centered directly below. Font size is **fixed at 140 pt across all five** so the longest word (NATURE) fits inside the ~500 px icon-safe square while WHY/TECH/MATH/SPACE share the same size for cohesion. Underline bar is the **same width on every cover** regardless of word length — consistency beats word-fit width-matching for a highlight set.

Canvas center is (540, 960). Text baseline at y=1010 and underline at y=1035 produces a visual block that sits roughly centered on 960 with enough margin for Instagram's circle crop.

- [ ] **Step 2: Write `assets/social/instagram-highlights/why.svg`**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1080 1920" width="1080" height="1920">
  <title>Instagram highlight cover — Why does X do that?</title>
  <rect width="1080" height="1920" fill="#FF8A1F"/>
  <text x="540" y="1010" font-family="'Space Grotesk', sans-serif" font-weight="700"
        font-size="140" fill="#0E0E12" text-anchor="middle" letter-spacing="-4">WHY</text>
  <!-- Hazard Pink signature bar -->
  <rect x="450" y="1035" width="180" height="4" fill="#FF2E63"/>
</svg>
```

- [ ] **Step 3: Write `assets/social/instagram-highlights/math.svg`**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1080 1920" width="1080" height="1920">
  <title>Instagram highlight cover — The math is insane</title>
  <rect width="1080" height="1920" fill="#C6F24E"/>
  <text x="540" y="1010" font-family="'Space Grotesk', sans-serif" font-weight="700"
        font-size="140" fill="#0E0E12" text-anchor="middle" letter-spacing="-4">MATH</text>
  <rect x="450" y="1035" width="180" height="4" fill="#FF2E63"/>
</svg>
```

- [ ] **Step 4: Write `assets/social/instagram-highlights/tech.svg`**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1080 1920" width="1080" height="1920">
  <title>Instagram highlight cover — Tech, explained dumb</title>
  <rect width="1080" height="1920" fill="#2EC4FF"/>
  <text x="540" y="1010" font-family="'Space Grotesk', sans-serif" font-weight="700"
        font-size="140" fill="#0E0E12" text-anchor="middle" letter-spacing="-4">TECH</text>
  <rect x="450" y="1035" width="180" height="4" fill="#FF2E63"/>
</svg>
```

- [ ] **Step 5: Write `assets/social/instagram-highlights/nature.svg`**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1080 1920" width="1080" height="1920">
  <title>Instagram highlight cover — Nature is unhinged</title>
  <rect width="1080" height="1920" fill="#4F8A3C"/>
  <text x="540" y="1010" font-family="'Space Grotesk', sans-serif" font-weight="700"
        font-size="140" fill="#0E0E12" text-anchor="middle" letter-spacing="-4">NATURE</text>
  <rect x="450" y="1035" width="180" height="4" fill="#FF2E63"/>
</svg>
```

- [ ] **Step 6: Write `assets/social/instagram-highlights/space.svg`**

The SPACE cover uses Off-Black text on Plasma Violet — contrast is ~3.9:1, below normal-text AA but passing large-text AA (3:1) at this word size. Do not reduce the font size below 140 or contrast protection degrades.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1080 1920" width="1080" height="1920">
  <title>Instagram highlight cover — Space stuff</title>
  <rect width="1080" height="1920" fill="#7A3BFF"/>
  <text x="540" y="1010" font-family="'Space Grotesk', sans-serif" font-weight="700"
        font-size="140" fill="#0E0E12" text-anchor="middle" letter-spacing="-4">SPACE</text>
  <rect x="450" y="1035" width="180" height="4" fill="#FF2E63"/>
</svg>
```

- [ ] **Step 7: Render all five PNGs**

Run from repo root:

```bash
cd assets/social/instagram-highlights
for f in why math tech nature space; do
  rsvg-convert -w 1080 -h 1920 "$f.svg" -o "$f.png"
done
cd ../../..
```

Expected: five PNG files, each 1080×1920.

Verify: `for f in assets/social/instagram-highlights/*.png; do file "$f"; done`

All five should report `1080 x 1920`.

- [ ] **Step 8: Visual check**

Open each SVG in a browser. Confirm:
- Pillar-color background full bleed.
- Word is centered vertically and horizontally.
- Word fits comfortably within the ~500×500 icon-safe square (imagine a circle inscribed at 38% radius from center — the word should not touch the circle edge).
- Hazard Pink underscore bar is visible directly under the word, centered, and roughly the width of the word.
- Contrast reads well; on the SPACE cover confirm the word remains legible (it's at the AA edge but passes for large text).

If "NATURE" overflows the icon-safe square, reduce the font-size uniformly across **all five** files — never let one cover differ.

- [ ] **Step 9: Commit**

```bash
git add assets/social/instagram-highlights/
git commit -m "$(cat <<'EOF'
Add 5 Instagram highlight covers (one per pillar)

1080x1920 covers with pillar word in Space Grotesk 700 Off-Black,
pillar-color background, Hazard Pink underscore bar beneath.
Consistent font-size across all five so the set reads as cohesive.
SPACE cover documented as large-text AA per spec §5.5.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 8: Platform setup runbook (`platform-setup.md`)

**Files:**
- Create: `platform-setup.md` (at repo root)

- [ ] **Step 1: Define the expected outcome**

A single markdown file at repo root containing everything needed to register and configure `@eli5hq` on TikTok, Instagram, and YouTube. Every paste block is verbatim from `brand-bible.md` §5. The file follows the runbook structure in spec §6.

- [ ] **Step 2: Create `platform-setup.md`**

Create `platform-setup.md` at repo root with exactly this content:

````markdown
# ELI5 HQ — Platform Setup Runbook

Top-to-bottom checklist for standing up `@eli5hq` on TikTok, Instagram,
and YouTube on launch day. Copy blocks are duplicated verbatim from
`brand-bible.md` §5 — the bible is the source of truth; if §5 changes,
update this file in the same commit.

## Before you start (10 min)

- [ ] Password manager ready; email inbox accessible.
- [ ] 2FA app ready (Authy, 1Password, or similar).
- [ ] **Handle fallback rule:** if `@eli5hq` is taken on any one platform,
      drop **all three** platforms to the next fallback rather than using
      per-platform optimization. Fallback ladder:
  1. `@eli5hq`
  2. `@eli5.hq`
  3. `@eli5hq.official`
  4. `@eli5_hq`
  5. `@geteli5hq`
  6. `@eli5hqtv`

## Pre-register defensive claims (bible §5)

Claim these handles on day one. Do not populate them — the goal is
ownership, not activity.

- [ ] Threads — `@eli5hq`
- [ ] X (Twitter) — `@eli5hq`
- [ ] BlueSky — `@eli5hq`
- [ ] Pinterest — `@eli5hq`
- [ ] Snapchat — `@eli5hq`
- [ ] Reddit — `u/eli5hq` (user) + `r/eli5hq` (dormant claim)
- [ ] Domain — `eli5hq.com`
- [ ] Domain — `eli5hq.co`

---

## TikTok

- **Display name:** `ELI5 HQ`
- **Handle:** `@eli5hq`
- **Avatar:** upload `assets/social/avatar.png`
- **Bio (paste exactly — 64 chars):**

  ```
  STEM explained like you're 5. the science is still for grown-ups. 🧪
  ```

- **Link:** (placeholder — add linktree/linkin.bio aggregator URL once it
  exists; leave empty on day one)
- **Category:** Education → Science
- **Post-launch note:** TikTok allows 3 pinned videos. Do not pin
  anything until 3 videos have shipped. Strategy per bible §5 is to pin
  one Nature-is-unhinged, one Math-is-insane, and one channel intro.

## Instagram

- **Display name:** `ELI5 HQ`
- **Handle:** `@eli5hq`
- **Avatar:** upload `assets/social/avatar.png`
- **Bio (paste exactly — 133 chars, preserve the line breaks):**

  ```
  STEM, explained like you're 5. the answer is never baby.
  chaotic-smart animated shorts. new drops tue + fri.
  tap → everything else ↓
  ```

- **Link:** (placeholder — linktree/linkin.bio)
- **Category:** Education
- **Highlight covers:**
  - `assets/social/instagram-highlights/why.png` — Why does X do that?
  - `assets/social/instagram-highlights/math.png` — The math is insane
  - `assets/social/instagram-highlights/tech.png` — Tech, explained dumb
  - `assets/social/instagram-highlights/nature.png` — Nature is unhinged
  - `assets/social/instagram-highlights/space.png` — Space stuff

  **Don't create a highlight until 2+ videos exist in that pillar.**
  Empty highlights age a profile fast (bible §5 note).

## YouTube

- **Channel name:** `ELI5 HQ`
- **Handle:** `@eli5hq`
- **Avatar:** upload `assets/social/avatar.png`
- **Channel banner:** upload `assets/social/youtube-banner.png`
- **Video watermark:** YouTube Studio → Customization → Branding →
  upload `assets/social/youtube-watermark.png`. Set display time to
  **"Entire video"**.
- **Short bio / banner tagline (paste exactly — 66 chars):**

  ```
  STEM, explained like you're 5. the science is still for grown-ups.
  ```

- **About description (paste exactly — ~370 chars, preserve line breaks):**

  ```
  ELI5 HQ is the STEM channel that explains real science like you're 5,
  then actually teaches it. Short-form animated shorts for the curious, the
  chronically online, and the allergic-to-lectures.

  Pillars: why does X do that · the math is insane · tech explained dumb ·
  nature is unhinged · space stuff.

  New videos every Tuesday and Friday. No talking heads. No politics. Just
  the good stuff, fast.
  ```

- **Links:**
  - Primary link: (placeholder — TBD)
  - TikTok: `https://tiktok.com/@eli5hq`
  - Instagram: `https://instagram.com/eli5hq`
- **Category:** Education
- **Featured video:** (placeholder — replace with intro-short URL once
  produced; bible §5 requires reshoot every 90 days).

---

## Post-launch verification

- [ ] TikTok avatar renders uncropped in feed.
- [ ] Instagram avatar renders uncropped in feed.
- [ ] YouTube avatar renders uncropped in feed.
- [ ] YouTube banner displays with no text cut off at mobile view
      (open your phone's YouTube app and navigate to the channel page).
- [ ] YouTube video watermark appears on a test upload at the corner,
      display time "Entire video" confirmed in Studio.
- [ ] TikTok bio displays with the 🧪 emoji and correct line count.
- [ ] Instagram bio shows all three lines in order.
- [ ] YouTube short bio and About description render without Markdown
      bleed-through (no stray asterisks or backticks visible).
- [ ] Handle is identical across all three platforms (same fallback tier
      if `@eli5hq` was unavailable anywhere).
````

- [ ] **Step 3: Verify against bible §5**

Run: `diff <(grep -A2 '^STEM explained' brand-bible.md | head -3) <(grep -A2 '^  STEM explained' platform-setup.md | head -3)`

Expected: either empty output or only whitespace-indent differences. The wording of the TikTok bio must match `brand-bible.md` §5 verbatim.

Eyeball every paste block against bible §5 and confirm character-for-character identity.

- [ ] **Step 4: Commit**

```bash
git add platform-setup.md
git commit -m "$(cat <<'EOF'
Add platform-setup.md launch runbook

Top-to-bottom checklist for registering @eli5hq on TikTok, Instagram,
and YouTube. Every paste block is verbatim from brand-bible.md §5;
asset paths point at assets/social/ files.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 9: Add `#channel-kit` section to `index.html`

**Files:**
- Modify: `index.html` — insert new section between `#logo` (ends with `</div></section>` currently near line 412) and the zigzag divider preceding `#hooks`.

- [ ] **Step 1: Define the expected outcome**

A new `<section id="channel-kit">` is inserted into `index.html` following the pattern used by existing sections (`py-16`, `max-w-6xl` container, kicker + `h2.heading-wonk` heading). It renders:
- Avatar (primary and alt) as round 240 px previews.
- YouTube banner at full container width with safe-zone and tablet-zone boxes drawn as HTML overlays on top of the `<img>`.
- YouTube watermark on a two-tile background (Off-Black + Paper Cream) to prove legibility.
- Instagram highlights as a row of five circle thumbnails with pillar captions.
- A link-out card pointing to `platform-setup.md`.

Each asset group has download links to both the `.svg` and `.png`.

- [ ] **Step 2: Locate the insertion point**

Run: `grep -n 'id="logo"\|id="hooks"' index.html`

Expected output shows two lines — one for the logo section open tag and one for the hooks section open tag. The zigzag divider between them is the insertion anchor.

Find the line `<div class="zigzag my-8" aria-hidden="true"></div>` that sits between `</div></section>` (closing `#logo`) and `<section id="hooks" ...>` (opening `#hooks`). The new section gets inserted **immediately after** that zigzag divider and followed by **another zigzag divider** before `#hooks`.

- [ ] **Step 3: Insert the new section**

Between the existing zigzag (after `#logo`) and the `<section id="hooks"` open tag, insert:

```html
      <section id="channel-kit" class="py-16"><div class="mx-auto max-w-6xl">
  <div class="mb-8 flex items-baseline gap-4">
    <h2 class="heading-wonk font-display text-4xl font-bold text-chalk-white md:text-5xl">
      Channel kit
    </h2>
    <span class="font-display text-xs font-bold uppercase tracking-[0.2em] text-lab-lime">
      applied identity · launch-day files
    </span>
  </div>

  <p class="mb-12 max-w-3xl font-body text-base text-smoke-200">
    Every file <code class="bg-smoke-900 px-2 py-0.5 text-chalk-white">@eli5hq</code>
    needs on day one — avatars, YouTube banner, video watermark, Instagram
    highlight covers, and the setup runbook. SVGs are the source; PNGs are
    rendered for upload.
  </p>

  <!-- Avatars -->
  <div class="mb-12">
    <p class="mb-4 font-display text-xs font-bold uppercase tracking-widest text-smoke-500">Avatar</p>
    <div class="grid gap-6 md:grid-cols-2">
      <div class="border border-smoke-900 bg-smoke-900 p-8 flex flex-col items-center text-center">
        <img src="assets/social/avatar.svg" alt="ELI5 HQ primary avatar — Off-Black 5 on Hazard Pink tile" class="h-60 w-60 rounded-full" />
        <p class="mt-6 font-display text-xs font-bold uppercase tracking-widest text-chalk-white">Primary</p>
        <p class="mt-2 font-body text-sm text-smoke-200">TikTok · Instagram · YouTube</p>
        <p class="mt-4 font-body text-xs text-smoke-500">
          <a href="assets/social/avatar.svg" download class="text-hazard-pink underline">.svg</a>
          <span class="mx-2">·</span>
          <a href="assets/social/avatar.png" download class="text-hazard-pink underline">.png</a>
        </p>
      </div>
      <div class="border border-smoke-900 bg-smoke-900 p-8 flex flex-col items-center text-center">
        <img src="assets/social/avatar-alt.svg" alt="ELI5 HQ alternate avatar — Hazard Pink 5 on Off-Black tile" class="h-60 w-60 rounded-full" />
        <p class="mt-6 font-display text-xs font-bold uppercase tracking-widest text-chalk-white">Alt · stories & events</p>
        <p class="mt-2 font-body text-sm text-smoke-200">Pinned stories only</p>
        <p class="mt-4 font-body text-xs text-smoke-500">
          <a href="assets/social/avatar-alt.svg" download class="text-hazard-pink underline">.svg</a>
          <span class="mx-2">·</span>
          <a href="assets/social/avatar-alt.png" download class="text-hazard-pink underline">.png</a>
        </p>
      </div>
    </div>
  </div>

  <!-- YouTube banner -->
  <div class="mb-12">
    <p class="mb-4 font-display text-xs font-bold uppercase tracking-widest text-smoke-500">YouTube banner · 2560×1440</p>
    <div class="border border-smoke-900 bg-smoke-900 p-4">
      <div class="relative w-full" style="aspect-ratio: 2560 / 1440;">
        <img src="assets/social/youtube-banner.svg" alt="ELI5 HQ YouTube channel banner" class="absolute inset-0 h-full w-full" />
        <!-- Tablet-safe guide: 1855x423 centered = 27.5% from each side, 35.2% from top/bottom -->
        <div class="pointer-events-none absolute border-2 border-dashed border-hazard-pink/60"
             style="top: 35.2%; left: 13.75%; width: 72.5%; height: 29.4%;"></div>
        <!-- Mobile-safe guide: 1546x423 centered -->
        <div class="pointer-events-none absolute border-2 border-hazard-pink"
             style="top: 35.2%; left: 19.8%; width: 60.4%; height: 29.4%;"></div>
      </div>
    </div>
    <p class="mt-3 font-body text-xs text-smoke-500">
      <span class="mr-3"><span class="inline-block h-3 w-3 align-middle border-2 border-hazard-pink"></span> mobile-safe 1546×423</span>
      <span><span class="inline-block h-3 w-3 align-middle border-2 border-dashed border-hazard-pink/60"></span> tablet-safe 1855×423</span>
    </p>
    <p class="mt-3 font-body text-xs text-smoke-500">
      <a href="assets/social/youtube-banner.svg" download class="text-hazard-pink underline">.svg</a>
      <span class="mx-2">·</span>
      <a href="assets/social/youtube-banner.png" download class="text-hazard-pink underline">.png</a>
    </p>
  </div>

  <!-- Watermark -->
  <div class="mb-12">
    <p class="mb-4 font-display text-xs font-bold uppercase tracking-widest text-smoke-500">YouTube video watermark · 150×150</p>
    <div class="grid gap-6 md:grid-cols-2">
      <div class="flex items-center justify-center border border-smoke-900 bg-off-black p-10">
        <img src="assets/social/youtube-watermark.svg" alt="Watermark on dark video frame" class="h-[120px] w-[120px]" />
      </div>
      <div class="flex items-center justify-center border border-smoke-900 p-10" style="background: #F5F1E8;">
        <img src="assets/social/youtube-watermark.svg" alt="Watermark on light video frame" class="h-[120px] w-[120px]" />
      </div>
    </div>
    <p class="mt-3 font-body text-xs text-smoke-500">
      Display time in YouTube Studio: <strong class="text-chalk-white">entire video</strong>.
      <a href="assets/social/youtube-watermark.svg" download class="ml-4 text-hazard-pink underline">.svg</a>
      <span class="mx-2">·</span>
      <a href="assets/social/youtube-watermark.png" download class="text-hazard-pink underline">.png</a>
    </p>
  </div>

  <!-- Instagram highlights -->
  <div class="mb-12">
    <p class="mb-4 font-display text-xs font-bold uppercase tracking-widest text-smoke-500">Instagram highlight covers · 1080×1920</p>
    <div class="border border-smoke-900 bg-smoke-900 p-8">
      <div class="flex flex-wrap gap-6 md:justify-between">
        <div class="text-center">
          <img src="assets/social/instagram-highlights/why.svg" alt="Highlight cover — Why does X do that?" class="h-32 w-32 rounded-full border-4 border-chalk-white" />
          <p class="mt-2 font-body text-xs text-smoke-200">Why does X</p>
        </div>
        <div class="text-center">
          <img src="assets/social/instagram-highlights/math.svg" alt="Highlight cover — The math is insane" class="h-32 w-32 rounded-full border-4 border-chalk-white" />
          <p class="mt-2 font-body text-xs text-smoke-200">Math is insane</p>
        </div>
        <div class="text-center">
          <img src="assets/social/instagram-highlights/tech.svg" alt="Highlight cover — Tech explained dumb" class="h-32 w-32 rounded-full border-4 border-chalk-white" />
          <p class="mt-2 font-body text-xs text-smoke-200">Tech explained</p>
        </div>
        <div class="text-center">
          <img src="assets/social/instagram-highlights/nature.svg" alt="Highlight cover — Nature is unhinged" class="h-32 w-32 rounded-full border-4 border-chalk-white" />
          <p class="mt-2 font-body text-xs text-smoke-200">Nature unhinged</p>
        </div>
        <div class="text-center">
          <img src="assets/social/instagram-highlights/space.svg" alt="Highlight cover — Space stuff" class="h-32 w-32 rounded-full border-4 border-chalk-white" />
          <p class="mt-2 font-body text-xs text-smoke-200">Space stuff</p>
        </div>
      </div>
    </div>
    <p class="mt-3 font-body text-xs text-smoke-500">
      Downloads:
      <a href="assets/social/instagram-highlights/why.svg" download class="text-hazard-pink underline">why</a>
      <span class="mx-1">·</span>
      <a href="assets/social/instagram-highlights/math.svg" download class="text-hazard-pink underline">math</a>
      <span class="mx-1">·</span>
      <a href="assets/social/instagram-highlights/tech.svg" download class="text-hazard-pink underline">tech</a>
      <span class="mx-1">·</span>
      <a href="assets/social/instagram-highlights/nature.svg" download class="text-hazard-pink underline">nature</a>
      <span class="mx-1">·</span>
      <a href="assets/social/instagram-highlights/space.svg" download class="text-hazard-pink underline">space</a>
    </p>
  </div>

  <!-- Runbook callout -->
  <div class="border-2 border-hazard-pink bg-smoke-900 p-6">
    <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">Launch runbook</p>
    <p class="mt-2 font-body text-base text-chalk-white">
      Paste-ready bios, asset paths, and a post-launch verification checklist.
    </p>
    <p class="mt-3">
      <a href="platform-setup.md" class="font-display text-sm font-bold uppercase tracking-widest text-hazard-pink underline">
        platform-setup.md →
      </a>
    </p>
  </div>
</div></section>
      <div class="zigzag my-8" aria-hidden="true"></div>
```

- [ ] **Step 4: Verify the markup parses and the page renders**

Open `index.html` in a browser (drag-drop or `open index.html` on macOS). Confirm:
- The page loads without any console errors (open DevTools).
- Scrolling past the `#logo` section, the `Channel kit` section is visible with the heading in the expected style.
- All five asset groups render — avatars, banner with visible pink safe-zone boxes, watermark on two backgrounds, highlights row, and the runbook callout.
- All download links are clickable and point at files that exist.
- The `#hooks` section follows after with its own zigzag divider.

If `<img>` tags show broken-image icons, check relative paths match the repo layout (`assets/social/...` from `index.html` at repo root).

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Add Channel Kit section to style guide

Inserts #channel-kit between #logo and #hooks. Renders avatars, YouTube
banner with mobile/tablet safe-zone overlays drawn as HTML guides (not
baked into the SVG), watermark on two backgrounds, Instagram highlight
covers as a row of circles, and a link-out to platform-setup.md.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 10: Final verification

**Files:**
- None created; verifies prior work.

- [ ] **Step 1: File-count check**

Run: `find assets/social -type f | sort`

Expected output (19 lines):

```
assets/social/README.md
assets/social/avatar-alt.png
assets/social/avatar-alt.svg
assets/social/avatar.png
assets/social/avatar.svg
assets/social/instagram-highlights/math.png
assets/social/instagram-highlights/math.svg
assets/social/instagram-highlights/nature.png
assets/social/instagram-highlights/nature.svg
assets/social/instagram-highlights/space.png
assets/social/instagram-highlights/space.svg
assets/social/instagram-highlights/tech.png
assets/social/instagram-highlights/tech.svg
assets/social/instagram-highlights/why.png
assets/social/instagram-highlights/why.svg
assets/social/youtube-banner.png
assets/social/youtube-banner.svg
assets/social/youtube-watermark.png
assets/social/youtube-watermark.svg
```

If any file is missing, return to its task.

- [ ] **Step 2: PNG dimension check**

Run:

```bash
for f in \
  assets/social/avatar.png:1024x1024 \
  assets/social/avatar-alt.png:1024x1024 \
  assets/social/youtube-banner.png:2560x1440 \
  assets/social/youtube-watermark.png:300x300 \
  assets/social/instagram-highlights/why.png:1080x1920 \
  assets/social/instagram-highlights/math.png:1080x1920 \
  assets/social/instagram-highlights/tech.png:1080x1920 \
  assets/social/instagram-highlights/nature.png:1080x1920 \
  assets/social/instagram-highlights/space.png:1080x1920; do
  path="${f%:*}"; want="${f##*:}"
  got=$(file "$path" | grep -oE '[0-9]+ x [0-9]+' | tr -d ' ' | tr 'x' 'x')
  if [ "$got" = "$want" ]; then echo "OK  $path ($got)"; else echo "FAIL $path got $got want $want"; fi
done
```

Expected: 9 `OK` lines, no `FAIL`.

- [ ] **Step 3: Contrast sanity check**

No automated tool here — visual confirmation:
- Open each Instagram highlight SVG in a browser. Confirm the word is
  clearly legible against its background.
- The SPACE cover (Off-Black on Plasma Violet, 3.9:1) is the tightest
  case. If it feels borderline, **do not reduce font size** — instead,
  re-check at the viewing size (it will be displayed as a small circle
  on Instagram, where the word is large relative to its container).

- [ ] **Step 4: Lighthouse ≥95 on `index.html`**

Per CLAUDE.md, the accessibility gate is Lighthouse ≥95. If Lighthouse isn't available locally:

- Option A: Open `index.html` in Chrome, open DevTools → Lighthouse tab, run with Accessibility category. Confirm score ≥95. Fix any reported issues (most commonly: missing alt text, low contrast, missing labels).
- Option B: If Lighthouse isn't available locally, name this as the user's manual verification step and move on.

- [ ] **Step 5: Dead-link check on `index.html`**

Run:

```bash
# Extract hrefs and srcs, check they resolve as files
grep -oE '(href|src)="[^"]+"' index.html \
  | sed -E 's/.*="([^"]+)".*/\1/' \
  | grep -vE '^(https?:|#|data:|mailto:)' \
  | while read path; do
      [ -f "$path" ] && echo "OK  $path" || echo "FAIL $path"
    done
```

Expected: every `OK` for paths that belong to the `assets/social/` tree and `platform-setup.md`. External URLs (Google Fonts, Tailwind CDN) are filtered out.

- [ ] **Step 6: Runbook sanity — paths resolve**

Run:

```bash
grep -oE 'assets/social[^[:space:]`]+\.png' platform-setup.md \
  | sort -u \
  | while read path; do
      [ -f "$path" ] && echo "OK  $path" || echo "FAIL $path"
    done
```

Expected: every path referenced in the runbook exists on disk.

- [ ] **Step 7: Git state check**

Run: `git status && git log --oneline -15`

Expected: clean working tree; at least 9 new commits (one per asset-producing task + the index.html commit + the README commit).

- [ ] **Step 8: Open items for follow-up**

Per spec §11, these stay open after this plan finishes. Note them back to the user if any are still unresolved:

- Final visual refinement of the hand-scrawled `5` glyph path. This plan commits a working draft; if the user wants iteration, open a follow-up ticket.
- Final refinement of the four banner-bleed stickers.
- GitHub Pages link behavior for the `platform-setup.md` link (relative vs absolute blob URL). If the deployed link at `https://eli5hq.github.io/brand/#channel-kit` opens `platform-setup.md` as raw Markdown rather than rendered, consider switching the link to the GitHub blob URL.

---

## Done-done definition

- All 19 files in `assets/social/` exist and PNG dimensions match §5 of the spec.
- `platform-setup.md` at repo root, all paste blocks verbatim from bible §5.
- `index.html` has a working `#channel-kit` section, passes Lighthouse ≥95, all links resolve.
- Every asset-producing task was committed individually (no megacommits).
- A fresh reader of `platform-setup.md` can register all three channels with the files in this repo and nothing else.

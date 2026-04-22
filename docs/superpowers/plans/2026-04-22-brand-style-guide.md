# ELI5 HQ — Brand Style Guide (HTML) Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Ship a single-page static HTML brand style guide at the repo root that renders the bible's visual and reference content in-brand, deployed via GitHub Pages at `https://eli5hq.github.io/brand/`.

**Architecture:** One `index.html` file at the repo root. Tailwind CSS via CDN with inline config extending the theme with brand tokens. Google Fonts for Space Grotesk + Inter. No JavaScript beyond the Tailwind CDN script. Custom CSS in a `<style>` block for hand-touched/zine effects (rotations, off-register shadows, noise overlay, zigzag dividers). All work lands directly on `develop`; `main` goes live via the existing develop→main PR flow.

**Tech Stack:** HTML5, Tailwind CSS (CDN), Google Fonts (Space Grotesk, Inter), inline SVG for dividers and decorative accents. No build step, no dependencies.

**Testing approach:** Static HTML with no business logic → automated tests would be theater. Verification is **(a)** open-in-browser visual inspection after each section and **(b)** Chrome DevTools Lighthouse accessibility audit at the end (must score ≥ 95). Each section's task ends with a browser-verification step before commit.

**Spec:** `docs/superpowers/specs/2026-04-22-brand-style-guide-design.md`

**Source of truth for content:** `brand-bible.md`

---

## Working Conventions

- Work directly on the `develop` branch. One commit per task.
- Use absolute paths in the repo: everything is relative to `/Users/sharmeyer/Development/eli5hq/brand/`.
- To view the page during development: `open /Users/sharmeyer/Development/eli5hq/brand/index.html` (opens via `file://` in the default browser). No server needed — Tailwind CDN and Google Fonts work over `file://`.
- Commit message style: match the existing commit (see `git log -1`). Use present-tense verb, no prefix required, co-authored footer for Claude commits.
- Push after each commit: `git push origin develop`. That's optional per task, but recommended at Task 5 and Task 12 at minimum.

---

## Task 1: Scaffold `index.html` with semantic skeleton, Tailwind CDN, and brand theme tokens

**Files:**
- Create: `/Users/sharmeyer/Development/eli5hq/brand/index.html`

**Purpose:** Lay down the document shell — head, fonts, Tailwind CDN, Tailwind config extending the theme with exact brand tokens, empty `<main>` with stub sections, and the custom-CSS `<style>` block scaffold.

- [ ] **Step 1: Write `index.html` with full scaffold**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>ELI5 HQ — Brand Style Guide v1.0</title>
    <meta name="description" content="Internal brand reference for ELI5 HQ. Colors, typography, voice, editorial formula, and pre-publish checklist." />

    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Space+Grotesk:wght@500;700&display=swap"
      rel="stylesheet"
    />

    <script src="https://cdn.tailwindcss.com"></script>
    <script>
      tailwind.config = {
        theme: {
          extend: {
            colors: {
              "hazard-pink": "#FF2E63",
              "off-black": "#0E0E12",
              "paper-cream": "#F5F1E8",
              "lab-lime": "#C6F24E",
              "plasma-violet": "#7A3BFF",
              "signal-orange": "#FF8A1F",
              "beaker-blue": "#2EC4FF",
              "moss-green": "#4F8A3C",
              "smoke-900": "#1A1A20",
              "smoke-500": "#6B6B78",
              "smoke-200": "#C9C6BD",
              "chalk-white": "#FAFAF5",
            },
            fontFamily: {
              display: ['"Space Grotesk"', "system-ui", "sans-serif"],
              body: ['"Inter"', "system-ui", "sans-serif"],
            },
          },
        },
      };
    </script>

    <style>
      /* Noise overlay — subtle paper-grain feel */
      body::before {
        content: "";
        position: fixed;
        inset: 0;
        pointer-events: none;
        z-index: 1;
        opacity: 0.07;
        background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='240' height='240'><filter id='n'><feTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='2' stitchTiles='stitch'/><feColorMatrix values='0 0 0 0 1  0 0 0 0 1  0 0 0 0 1  0 0 0 0.6 0'/></filter><rect width='100%' height='100%' filter='url(%23n)'/></svg>");
      }

      /* Sticker-style card — slight tilt + off-register shadow */
      .sticker {
        position: relative;
        transform: rotate(-0.75deg);
      }
      .sticker::after {
        content: "";
        position: absolute;
        inset: 0;
        z-index: -1;
        background: #7a3bff; /* plasma-violet default */
        transform: translate(4px, 4px);
      }
      .sticker--tilt-pos {
        transform: rotate(1.25deg);
      }
      .sticker--shadow-pink::after {
        background: #ff2e63;
      }
      .sticker--shadow-lime::after {
        background: #c6f24e;
      }

      /* Rotated headings for zine energy */
      .heading-wonk {
        display: inline-block;
        transform: rotate(-1.25deg);
      }

      /* Zigzag divider (inline SVG background) */
      .zigzag {
        height: 12px;
        background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='40' height='12' viewBox='0 0 40 12'><path d='M0 6 L10 1 L20 11 L30 1 L40 6' fill='none' stroke='%23FF2E63' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'/></svg>");
        background-repeat: repeat-x;
        background-position: center;
      }

      /* Offset color fill — one rule, applied to elements that want the
         "sticker is slightly off the outline" look */
      .offset-fill {
        position: relative;
      }
      .offset-fill > span {
        position: relative;
        z-index: 1;
      }
      .offset-fill::before {
        content: "";
        position: absolute;
        inset: 0;
        background: inherit;
        transform: translate(3px, 3px);
        z-index: 0;
      }

      /* Ensure content sits above the noise overlay */
      main,
      header,
      footer {
        position: relative;
        z-index: 2;
      }
    </style>
  </head>

  <body class="bg-off-black font-body text-chalk-white antialiased">
    <header id="hero" class="px-6 py-20 md:px-12 md:py-28">
      <!-- Task 2 fills this -->
    </header>

    <main class="px-6 md:px-12">
      <section id="foundation" class="py-16"><!-- Task 2 --></section>
      <div class="zigzag my-8" aria-hidden="true"></div>

      <section id="palette" class="py-16"><!-- Task 3 --></section>
      <div class="zigzag my-8" aria-hidden="true"></div>

      <section id="typography" class="py-16"><!-- Task 4 --></section>
      <div class="zigzag my-8" aria-hidden="true"></div>

      <section id="logo" class="py-16"><!-- Task 5 --></section>
      <div class="zigzag my-8" aria-hidden="true"></div>

      <section id="hooks" class="py-16"><!-- Task 6 --></section>
      <div class="zigzag my-8" aria-hidden="true"></div>

      <section id="do-dont" class="py-16"><!-- Task 7 --></section>
      <div class="zigzag my-8" aria-hidden="true"></div>

      <section id="checklist" class="py-16"><!-- Task 8 --></section>
      <div class="zigzag my-8" aria-hidden="true"></div>

      <section id="four-beat" class="py-16"><!-- Task 9 --></section>
    </main>

    <footer class="mt-16 border-t border-smoke-900 px-6 py-10 text-smoke-500 md:px-12">
      <!-- Task 10 fills this -->
      <p class="font-body text-sm">scaffolding — sections wired in subsequent tasks.</p>
    </footer>
  </body>
</html>
```

- [ ] **Step 2: Open in browser and verify**

Run: `open /Users/sharmeyer/Development/eli5hq/brand/index.html`

Expected: a mostly-empty off-black page with zigzag dividers visible between empty sections. Fonts loaded (check DevTools → Network → filter "font" → should see Inter and Space Grotesk 200/OK). No console errors other than the known Tailwind CDN "production" warning.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Scaffold index.html style guide with Tailwind theme tokens

Adds the document shell: Google Fonts for Space Grotesk + Inter,
Tailwind via CDN with an inline config extending the theme with every
brand color and the display/body font families, empty section stubs
matching the spec, and a custom-CSS block with the noise overlay,
sticker-card tilt/shadow utilities, and the zigzag divider pattern.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 2: Hero masthead + foundation strip

**Files:**
- Modify: `/Users/sharmeyer/Development/eli5hq/brand/index.html` (`<header id="hero">` and `<section id="foundation">`)

**Purpose:** Oversized `ELI5 HQ` wordmark with `5` as hero character, tagline, version badge. Foundation strip with positioning one-liner + engine quote.

- [ ] **Step 1: Fill the `<header id="hero">` block**

Replace the `<!-- Task 2 fills this -->` inside `<header id="hero">` with:

```html
<div class="mx-auto max-w-6xl">
  <div class="mb-8 inline-block">
    <span class="sticker sticker--tilt-pos sticker--shadow-lime inline-block bg-hazard-pink px-3 py-1 font-display text-xs font-bold uppercase tracking-wider text-off-black">
      v1.0 · Brand Guide
    </span>
  </div>
  <h1 class="font-display font-bold leading-[0.85] tracking-tight">
    <span class="block text-chalk-white" style="font-size: clamp(64px, 14vw, 220px);">
      ELI<span class="text-hazard-pink">5</span>
    </span>
    <span class="mt-2 block text-smoke-500" style="font-size: clamp(28px, 5vw, 72px);">
      HQ
    </span>
  </h1>
  <p class="mt-10 max-w-2xl font-body text-lg text-smoke-200 md:text-xl">
    STEM explained like you're 5. The science is still for grown-ups.
  </p>
</div>
```

- [ ] **Step 2: Fill the `<section id="foundation">` block**

Replace the `<!-- Task 2 -->` inside `<section id="foundation">` with:

```html
<div class="mx-auto max-w-6xl">
  <div class="mb-6 inline-block">
    <span class="heading-wonk font-display text-xs font-bold uppercase tracking-[0.2em] text-hazard-pink">
      Foundation
    </span>
  </div>
  <div class="grid gap-8 md:grid-cols-5">
    <blockquote class="md:col-span-3 border-l-4 border-hazard-pink pl-6 font-display text-2xl font-medium leading-snug text-chalk-white md:text-3xl">
      For chronically online 16–24-year-olds curious about how the world works,
      <span class="text-hazard-pink">ELI5 HQ</span> is the short-form animated
      STEM channel that explains real science like you're five — and then
      actually teaches it.
    </blockquote>
    <aside class="md:col-span-2">
      <div class="sticker sticker--shadow-pink inline-block bg-lab-lime px-5 py-4 font-display text-off-black">
        <p class="text-xs font-bold uppercase tracking-widest">The engine</p>
        <p class="mt-1 text-2xl font-bold leading-tight">
          Surface: dumb.<br />Core: real.
        </p>
      </div>
    </aside>
  </div>
</div>
```

- [ ] **Step 3: Verify in browser**

Reload `index.html`. Expected:
- Hero with big `ELI5` (the `5` Hazard Pink), small `HQ` underneath.
- Version badge in top of hero, pink background, tilted.
- Foundation section: quote block with pink left border, lime "engine" sticker card with pink shadow offset.
- No layout overflow on desktop.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Add hero masthead and foundation strip

Hero renders the ELI5 HQ wordmark with the '5' as the Hazard-Pink hero
character, the smaller HQ underneath, tagline, and a v1.0 sticker
badge. Foundation strip displays the positioning statement and the
"Surface: dumb. Core: real." engine quote on a Lab Lime sticker.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 3: Color palette section

**Files:**
- Modify: `/Users/sharmeyer/Development/eli5hq/brand/index.html` (`<section id="palette">`)

**Purpose:** Every color from the bible as a swatch card with name, hex, usage rule, and WCAG AA pass/fail labels for common text pairings.

- [ ] **Step 1: Fill the `<section id="palette">` block**

Replace the stub with:

```html
<div class="mx-auto max-w-6xl">
  <div class="mb-8 flex items-baseline gap-4">
    <h2 class="heading-wonk font-display text-4xl font-bold text-chalk-white md:text-5xl">
      Palette
    </h2>
    <span class="font-display text-xs font-bold uppercase tracking-[0.2em] text-lab-lime">
      saturated · slightly acidic · sticker-pack
    </span>
  </div>

  <div class="mb-10">
    <h3 class="mb-4 font-display text-sm font-bold uppercase tracking-widest text-smoke-500">
      Primary
    </h3>
    <div class="grid gap-4 md:grid-cols-3">
      <!-- Hazard Pink -->
      <div class="border border-smoke-900 bg-smoke-900 p-6">
        <div class="mb-4 h-24 bg-hazard-pink"></div>
        <p class="font-display text-lg font-bold text-chalk-white">Hazard Pink</p>
        <p class="font-mono text-sm text-smoke-200">#FF2E63</p>
        <p class="mt-3 font-body text-sm text-smoke-200">
          Brand primary. Logo, key hooks, accent across every video.
          <strong class="text-hazard-pink">Not optional.</strong>
        </p>
        <dl class="mt-4 space-y-1 font-mono text-xs">
          <div class="flex justify-between"><dt class="text-smoke-500">On Off-Black</dt><dd class="text-lab-lime">AA · 5.1:1</dd></div>
          <div class="flex justify-between"><dt class="text-smoke-500">Chalk on Pink (body)</dt><dd class="text-hazard-pink">FAIL · 3.3:1</dd></div>
        </dl>
      </div>

      <!-- Off-Black -->
      <div class="border border-smoke-900 bg-smoke-900 p-6">
        <div class="mb-4 h-24 bg-off-black border border-smoke-500"></div>
        <p class="font-display text-lg font-bold text-chalk-white">Off-Black</p>
        <p class="font-mono text-sm text-smoke-200">#0E0E12</p>
        <p class="mt-3 font-body text-sm text-smoke-200">
          Dominant background. Softer than pure #000 — feels designed.
        </p>
        <dl class="mt-4 space-y-1 font-mono text-xs">
          <div class="flex justify-between"><dt class="text-smoke-500">Chalk on Off-Black</dt><dd class="text-lab-lime">AA · 17:1</dd></div>
        </dl>
      </div>

      <!-- Paper Cream -->
      <div class="border border-smoke-900 bg-smoke-900 p-6">
        <div class="mb-4 h-24 bg-paper-cream"></div>
        <p class="font-display text-lg font-bold text-chalk-white">Paper Cream</p>
        <p class="font-mono text-sm text-smoke-200">#F5F1E8</p>
        <p class="mt-3 font-body text-sm text-smoke-200">
          Daylight-video background. Warm, aged, sticker-sheet energy.
        </p>
        <dl class="mt-4 space-y-1 font-mono text-xs">
          <div class="flex justify-between"><dt class="text-smoke-500">Off-Black on Cream</dt><dd class="text-lab-lime">AA · 13:1</dd></div>
        </dl>
      </div>
    </div>
  </div>

  <div class="mb-10">
    <h3 class="mb-4 font-display text-sm font-bold uppercase tracking-widest text-smoke-500">
      Secondary · Sticker Pack
    </h3>
    <div class="grid gap-4 md:grid-cols-5">
      <div class="border border-smoke-900 bg-smoke-900 p-4">
        <div class="mb-3 h-16 bg-lab-lime"></div>
        <p class="font-display text-sm font-bold text-chalk-white">Lab Lime</p>
        <p class="font-mono text-xs text-smoke-200">#C6F24E</p>
        <p class="mt-2 font-body text-xs text-smoke-500">Math · tech</p>
      </div>
      <div class="border border-smoke-900 bg-smoke-900 p-4">
        <div class="mb-3 h-16 bg-plasma-violet"></div>
        <p class="font-display text-sm font-bold text-chalk-white">Plasma Violet</p>
        <p class="font-mono text-xs text-smoke-200">#7A3BFF</p>
        <p class="mt-2 font-body text-xs text-smoke-500">Space · physics</p>
      </div>
      <div class="border border-smoke-900 bg-smoke-900 p-4">
        <div class="mb-3 h-16 bg-signal-orange"></div>
        <p class="font-display text-sm font-bold text-chalk-white">Signal Orange</p>
        <p class="font-mono text-xs text-smoke-200">#FF8A1F</p>
        <p class="mt-2 font-body text-xs text-smoke-500">Biology · everyday</p>
      </div>
      <div class="border border-smoke-900 bg-smoke-900 p-4">
        <div class="mb-3 h-16 bg-beaker-blue"></div>
        <p class="font-display text-sm font-bold text-chalk-white">Beaker Blue</p>
        <p class="font-mono text-xs text-smoke-200">#2EC4FF</p>
        <p class="mt-2 font-body text-xs text-smoke-500">Chemistry · tech</p>
      </div>
      <div class="border border-smoke-900 bg-smoke-900 p-4">
        <div class="mb-3 h-16 bg-moss-green"></div>
        <p class="font-display text-sm font-bold text-chalk-white">Moss Green</p>
        <p class="font-mono text-xs text-smoke-200">#4F8A3C</p>
        <p class="mt-2 font-body text-xs text-smoke-500">Nature</p>
      </div>
    </div>
  </div>

  <div>
    <h3 class="mb-4 font-display text-sm font-bold uppercase tracking-widest text-smoke-500">
      Neutrals
    </h3>
    <div class="grid gap-4 md:grid-cols-4">
      <div class="border border-smoke-900 bg-smoke-900 p-4">
        <div class="mb-3 h-16 bg-smoke-900 border border-smoke-500"></div>
        <p class="font-display text-sm font-bold text-chalk-white">Smoke 900</p>
        <p class="font-mono text-xs text-smoke-200">#1A1A20</p>
      </div>
      <div class="border border-smoke-900 bg-smoke-900 p-4">
        <div class="mb-3 h-16 bg-smoke-500"></div>
        <p class="font-display text-sm font-bold text-chalk-white">Smoke 500</p>
        <p class="font-mono text-xs text-smoke-200">#6B6B78</p>
      </div>
      <div class="border border-smoke-900 bg-smoke-900 p-4">
        <div class="mb-3 h-16 bg-smoke-200"></div>
        <p class="font-display text-sm font-bold text-chalk-white">Smoke 200</p>
        <p class="font-mono text-xs text-smoke-200">#C9C6BD</p>
      </div>
      <div class="border border-smoke-900 bg-smoke-900 p-4">
        <div class="mb-3 h-16 bg-chalk-white"></div>
        <p class="font-display text-sm font-bold text-chalk-white">Chalk White</p>
        <p class="font-mono text-xs text-smoke-200">#FAFAF5</p>
      </div>
    </div>
  </div>

  <div class="mt-10 border-l-4 border-lab-lime bg-smoke-900 p-5">
    <p class="font-display text-sm font-bold uppercase tracking-widest text-lab-lime">
      60 / 30 / 10 rule
    </p>
    <p class="mt-2 font-body text-base text-smoke-200">
      ~60% Off-Black or Paper Cream (ground) · ~30% one secondary (topic
      color) · ~10% Hazard Pink (brand signature). Never pair Hazard Pink
      with Signal Orange — they fight.
    </p>
  </div>
</div>
```

- [ ] **Step 2: Verify in browser**

Reload. Expected:
- "Palette" heading with a slight rotation and lime sub-label.
- Three primary swatches in a row on desktop, stacked on mobile.
- Five secondary swatches in a row on desktop.
- Four neutral swatches in a row on desktop.
- 60/30/10 rule callout at the bottom with a lime left border.
- Hex values render in monospace (Tailwind's `font-mono` default — fine, not a brand font but only appears in technical labels).

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Add color palette section with WCAG pairings

Renders every color from the bible as a swatch card: primaries with
named WCAG AA pass/fail pairings, five secondaries mapped to pillars,
four neutrals, and a 60/30/10 usage-rule callout.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 4: Typography specimens

**Files:**
- Modify: `/Users/sharmeyer/Development/eli5hq/brand/index.html` (`<section id="typography">`)

**Purpose:** Show Space Grotesk (display) and Inter (body) at real scale with every weight we actually use, plus a live hook → translation → caption example demonstrating how type is applied in video work.

- [ ] **Step 1: Fill the `<section id="typography">` block**

```html
<div class="mx-auto max-w-6xl">
  <div class="mb-8 flex items-baseline gap-4">
    <h2 class="heading-wonk font-display text-4xl font-bold text-chalk-white md:text-5xl">
      Typography
    </h2>
    <span class="font-display text-xs font-bold uppercase tracking-[0.2em] text-beaker-blue">
      two fonts · that's the cap
    </span>
  </div>

  <div class="grid gap-8 md:grid-cols-2">
    <!-- Display: Space Grotesk -->
    <div class="border border-smoke-900 bg-smoke-900 p-8">
      <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">Display</p>
      <p class="mt-1 font-display text-3xl font-bold text-chalk-white">Space Grotesk</p>
      <p class="mt-1 font-body text-sm text-smoke-200">
        Hooks · thumbnails · on-screen key words · stingers. Never below 24pt on video. Never italicized.
      </p>
      <div class="mt-6 space-y-3">
        <p class="font-display text-5xl font-bold text-chalk-white leading-tight">Aa Bb 5<span class="text-hazard-pink">2</span></p>
        <p class="font-display text-base font-medium text-smoke-200">500 Medium — the support weight</p>
        <p class="font-display text-base font-bold text-chalk-white">700 Bold — the default display weight</p>
      </div>
      <p class="mt-6 font-mono text-xs text-smoke-500">SIL OFL · Google Fonts · fallback: Archivo Black / Bagel Fat One</p>
    </div>

    <!-- Body: Inter -->
    <div class="border border-smoke-900 bg-smoke-900 p-8">
      <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">Body</p>
      <p class="mt-1 font-display text-3xl font-bold text-chalk-white">Inter</p>
      <p class="mt-1 font-body text-sm text-smoke-200">
        Burned-in captions · body text · bio copy · anything long-form. 600 for captions; 500 support; 400 UI/legal only.
      </p>
      <div class="mt-6 space-y-3">
        <p class="font-body text-5xl font-semibold text-chalk-white leading-tight">Aa Bb 5<span class="text-hazard-pink">2</span></p>
        <p class="font-body text-base font-normal text-smoke-200">400 Regular — UI/legal only</p>
        <p class="font-body text-base font-medium text-smoke-200">500 Medium — supporting lines</p>
        <p class="font-body text-base font-semibold text-chalk-white">600 Semibold — caption default</p>
      </div>
      <p class="mt-6 font-mono text-xs text-smoke-500">SIL OFL · Google Fonts · fallback: Manrope / DM Sans</p>
    </div>
  </div>

  <div class="mt-10 border border-smoke-900 bg-paper-cream p-8 text-off-black">
    <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">In use</p>
    <p class="mt-1 font-display text-xl font-bold">Hook → translation → caption, applied</p>

    <div class="mt-6 space-y-4">
      <p class="font-display text-3xl font-bold leading-tight md:text-4xl">
        why does your knuckle crack?
      </p>
      <p class="font-display text-2xl font-bold leading-tight">
        <span class="bg-hazard-pink px-1 text-chalk-white">cavitation</span>
        <span class="text-smoke-500">— a little gas bubble being born</span>
      </p>
      <p class="font-body text-base font-semibold">
        your knuckle is a little bag of joint juice. pressure drops, a
        bubble pops into existence. that pop? the bubble's birthday.
      </p>
    </div>
  </div>
</div>
```

- [ ] **Step 2: Verify in browser**

Reload. Expected:
- Two dark cards side-by-side (desktop) showing Space Grotesk and Inter.
- A Paper Cream card below showing the in-use example with a Hazard-Pink highlighted word.
- The specimen characters render in the correct fonts (Space Grotesk is geometric with a distinctive 2; Inter is neutral).
- Fallback fonts should NOT be visible on any modern browser with network access.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Add typography specimens and in-use example

Space Grotesk (500/700) and Inter (400/500/600) shown at scale with
usage rules and fallbacks. Paper Cream card demonstrates the hook →
translation → caption pattern with Hazard Pink highlight.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 5: Logo concept block

**Files:**
- Modify: `/Users/sharmeyer/Development/eli5hq/brand/index.html` (`<section id="logo">`)

**Purpose:** Make the "written logo concept" visible — wordmark with the `5` as the personality character, monogram tile for the avatar. No final logo art; this is the brief rendered as styled HTML so a designer can riff from it.

- [ ] **Step 1: Fill the `<section id="logo">` block**

```html
<div class="mx-auto max-w-6xl">
  <div class="mb-8 flex items-baseline gap-4">
    <h2 class="heading-wonk font-display text-4xl font-bold text-chalk-white md:text-5xl">
      Logo — concept
    </h2>
    <span class="font-display text-xs font-bold uppercase tracking-[0.2em] text-plasma-violet">
      wordmark-led · the 5 does the work
    </span>
  </div>

  <div class="grid gap-8 md:grid-cols-5">
    <!-- Wordmark mock -->
    <div class="md:col-span-3 border border-smoke-900 bg-smoke-900 p-10">
      <p class="font-display text-xs font-bold uppercase tracking-widest text-smoke-500">Primary lockup</p>
      <div class="mt-6 flex items-start gap-3">
        <span class="font-display text-[128px] font-bold leading-none text-chalk-white">ELI</span>
        <span class="sticker sticker--shadow-pink font-display text-[128px] font-bold leading-none text-hazard-pink">5</span>
        <span class="mt-3 inline-block bg-chalk-white px-2 py-1 font-display text-xs font-bold uppercase tracking-widest text-off-black">
          HQ
        </span>
      </div>
      <p class="mt-8 font-body text-sm text-smoke-200">
        The <span class="text-hazard-pink font-semibold">5</span> is the
        load-bearing personality character — sticker-treated, slightly
        off-register, a little hand-touched. <span class="uppercase tracking-widest text-xs font-bold text-chalk-white bg-chalk-white px-1.5 py-0.5 text-off-black">HQ</span>
        is a minor stamp, never a co-star.
      </p>
    </div>

    <!-- Monogram / avatar -->
    <div class="md:col-span-2 border border-smoke-900 bg-smoke-900 p-10 flex flex-col items-center justify-center text-center">
      <p class="font-display text-xs font-bold uppercase tracking-widest text-smoke-500 self-start">Monogram · Avatar</p>
      <div class="my-6 flex h-48 w-48 items-center justify-center bg-hazard-pink">
        <span class="font-display text-[120px] font-bold leading-none text-off-black">5</span>
      </div>
      <p class="font-body text-sm text-smoke-200">
        Off-Black 5 on a Hazard Pink tile. Works at 40×40px in the profile
        rail. Never invert to Paper Cream — loses punch.
      </p>
    </div>
  </div>

  <div class="mt-8 grid gap-4 md:grid-cols-2">
    <ul class="border border-lab-lime bg-smoke-900 p-6 font-body text-sm text-smoke-200 space-y-2">
      <li class="font-display font-bold text-lab-lime uppercase tracking-widest text-xs">Do</li>
      <li>• Flat only — no gradients in the lockup.</li>
      <li>• 5 works alone as the avatar.</li>
      <li>• Single-color knockout viable (white on color, color on off-black).</li>
      <li>• Min wordmark width 120px digital.</li>
    </ul>
    <ul class="border border-hazard-pink bg-smoke-900 p-6 font-body text-sm text-smoke-200 space-y-2">
      <li class="font-display font-bold text-hazard-pink uppercase tracking-widest text-xs">Don't</li>
      <li>• Science-cliché icons (atoms, beakers, gears, lightbulbs).</li>
      <li>• Gradients, drop-shadows, or 3D in the primary lockup.</li>
      <li>• Cutesy. We're confident, not precious.</li>
      <li>• HQ oversized — it's a stamp, not a co-star.</li>
    </ul>
  </div>
</div>
```

- [ ] **Step 2: Verify in browser**

Reload. Expected:
- Wordmark mock with oversized `ELI` + Hazard Pink `5` (with a pink-shadow sticker offset) + a small `HQ` chip.
- Monogram mock on the right: 192×192px Hazard Pink tile with a big off-black `5`.
- Do/Don't lists below with lime and pink borders.

- [ ] **Step 3: Commit & push**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Add logo concept block with wordmark and monogram mocks

CSS-rendered mock of the primary lockup (ELI + Hazard Pink 5 + small HQ
chip) and the monogram avatar tile. Includes do/don't rules for the
designer. No final art; this is the concept brief made visible.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
git push origin develop
```

---

## Task 6: Hook library (12 cards)

**Files:**
- Modify: `/Users/sharmeyer/Development/eli5hq/brand/index.html` (`<section id="hooks">`)

**Purpose:** 12 hook patterns from the bible as sticker-style reference cards. Each card: template + one worked example.

- [ ] **Step 1: Fill the `<section id="hooks">` block**

```html
<div class="mx-auto max-w-6xl">
  <div class="mb-8 flex items-baseline gap-4">
    <h2 class="heading-wonk font-display text-4xl font-bold text-chalk-white md:text-5xl">
      Hook library
    </h2>
    <span class="font-display text-xs font-bold uppercase tracking-[0.2em] text-signal-orange">
      first 2 seconds · fill in the blank
    </span>
  </div>

  <div class="grid gap-5 md:grid-cols-2 lg:grid-cols-3">
    <!-- 1 -->
    <article class="border border-smoke-900 bg-smoke-900 p-5">
      <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">01 · Dumb-question</p>
      <p class="mt-2 font-display text-lg font-bold text-chalk-white">why does [thing] [do thing]?</p>
      <p class="mt-3 font-body text-sm italic text-smoke-200">"why does the sky go orange at sunset?"</p>
    </article>
    <!-- 2 -->
    <article class="border border-smoke-900 bg-smoke-900 p-5">
      <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">02 · Five-year-old framing</p>
      <p class="mt-2 font-display text-lg font-bold text-chalk-white">okay imagine you're 5 and you asked [dumb question].</p>
      <p class="mt-3 font-body text-sm italic text-smoke-200">"imagine you're 5 and you asked why ice floats."</p>
    </article>
    <!-- 3 -->
    <article class="border border-smoke-900 bg-smoke-900 p-5">
      <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">03 · Wait-what</p>
      <p class="mt-2 font-display text-lg font-bold text-chalk-white">wait. [counterintuitive fact]. actually wait.</p>
      <p class="mt-3 font-body text-sm italic text-smoke-200">"wait. you can't touch anything. actually wait."</p>
    </article>
    <!-- 4 -->
    <article class="border border-smoke-900 bg-smoke-900 p-5">
      <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">04 · Numeric gut-punch</p>
      <p class="mt-2 font-display text-lg font-bold text-chalk-white">there are more [x] than [y]. let that cook.</p>
      <p class="mt-3 font-body text-sm italic text-smoke-200">"more stars than grains of sand. let that cook."</p>
    </article>
    <!-- 5 -->
    <article class="border border-smoke-900 bg-smoke-900 p-5">
      <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">05 · Accusation</p>
      <p class="mt-2 font-display text-lg font-bold text-chalk-white">[field of science] is lying to you and here's how.</p>
      <p class="mt-3 font-body text-sm italic text-smoke-200">"chemistry is lying to you and here's how."</p>
    </article>
    <!-- 6 -->
    <article class="border border-smoke-900 bg-smoke-900 p-5">
      <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">06 · Cursed fact drop</p>
      <p class="mt-2 font-display text-lg font-bold text-chalk-white">[insane true thing]. that's real. right now.</p>
      <p class="mt-3 font-body text-sm italic text-smoke-200">"there's a cloud of alcohol in space. that's real."</p>
    </article>
    <!-- 7 -->
    <article class="border border-smoke-900 bg-smoke-900 p-5">
      <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">07 · Everyday-to-cosmic</p>
      <p class="mt-2 font-display text-lg font-bold text-chalk-white">you do [normal thing]. physics does [insane thing] so that works.</p>
      <p class="mt-3 font-body text-sm italic text-smoke-200">"you open the fridge. physics does interpretive dance so that works."</p>
    </article>
    <!-- 8 -->
    <article class="border border-smoke-900 bg-smoke-900 p-5">
      <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">08 · Roast setup</p>
      <p class="mt-2 font-display text-lg font-bold text-chalk-white">[animal] is the [unhinged superlative] and nobody's talking about it.</p>
      <p class="mt-3 font-body text-sm italic text-smoke-200">"the mantis shrimp is the worst roommate in the ocean."</p>
    </article>
    <!-- 9 -->
    <article class="border border-smoke-900 bg-smoke-900 p-5">
      <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">09 · Wrong-on-purpose</p>
      <p class="mt-2 font-display text-lg font-bold text-chalk-white">[confidently wrong]. ok not really. here's the real answer.</p>
      <p class="mt-3 font-body text-sm italic text-smoke-200">"the sun is on fire. ok not really. it's worse."</p>
    </article>
    <!-- 10 -->
    <article class="border border-smoke-900 bg-smoke-900 p-5">
      <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">10 · Class-failed</p>
      <p class="mt-2 font-display text-lg font-bold text-chalk-white">they told you [school version]. they lied for convenience.</p>
      <p class="mt-3 font-body text-sm italic text-smoke-200">"they told you atoms are balls with dots. they lied."</p>
    </article>
    <!-- 11 -->
    <article class="border border-smoke-900 bg-smoke-900 p-5">
      <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">11 · Receipts</p>
      <p class="mt-2 font-display text-lg font-bold text-chalk-white">i will now prove [absurd claim] in under 30 seconds.</p>
      <p class="mt-3 font-body text-sm italic text-smoke-200">"i will now prove you're mostly empty space."</p>
    </article>
    <!-- 12 -->
    <article class="border border-smoke-900 bg-smoke-900 p-5">
      <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">12 · Name-the-villain</p>
      <p class="mt-2 font-display text-lg font-bold text-chalk-white">[concept], explained. it's the reason [annoying everyday thing].</p>
      <p class="mt-3 font-body text-sm italic text-smoke-200">"entropy, explained. it's the reason your room is messy."</p>
    </article>
  </div>
</div>
```

- [ ] **Step 2: Verify in browser**

Reload. Expected:
- 12 cards in a 3-column grid on desktop (2-col on md, 1-col on mobile).
- Each card: pink number+label, bold template line, italic example.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Add hook library with 12 reusable patterns

Renders the hook-library section from the bible as a responsive 3-col
grid. Each card has a numbered label, template, and example in the
brand voice.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 7: Do / don't grid

**Files:**
- Modify: `/Users/sharmeyer/Development/eli5hq/brand/index.html` (`<section id="do-dont">`)

**Purpose:** Side-by-side ✅/❌ pairs from bible §6. Left column = do (lime); right column = don't (pink).

- [ ] **Step 1: Fill the `<section id="do-dont">` block**

```html
<div class="mx-auto max-w-6xl">
  <div class="mb-8 flex items-baseline gap-4">
    <h2 class="heading-wonk font-display text-4xl font-bold text-chalk-white md:text-5xl">
      Do · Don't
    </h2>
    <span class="font-display text-xs font-bold uppercase tracking-[0.2em] text-lab-lime">
      voice applied · side by side
    </span>
  </div>

  <div class="grid gap-4 md:grid-cols-2">
    <!-- DO column -->
    <div class="border-l-4 border-lab-lime bg-smoke-900 p-6 space-y-5">
      <p class="font-display text-lg font-bold uppercase tracking-widest text-lab-lime">Do</p>
      <p class="font-body text-base text-chalk-white">"why does your knuckle crack?"</p>
      <p class="font-body text-base text-chalk-white">"there are more shuffles than atoms on earth. let that cook."</p>
      <p class="font-body text-base text-chalk-white">"anyway. your brain is a three-pound electric meat sponge. sleep well."</p>
      <p class="font-body text-base text-chalk-white">"platypus PSA"</p>
      <p class="font-body text-base text-chalk-white">"why does rain smell good? dirt bacteria having a party."</p>
    </div>

    <!-- DON'T column -->
    <div class="border-l-4 border-hazard-pink bg-smoke-900 p-6 space-y-5 text-smoke-500">
      <p class="font-display text-lg font-bold uppercase tracking-widest text-hazard-pink">Don't</p>
      <p class="font-body text-base line-through">"have you ever wondered what makes that popping sound when you crack your knuckles?"</p>
      <p class="font-body text-base line-through">"did you know… 52 factorial is a truly mind-boggling number! 🤯"</p>
      <p class="font-body text-base line-through">"thanks for watching! don't forget to like and subscribe!"</p>
      <p class="font-body text-base line-through">"TOP 5 WEIRD FACTS ABOUT PLATYPUSES YOU WON'T BELIEVE!!!"</p>
      <p class="font-body text-base line-through">"🌧️✨ have you ever noticed the beautiful smell of rain?? 🌧️✨"</p>
    </div>
  </div>
</div>
```

- [ ] **Step 2: Verify in browser**

Reload. Expected:
- Two columns. Left column: lime left border, chalk-white text, clean examples. Right column: pink left border, struck-through text in smoke-500 gray.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Add do/don't grid

Side-by-side voice-applied examples from bible §6. Do column gets lime
border and chalk text; don't column gets pink border with strike-through
smoke text.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 8: Pre-publish checklist

**Files:**
- Modify: `/Users/sharmeyer/Development/eli5hq/brand/index.html` (`<section id="checklist">`)

**Purpose:** 15-point pre-publish checklist from bible §7 as an unchecked reference card.

- [ ] **Step 1: Fill the `<section id="checklist">` block**

```html
<div class="mx-auto max-w-6xl">
  <div class="mb-8 flex items-baseline gap-4">
    <h2 class="heading-wonk font-display text-4xl font-bold text-chalk-white md:text-5xl">
      Pre-publish checklist
    </h2>
    <span class="font-display text-xs font-bold uppercase tracking-[0.2em] text-beaker-blue">
      60-second run · if any fail, don't ship
    </span>
  </div>

  <div class="border-2 border-hazard-pink bg-off-black p-8">
    <ol class="grid gap-3 md:grid-cols-2 font-body text-base text-chalk-white">
      <li class="flex gap-3"><span class="inline-block h-5 w-5 shrink-0 border-2 border-smoke-500 mt-0.5" aria-hidden="true"></span><span><strong class="font-display text-smoke-500 text-xs uppercase tracking-widest block">01</strong>Hook lands in first 2s with no "today we'll" energy.</span></li>
      <li class="flex gap-3"><span class="inline-block h-5 w-5 shrink-0 border-2 border-smoke-500 mt-0.5" aria-hidden="true"></span><span><strong class="font-display text-smoke-500 text-xs uppercase tracking-widest block">02</strong>Clear whiplash pivot by second 8.</span></li>
      <li class="flex gap-3"><span class="inline-block h-5 w-5 shrink-0 border-2 border-smoke-500 mt-0.5" aria-hidden="true"></span><span><strong class="font-display text-smoke-500 text-xs uppercase tracking-widest block">03</strong>Science is accurate with a source on hand.</span></li>
      <li class="flex gap-3"><span class="inline-block h-5 w-5 shrink-0 border-2 border-smoke-500 mt-0.5" aria-hidden="true"></span><span><strong class="font-display text-smoke-500 text-xs uppercase tracking-widest block">04</strong>Every technical term translated on first use.</span></li>
      <li class="flex gap-3"><span class="inline-block h-5 w-5 shrink-0 border-2 border-smoke-500 mt-0.5" aria-hidden="true"></span><span><strong class="font-display text-smoke-500 text-xs uppercase tracking-widest block">05</strong>Video is 15–60 seconds.</span></li>
      <li class="flex gap-3"><span class="inline-block h-5 w-5 shrink-0 border-2 border-smoke-500 mt-0.5" aria-hidden="true"></span><span><strong class="font-display text-smoke-500 text-xs uppercase tracking-widest block">06</strong>Hazard Pink appears on screen at least once.</span></li>
      <li class="flex gap-3"><span class="inline-block h-5 w-5 shrink-0 border-2 border-smoke-500 mt-0.5" aria-hidden="true"></span><span><strong class="font-display text-smoke-500 text-xs uppercase tracking-widest block">07</strong>Captions burned in, Inter 600+, not CapCut default.</span></li>
      <li class="flex gap-3"><span class="inline-block h-5 w-5 shrink-0 border-2 border-smoke-500 mt-0.5" aria-hidden="true"></span><span><strong class="font-display text-smoke-500 text-xs uppercase tracking-widest block">08</strong>Stinger in last 2–5s (callback / joke / wait-what).</span></li>
      <li class="flex gap-3"><span class="inline-block h-5 w-5 shrink-0 border-2 border-smoke-500 mt-0.5" aria-hidden="true"></span><span><strong class="font-display text-smoke-500 text-xs uppercase tracking-widest block">09</strong>Zero talking heads, politics, or punching down.</span></li>
      <li class="flex gap-3"><span class="inline-block h-5 w-5 shrink-0 border-2 border-smoke-500 mt-0.5" aria-hidden="true"></span><span><strong class="font-display text-smoke-500 text-xs uppercase tracking-widest block">10</strong>VO uses contractions and short sentences throughout.</span></li>
      <li class="flex gap-3"><span class="inline-block h-5 w-5 shrink-0 border-2 border-smoke-500 mt-0.5" aria-hidden="true"></span><span><strong class="font-display text-smoke-500 text-xs uppercase tracking-widest block">11</strong>Thumbnail (Shorts): Off-Black BG + 1 accent + ≤4-word hook.</span></li>
      <li class="flex gap-3"><span class="inline-block h-5 w-5 shrink-0 border-2 border-smoke-500 mt-0.5" aria-hidden="true"></span><span><strong class="font-display text-smoke-500 text-xs uppercase tracking-widest block">12</strong>Caption ≤200 chars, 3–5 hashtags, one is the pillar.</span></li>
      <li class="flex gap-3"><span class="inline-block h-5 w-5 shrink-0 border-2 border-smoke-500 mt-0.5" aria-hidden="true"></span><span><strong class="font-display text-smoke-500 text-xs uppercase tracking-widest block">13</strong>Title fits one of our patterns.</span></li>
      <li class="flex gap-3"><span class="inline-block h-5 w-5 shrink-0 border-2 border-smoke-500 mt-0.5" aria-hidden="true"></span><span><strong class="font-display text-smoke-500 text-xs uppercase tracking-widest block">14</strong>Read aloud — sounds like a smart friend, not a teacher.</span></li>
      <li class="flex gap-3"><span class="inline-block h-5 w-5 shrink-0 border-2 border-smoke-500 mt-0.5" aria-hidden="true"></span><span><strong class="font-display text-smoke-500 text-xs uppercase tracking-widest block">15</strong>If someone shared this with a friend, is there a quotable line?</span></li>
    </ol>
  </div>
</div>
```

- [ ] **Step 2: Verify in browser**

Reload. Expected:
- Hazard-Pink-bordered checklist card.
- 15 items in a 2-column grid on desktop, 1-column on mobile.
- Each item: unchecked square, numbered label in smoke, statement in chalk.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Add pre-publish checklist reference card

Renders the 15-point pre-publish checklist from bible §7 as a pink-
bordered reference card with unchecked boxes and numbered labels.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 9: Four-beat formula diagram

**Files:**
- Modify: `/Users/sharmeyer/Development/eli5hq/brand/index.html` (`<section id="four-beat">`)

**Purpose:** Visual timeline of the four-beat formula: dumb premise / whiplash pivot / real payoff / stinger.

- [ ] **Step 1: Fill the `<section id="four-beat">` block**

```html
<div class="mx-auto max-w-6xl">
  <div class="mb-8 flex items-baseline gap-4">
    <h2 class="heading-wonk font-display text-4xl font-bold text-chalk-white md:text-5xl">
      Four-beat formula
    </h2>
    <span class="font-display text-xs font-bold uppercase tracking-[0.2em] text-signal-orange">
      every video · no exceptions
    </span>
  </div>

  <div class="relative overflow-hidden border border-smoke-900 bg-smoke-900 p-8">
    <!-- Timeline bar -->
    <div class="mb-8 flex h-3 w-full overflow-hidden">
      <div class="w-[7%] bg-hazard-pink"></div>
      <div class="w-[11%] bg-lab-lime"></div>
      <div class="w-[78%] bg-plasma-violet"></div>
      <div class="w-[4%] bg-signal-orange"></div>
    </div>

    <div class="grid gap-6 md:grid-cols-4">
      <div>
        <p class="font-display text-xs font-bold uppercase tracking-widest text-hazard-pink">Beat 1</p>
        <p class="mt-1 font-display text-2xl font-bold text-chalk-white">Dumb premise</p>
        <p class="mt-2 font-mono text-xs text-smoke-500">0–3s</p>
        <p class="mt-3 font-body text-sm text-smoke-200">
          Stop the thumb. Ask the dumbest honest version of the question.
        </p>
      </div>
      <div>
        <p class="font-display text-xs font-bold uppercase tracking-widest text-lab-lime">Beat 2</p>
        <p class="mt-1 font-display text-2xl font-bold text-chalk-white">Whiplash pivot</p>
        <p class="mt-2 font-mono text-xs text-smoke-500">3–8s</p>
        <p class="mt-3 font-body text-sm text-smoke-200">
          "Actually—" snap into the real topic. We're not joking anymore.
        </p>
      </div>
      <div>
        <p class="font-display text-xs font-bold uppercase tracking-widest text-plasma-violet">Beat 3</p>
        <p class="mt-1 font-display text-2xl font-bold text-chalk-white">Real payoff</p>
        <p class="mt-2 font-mono text-xs text-smoke-500">8–45s</p>
        <p class="mt-3 font-body text-sm text-smoke-200">
          The actual explanation. Rigorous. Translated, not dumbed down.
        </p>
      </div>
      <div>
        <p class="font-display text-xs font-bold uppercase tracking-widest text-signal-orange">Beat 4</p>
        <p class="mt-1 font-display text-2xl font-bold text-chalk-white">Stinger</p>
        <p class="mt-2 font-mono text-xs text-smoke-500">last 2–5s</p>
        <p class="mt-3 font-body text-sm text-smoke-200">
          Callback, wait-what, or absurdist bow. Never a CTA.
        </p>
      </div>
    </div>

    <p class="mt-8 border-l-4 border-hazard-pink bg-off-black p-4 font-body text-sm text-smoke-200">
      Sweet spot: <span class="text-chalk-white font-semibold">22–45s</span> ·
      floor <span class="text-chalk-white font-semibold">15s</span> ·
      ceiling <span class="text-chalk-white font-semibold">60s</span>.
      Longer than that, split into a two-parter.
    </p>
  </div>
</div>
```

- [ ] **Step 2: Verify in browser**

Reload. Expected:
- Four-segment timeline bar at top showing proportional beats (pink/lime/violet/orange).
- Four-column grid below with labeled beats, timing in mono, description in body.
- Length-bounds callout at the bottom.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Add four-beat formula timeline diagram

Visual timeline with proportional colored segments and a four-column
breakdown of each beat (timing, description). Plus a length-bounds
callout anchored with a Hazard Pink border.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 10: Footer + polish pass

**Files:**
- Modify: `/Users/sharmeyer/Development/eli5hq/brand/index.html` (footer and any tweaks found during full-page review)

**Purpose:** Replace the scaffold footer, do a top-to-bottom QA pass: consistent section spacing, heading sizes, noise overlay visible, no layout breaks on mobile, all zigzag dividers render.

- [ ] **Step 1: Replace the footer content**

Replace the entire `<footer>` block with:

```html
<footer class="mt-20 border-t border-smoke-900 bg-off-black px-6 py-12 md:px-12">
  <div class="mx-auto max-w-6xl md:flex md:items-baseline md:justify-between md:gap-10">
    <div>
      <p class="font-display text-lg font-bold text-chalk-white">
        ELI<span class="text-hazard-pink">5</span> HQ — Brand Guide v1.0
      </p>
      <p class="mt-2 font-body text-sm text-smoke-500">
        Source of truth:
        <a href="brand-bible.md" class="underline decoration-hazard-pink decoration-2 underline-offset-4 hover:text-chalk-white">brand-bible.md</a>
        · last updated 2026-04-22
      </p>
    </div>
    <p class="mt-6 max-w-md font-body text-xs text-smoke-500 md:mt-0 md:text-right">
      This page is an internal reference. If a decision isn't here, default to
      the bible. If the bible doesn't cover it, the founder decides.
    </p>
  </div>
</footer>
```

- [ ] **Step 2: Full-page QA pass**

Open the page, scroll top to bottom. Check:
- Hero fonts render (Space Grotesk visible, not fallback).
- Every section heading has the `-1.25°` rotation via `.heading-wonk`.
- Zigzag divider SVG renders between sections (should see a pink zigzag line, not a solid line).
- Noise overlay is subtle but visible (compare with/without by toggling `body::before` opacity in DevTools).
- Color palette WCAG labels are readable.
- Typography card's Paper Cream section has enough contrast (Off-Black text on Cream = AA 13:1).
- Logo concept wordmark `5` has the pink offset shadow.
- 12 hook cards lay out in 3 columns at desktop, 2 at md, 1 at mobile.
- Do/don't strike-through renders.
- Checklist card is pink-bordered and items are visible.
- Four-beat timeline bar colors match the labeled beats.
- Footer's `brand-bible.md` link is underlined in Hazard Pink.

- [ ] **Step 3: Responsive check**

Resize the browser window to ~375px width. Expected:
- All sections stack into single columns where applicable.
- No horizontal scroll.
- Hero wordmark shrinks (using `clamp()` — should hit the 64px floor at 375px).

- [ ] **Step 4: Fix any issues found**

Edit `index.html` to address anything broken. Keep fixes tight — we're polishing, not refactoring.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
Add footer and polish pass

Replaces the scaffold footer with the real version (wordmark, link to
brand-bible.md, last-updated, internal-use note). Includes any polish
fixes found during full-page QA.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
```

---

## Task 11: Accessibility audit (Lighthouse)

**Files:**
- Potentially modify: `/Users/sharmeyer/Development/eli5hq/brand/index.html` (only if Lighthouse surfaces issues)

**Purpose:** Enforce the spec's ≥ 95 Lighthouse accessibility score and fix any surfaced issues.

- [ ] **Step 1: Run Lighthouse**

In Chrome:
1. Open `index.html` via `file://`.
2. DevTools → Lighthouse → select Accessibility only → Analyze.
3. Alternatively, use the CLI: `npx lighthouse file:///Users/sharmeyer/Development/eli5hq/brand/index.html --only-categories=accessibility --quiet --chrome-flags="--headless"` (requires `npx` with Lighthouse installed — skip if not available; DevTools run is sufficient).

Record the score.

- [ ] **Step 2: Fix any issues**

Common fixable issues for this page:
- Missing `aria-hidden="true"` on decorative SVG dividers → already in scaffold, verify.
- Insufficient contrast → should not occur given the palette pairings used, but if flagged, swap text color.
- Missing alt text → no `<img>` elements in this page, but check.
- Missing `<main>` landmark → already present.

Make any required edits.

- [ ] **Step 3: Re-run Lighthouse**

Confirm score ≥ 95. If not, iterate until it is.

- [ ] **Step 4: Commit (only if fixes were made)**

If Steps 2–3 surfaced no issues and nothing changed, skip this step entirely.

If fixes were made, write a commit message that lists each fix concretely (e.g., "Added `aria-hidden='true'` to zigzag dividers", "Raised Smoke 500 metadata text to Smoke 200 to reach 4.5:1 contrast"). Do not commit with a generic "fix lighthouse issues" message — the commit log should record what actually changed.

```bash
git add index.html
git commit  # opens editor; write the concrete fix list
```

---

## Task 12: Push `develop` and open PR to `main`

**Files:** (none — git operations)

**Purpose:** Get all the style-guide work onto `origin/develop` and open a PR so it can merge into `main` for GitHub Pages deploy.

- [ ] **Step 1: Check current state**

Run:
```bash
cd /Users/sharmeyer/Development/eli5hq/brand
git status
git log --oneline origin/develop..HEAD
```

Expected: working tree clean, and the log shows the new commits since the last push (Tasks 1–11's commits that haven't been pushed).

- [ ] **Step 2: Push to origin/develop**

Run:
```bash
git push origin develop
```

Expected: successful push.

- [ ] **Step 3: Check the existing develop→main PR**

Run:
```bash
gh pr list --base main --head develop
```

If PR #1 (the brand bible PR) is still open, the style-guide commits will have been added to it automatically. Either:
- **(a)** Let the style guide land with the bible in PR #1 (simpler, one merge). Update the PR description to note the added scope.
- **(b)** Merge PR #1 first, then open a fresh develop→main PR for just the style guide.

Unless the user says otherwise, prefer (a) — less ceremony. Update the PR:

```bash
gh pr edit 1 --body "$(cat <<'EOF'
## Summary

- Lays down the ELI5 HQ brand system and the internal style guide page in one go.
- Adds the approved strategic brief, the Brand Bible v1.0, a project README, and a single-page HTML style guide deployed via GitHub Pages.

## What's in here

- **`docs/superpowers/specs/2026-04-22-eli5hq-brand-brief.md`** — approved strategic brief.
- **`brand-bible.md`** — ~6,500-word operating manual.
- **`README.md`** — repo orientation.
- **`index.html`** — internal brand style guide, served via GitHub Pages at `https://eli5hq.github.io/brand/`.
- **`docs/superpowers/specs/2026-04-22-brand-style-guide-design.md`** — style-guide spec.
- **`docs/superpowers/plans/2026-04-22-brand-style-guide.md`** — implementation plan.

## GitHub Pages

After merge: enable Pages at Settings → Pages → Deploy from branch → `main` / `(root)`.

🤖 Generated with [Claude Code](https://claude.com/claude-code)
EOF
)"
```

- [ ] **Step 4: Verify PR reflects the new commits**

Run:
```bash
gh pr view 1
```

Expected: commit list now includes the style-guide commits. CI (if any) is green.

---

## Task 13: Enable GitHub Pages

**Files:** (none — GitHub configuration)

**Purpose:** After PR #1 merges, turn on GitHub Pages so the published URL goes live.

- [ ] **Step 1: Wait for PR merge**

Confirm with the user that PR #1 has been merged into `main`. Do not proceed until then.

- [ ] **Step 2: Enable Pages via `gh api`**

Run:
```bash
gh api -X POST /repos/eli5hq/brand/pages \
  -f "source[branch]=main" \
  -f "source[path]=/"
```

If Pages is already enabled (returns 409), update instead:
```bash
gh api -X PUT /repos/eli5hq/brand/pages \
  -F "source[branch]=main" \
  -F "source[path]=/"
```

Expected: a 201 or 204 response. GitHub begins the initial deploy.

- [ ] **Step 3: Watch the first deploy**

Run:
```bash
gh api /repos/eli5hq/brand/pages/builds/latest --jq '.status, .error.message'
```

Expected: `built` and `null` within 60 seconds. If `errored`, read the message.

- [ ] **Step 4: Verify the live URL**

Run:
```bash
curl -sI https://eli5hq.github.io/brand/ | head -5
```

Expected: `HTTP/2 200`. Open `https://eli5hq.github.io/brand/` in a browser and spot-check that fonts and palette render identically to the local version.

- [ ] **Step 5: Note the URL in the README**

If not already there, add a "Published at" line near the top of `README.md`:

```markdown
> **Style guide:** https://eli5hq.github.io/brand/
```

Commit:

```bash
git add README.md
git commit -m "$(cat <<'EOF'
Add published style-guide URL to README

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>
EOF
)"
git push origin develop
```

(This goes on `develop`; flows into `main` on the next PR cycle.)

---

## Done criteria

- All 13 tasks checked off.
- `index.html` renders in-brand at `https://eli5hq.github.io/brand/`.
- Lighthouse accessibility score ≥ 95.
- No console errors in Chrome/Safari/Firefox (Tailwind CDN "production" warning is acceptable and expected).
- `develop` merged into `main` via PR; `develop` contains any post-merge additions.

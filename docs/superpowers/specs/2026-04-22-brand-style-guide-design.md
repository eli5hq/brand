# ELI5 HQ — Internal Brand Style Guide (HTML) — Design Spec

**Status:** Approved (design), pending implementation
**Date:** 2026-04-22
**Owner:** Steve Harmeyer

A single-page static HTML brand style guide for internal use. Renders the
bible's visual and reference content in-brand, as a living reference
designers, animators, editors, and caption writers can open in a browser.

---

## 1. Goals & Non-Goals

**Goals:**

- Give practitioners (founder + freelancers) one scannable URL that shows
  the brand "for real" — colors rendered, type specimens at scale, hook
  library and checklists as reference cards.
- Make the page itself a demonstration of the brand voice and visual
  direction. Looking at the page should teach a new contributor what
  "on-brand" means.
- Host on GitHub Pages at `https://eli5hq.github.io/brand/`.

**Non-goals:**

- Not a public-facing landing page. No email capture, no "coming soon"
  copy, no social links to grow an audience.
- Not a full Storybook-style interactive design system. No copy-to-clipboard,
  no component playground, no theme switcher.
- Not the eli5hq.com site. If/when the public site gets built, it's a
  separate project on a separate path.

## 2. Page Sections (top to bottom)

1. **Hero masthead** — oversized `ELI5 HQ` wordmark in Space Grotesk 700,
   Hazard Pink on Off-Black. Tagline: *"STEM explained like you're 5. The
   science is still for grown-ups."* Sticker-style `v1.0 brand guide`
   badge in a corner.
2. **Foundation strip** — positioning one-liner + *"Surface: dumb. Core:
   real."* engine quote. Zine-torn block.
3. **Color palette** — every color from the bible as a swatch card. Each
   card: color block, name, hex, usage rule, WCAG AA pass/fail labels on
   common text pairings.
4. **Typography specimens** — Space Grotesk (display) and Inter (body)
   shown at real scale, each weight labeled. Live example of "hook →
   translation → caption" styling.
5. **Logo concept block** — CSS-rendered visualization of the wordmark
   treatment and the monogram tile (no final logo art yet; this is the
   concept brief made visible).
6. **Hook library** — 12 hook patterns as sticker-style cards. Template
   + one brand-voice worked example per card.
7. **Do / don't grid** — side-by-side ✅/❌ pairs from bible §6.
8. **Pre-publish checklist** — 15 items from bible §7 as unchecked
   reference card.
9. **Four-beat formula diagram** — timeline visual of 0–3s / 3–8s /
   8–45s / last 2–5s beats with annotations.
10. **Footer** — version + source-of-truth pointer to `brand-bible.md` +
    last-updated date.

## 3. Visual Approach

- **Background:** Off-Black dominant. Paper Cream in the typography
  specimen card (to demonstrate the light-mode treatment).
- **Accent discipline:** Hazard Pink appears in every section (bible rule:
  "Hazard Pink is not optional"). Each section gets one secondary color
  as its vibe anchor (e.g., palette → Lab Lime, logo concept → Plasma
  Violet).
- **Sticker energy:** headings set slightly rotated (−1° to 2°) in
  Space Grotesk. Cards have subtle tilt (~1–3°), with an off-register
  color-fill shadow (sharp, unblurred, Plasma Violet by default) offset
  2–4px.
- **Paper grain:** SVG noise overlay at 5–8% opacity on the page body.
- **Dividers:** hand-drawn-feeling zigzag SVG lines between major sections.
- **No stock icons, no emojis.** Hand-drawn/scribbled SVG accents only.

## 4. Technical Architecture

- Single `index.html` at repo root.
- **Tailwind CSS via CDN** (`https://cdn.tailwindcss.com`) with inline
  Tailwind config extending the theme with the exact brand tokens:
  - Colors: `hazard-pink`, `off-black`, `paper-cream`, `lab-lime`,
    `plasma-violet`, `signal-orange`, `beaker-blue`, `moss-green`,
    `smoke-900`, `smoke-500`, `smoke-200`, `chalk-white`.
  - Fonts: `display` → `Space Grotesk`, `body` → `Inter`.
- **Custom CSS** in a `<style>` block for what Tailwind does poorly:
  rotated stickers, off-register shadows, noise overlay, zigzag SVG
  dividers.
- **Google Fonts `<link>`** for Space Grotesk (500, 700) + Inter (400,
  500, 600).
- **No JavaScript** beyond the Tailwind CDN script. Pure static.

## 5. File Layout

```
/                  ← repo root
├── index.html     ← the style guide (new)
├── README.md
├── brand-bible.md
└── docs/
```

No separate CSS, JS, or asset files for v1. If assets (logo SVG, mockup
images) arrive later, introduce an `assets/` folder at repo root.

## 6. GitHub Pages Setup

- **Source:** Deploy from a branch → `main` → `/ (root)`.
- **Published URL:** `https://eli5hq.github.io/brand/`.
- Configuration via `gh api` (command-line) to avoid the GitHub UI
  round-trip.
- Custom domain deferred (requires DNS + `CNAME` file; out of scope).

## 7. Accessibility

- All text-on-color pairings must hit WCAG AA (4.5:1 body, 3:1 large).
  Bible already catalogues safe pairings; the page exhibits them and
  labels the unsafe ones (e.g., Chalk White on Hazard Pink flagged as
  "AA-fail for body text").
- Semantic HTML (`<header>`, `<main>`, `<section>`, `<footer>`,
  `<h1>`–`<h3>` hierarchy).
- All decorative SVG `aria-hidden="true"`. Meaningful elements labeled.
- Keyboard focus states visible. No interactive controls that aren't
  actual controls.

## 8. Success Criteria

The style guide succeeds if:

- A new freelancer can open the page and understand the palette, type,
  voice, and video formula without reading the bible markdown.
- The page itself looks unmistakably like an ELI5 HQ artifact (Hazard
  Pink, Off-Black, Space Grotesk, sticker energy).
- It renders identically on latest Chrome, Safari, and Firefox.
- Loads in under 2 seconds over a typical connection (no heavy assets).
- Lighthouse accessibility score ≥ 95.

## 9. Workflow

- All work commits directly to `develop` (solo founder, low ceremony).
- Merges into `main` via the existing develop→main PR pattern.
- GitHub Pages serves from `main`, so the page only goes live after PR
  merge.

## 10. Out of Scope

- Logo art files (CSS mock only).
- Mockup images (storyboard/thumbnail samples as styled HTML, not images).
- Copy-to-clipboard swatch interactions.
- Dark/light mode toggle (the brand IS dark by default).
- Print stylesheet.
- i18n.
- Analytics.

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

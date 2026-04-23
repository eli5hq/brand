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

# Reply Bubble Studio

A single-file web app for generating stylish "Reply to @username" comment bubbles in the style of TikTok reply videos — then downloading any of them as a pixel-perfect transparent PNG.

Open `reply-bubble-studio.html` in a browser. No build step, no dependencies, no network calls except Google Fonts.

## Features

- **50 sticker skins**, loaded 15 at a time (`+ Show 15 more` / `Show all 50`)
- **Live text editing** — type in the sidebar, or click any bubble and edit it in place; edits mirror across every unlocked bubble
- **Lock** a bubble to give it its own text
- **Generate** rolls a fresh sample comment and username into every skin
- **Typography controls** — 22 typefaces, weight, size, line height, tracking, letter case, italic. Each has an "Auto" state that defers to the skin's own design; set one and it applies identically everywhere.
- **Text outline** — TikTok-style black stroke (Off / Soft / Bold), drawn with `paint-order: stroke fill` so it never thins the glyphs
- **Color** — hue shift and saturation across all skins
- **PNG export** — per-bubble download at 2×/3×/4×, on a transparent, white, or black ground, auto-trimmed to the sticker's alpha bounds
- **Expand** — drop one bubble on a clean ground for screenshotting
- Light and dark UI themes

## How the skins are built

Two rendering paths share one source of truth.

The 14 original skins are hand-written CSS, each with a matching Canvas 2D painter for export.

The other 36 are **engine skins**: each is a declarative spec — shape, gradient layers, decor, rim, tail, typography — and a single painter reads that spec for *both* the on-page canvas underlay and the PNG export. The page and the downloaded file therefore cannot drift apart.

The spec vocabulary covers linear/radial/conic/mesh gradients, 18 decor types (stars, orbs, constellations, perspective grids, hex meshes, waves, rays, streaks, contours, sparks, grain…), rounded/notched/blob shapes, and 8 speech-tail styles.

## Why Canvas instead of DOM screenshotting

The usual DOM-to-PNG approach (serializing to SVG `foreignObject`, then rasterizing) silently drops webfonts, blend modes, `backdrop-filter`, and `blur` — which is most of what these skins are made of. Every skin is repainted natively in Canvas 2D instead, so exports are deterministic and match what's on screen.

## Layout

Everything lives in `reply-bubble-studio.html`:

1. `<style>` — design tokens, tool chrome, the 14 hand-built skins, engine-skin scaffolding
2. Markup — top bar, console, stage, focus overlay
3. Script 1 — the skin engine (spec painter + 36 specs)
4. Script 2 — UI, roster, paging, in-place editing
5. Script 3 — PNG export pipeline

## License

Personal project. All rights reserved.

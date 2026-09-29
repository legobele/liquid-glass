# LIQUID GLASS — HTML adaptation

Apple's Liquid Glass material (Human Interface Guidelines, 2025), adapted to HTML/CSS. Not the real dynamic material — a faithful static approximation using `backdrop-filter`, layered specular shadows, rim lighting, and springy motion.

## The doctrine (straight from the HIG)

| # | Rule | How it's honored here |
|---|------|----------------------|
| 1 | **Two layers.** Content layer vs floating functional layer | Demo page: aurora content scrolls *under* the floating toolbar + tab bar |
| 2 | **Glass belongs to the functional layer only** — tab bars, sidebars, toolbars, docks, sheets | `.lg-glass` is used on toolbar, tab bar, sheet. Cards/rows use `.lg-material` (standard frosted material), never glass |
| 3 | **Never stack glass on glass** | One glass layer at a time; icons and labels on glass use plain fills |
| 4 | **Regular vs clear.** Regular (blur + luminosity) for text; clear only over rich media | `.lg-glass` = regular, `.lg-glass-clear` = clear, demoed over a gradient |
| 5 | **Controls get rounder and concentric** | Everything interactive is a capsule (`--lg-radius-pill`); sheets get the largest radii |
| 6 | **Solid fills belong ON glass** | Selected tab and primary buttons are solid fills sitting on the glass |
| 7 | **Be judicious with color** so content shines | Neutrals + translucency; color lives in the content underneath |

## Files

```
tokens.css      — the material as custom properties (blur, specular, rim, radii, springs)
components.css  — toolbar, tab bar, dock, buttons, segmented, sheet, cards, rows, sheen/lens effects
index.html      — live demo: aurora content, floating toolbar + tab bar, sheet, theme toggle
```

## Usage

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/legobele/liquid-glass@main/tokens.css">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/legobele/liquid-glass@main/components.css">
```

Or vendor the two CSS files. No build step, no dependencies. A few lines of JS in the demo handle theme toggle, sheet, segmented state, and the pointer-tracked specular highlight (`.lg-lens` reads `--mx`/`--my`).

## The material, decomposed

```css
.lg-glass {
  backdrop-filter: blur(26px) saturate(1.7) brightness(1.06);
  background: linear-gradient(135deg, rgba(255,255,255,.32), rgba(255,255,255,.10) 45%, rgba(255,255,255,.22));
  border: 1px solid rgba(255,255,255,.45);          /* rim light */
  box-shadow:
    inset 0 1px 1px rgba(255,255,255,.75),          /* top specular edge */
    inset 0 -1px 2px rgba(255,255,255,.14),
    inset 1px 0 1px rgba(255,255,255,.22),
    inset -1px 0 1px rgba(255,255,255,.22),
    0 16px 48px rgba(10,10,30,.28);                /* float shadow */
}
```

Plus `.lg-sheen` (light sweep on hover) and `.lg-lens` (specular highlight follows the pointer).

## Motion — liquid, not fades

- **Gooey indicators**: tab bar + segmented thumbs are two blobs under an SVG
  goo filter (`#lg-goo`). The leader snaps to the new tab, the trailer lags
  70ms, and the filter melts them together mid-travel — the pill *stretches*
  like liquid instead of fading. Mark the container `.lg-tabbar-goo` /
  `.lg-segmented-goo` and call `moveGoo(track, el)` on selection.
- **Jelly press**: `.lg-jelly` squash-and-stretches on `:active`
  (1.18×0.8 → 0.9×1.1 → settle) with a wobble spring.
- **Living background**: aurora blobs morph `border-radius` + drift on
  14–22s loops so the backdrop never sits still.
- **Sheet**: opens on an overshoot spring (`--lg-spring-bouncy`).
- All motion respects `prefers-reduced-motion`.

## Honest limitations

- Real Liquid Glass refracts dynamically per-pixel; CSS `backdrop-filter` blurs but doesn't bend light. The approximation reads correctly at a glance.
- `backdrop-filter` needs a real backdrop — over flat solid colors the effect is subtle. Give it rich content.
- Dark mode is included (`data-theme="dark"`); `prefers-reduced-motion` stops the aurora.

## Sources

- Apple HIG — Materials: developer.apple.com/design/human-interface-guidelines/materials
- Adopting Liquid Glass: developer.apple.com/documentation/TechnologyOverviews/adopting-liquid-glass
- Apple Design Resources (Figma/Sketch UI kits): developer.apple.com/design/resources

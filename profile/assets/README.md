# Brand assets

The Ezekiel Labs mark, mascot, and palette. Everything here is MIT-licensed
along with the rest of this repository.

## The idea

The eye mark is one eye split down the middle: the left half red, the right half
blue, the pupil purple where they meet. That is the whole thesis as a glyph —
red and blue are two readings of the same event, and the interesting part is
where they overlap. Zeke, the owl, carries the same split across his two eyes.

## Palette

[Tokyo Night](https://github.com/enkia/tokyo-night-vscode-theme). Reuse these
exact values in anything new so screenshots, terminal output, and the website
stay one system.

| Role | Hex | Where it is used |
|---|---|---|
| Background | `#1a1b26` | Canvas for every mark; terminal background in demos |
| Offense / red | `#f7768e` | Left half of the eye; loud actions, high detectability |
| Defense / blue | `#7aa2f7` | Right half of the eye; detections, coverage |
| Overlap / purple | `#bb9af7` | The pupil; anything that is genuinely both |
| Foreground | `#c0caf5` | Primary text |
| Foreground, dim | `#a9b1d6` | Secondary text |
| Muted | `#565f89` | Borders, rules, de-emphasized text |

## Files

| File | Use |
|---|---|
| `owl.svg` | Zeke. The org profile header. |
| `eye-mark.svg` | The two-tone eye. Product headers, favicons, README hero. |
| `eye-mark-mono.svg` | Single-color eye, for places that cannot render two tones — workflow-template icons, small monochrome contexts. |
| `favicon.svg`, `favicon-{16,32,48,180,512}.png` | Browser and touch icons. |
| `og-image.svg`, `og-image.png` | Social preview. Carries the wordmark and *"what would a defender see?"* |
| `org-avatar-owl-512.png` | The organization avatar on github.com. |

## Rules of thumb

- Prefer the SVG; the PNGs exist only where a raster is required.
- Never recolor the eye halves. Red-left/blue-right is the meaning, not a style.
- On a light background, keep the `#1a1b26` field behind the mark rather than
  dropping it — the marks are designed against dark.

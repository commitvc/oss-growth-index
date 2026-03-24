# Landing Page — Design Spec

> "Coming soon" landing page for the OSS Growth Index.
> Single-file HTML, deployed via GitHub Pages to `ossgrowthindex.com`.

---

## Layout

Centered vertically and horizontally. Single column, no scroll (`overflow: hidden`).

```
┌─────────────────────────────────────────────────────┐
│                                                     │
│            ⟨ full-screen canvas layer ⟩             │
│                                                     │
│       [Supabase logo]  ×  [>commit logo]            │
│                                                     │
│                  coming soon...                     │
│                                                     │
│       [ your@email.com    ] [ notify me ]           │
│                                                     │
│          indexing_  ████░░░░░  2/6                   │
│                                                     │
│            ⟨ CRT scanline overlay ⟩                 │
│            ⟨ animated favicon ⟩                     │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Stacking order (z-index)**:
- `0` — Canvas (icon discovery game, `pointer-events: none`)
- `1` — `.container` (logos, text, form)
- `9999` — CRT scanline overlay (`pointer-events: none`)

---

## Color System

| Token | Value | Usage |
|---|---|---|
| Background | `#0a0a0a` | Body background |
| Foreground | `#fafafa` | Primary text, wordmarks |
| Muted text | `rgba(255,255,255,0.35)` | "coming soon", separator |
| Subtle border | `rgba(255,255,255,0.1)` | Input/button borders |
| Subtle fill | `rgba(255,255,255,0.05)` | Input background |
| Button fill | `rgba(255,255,255,0.08)` | Button background |
| Button hover | `rgba(255,255,255,0.12)` | Button hover background |
| Error | `rgba(255,80,80,0.7)` | Validation error text |
| Error border | `rgba(255,80,80,0.3)` | Invalid input border |
| Supabase green | `#3ECF8E` | Supabase logo, glow |
| Commit red | `#FA0C08` / `#fa0d07` | >commit logo, glow |
| Insider amber | `#c9952a` | Unlocked state accent |
| Icon grey | `#555` – `#666` | Canvas icon pixels |
| Loader grey | `#333` – `#555` | Loader track/fill |
| CRT overlay | `rgba(0,0,0,0.02)` | Scanline stripes |

---

## Typography

| Element | Family | Size | Weight | Tracking |
|---|---|---|---|---|
| Supabase wordmark | Inter | 21px | 400 | -0.01em |
| >commit wordmark | JetBrains Mono | 21px | 400 | -0.02em |
| Separator (×) | Inter | 15px | 300 | 0.08em |
| "coming soon" | Inter | 15px | 300 | 0.08em |
| Email input | Inter | 14px | 400 | — |
| Button | Inter | 14px | 400 | — |
| Error text | JetBrains Mono | 11px | 400 | 0.02em |
| Loader label | JetBrains Mono | 11px | 400 | 0.05em |
| Loader counter | JetBrains Mono | 10px | 400 | — |
| Success message | Inter | 14px | 400 | — |

**Font loading**: Google Fonts, `Inter` (300, 400, 500) + `JetBrains Mono` (400, 500). Preconnect to `fonts.googleapis.com` and `fonts.gstatic.com`.

---

## Logo Row

Two logo lockups separated by a `×` character.

**Supabase**: SVG icon (109×113 viewBox, green gradient) + "supabase" wordmark.
**>commit**: SVG icon (40×41 viewBox, red square with white shapes) + ">commit" wordmark (the `>` is an HTML entity).

Each logo wrapper has a soft glow via `::after` pseudo-element: 20px blur, 8% opacity, colored by brand.

**Spacing**: 32px gap between the three elements (logo, separator, logo). Container gap between sections: 48px.

---

## Email Capture Form

Horizontal layout: text input + submit button.

| Property | Input | Button |
|---|---|---|
| Border radius | 8px | 8px |
| Padding | 10px 16px | 10px 20px |
| Width | 260px | auto |
| Border | 1px subtle | 1px subtle |
| Focus state | border brightens to 0.25 | — |
| Hover state | — | fill/border/text brighten |

**Validation error**: Positioned absolutely below the input (`top: calc(100% + 6px)`). Red monospace text, hidden by default.

**Success message**: Replaces the entire form. Fades in (`opacity 0→1, 0.3s`). Default color white, amber for insider.

---

## Loader Bar

Retro terminal-style progress indicator. Hidden by default, fades in after first icon discovery.

```
indexing_  ████░░░░░  2/6
  label     track    counter
```

- **Label**: "indexing" with a blinking underscore cursor (1s step animation)
- **Track**: 80×8px, 1px `#333` border, `#111` background
- **Fill**: Grey `#555`, width transitions smoothly (1.2s cubic-bezier)
- **Scanline effect**: Repeating vertical lines on the fill via `::after`
- **Counter**: `discovered/total` format

**Positioning**: Appears in the email-capture flex column with `order: -1` (above form).

---

## CRT Scanline Overlay

Full-viewport `body::after` pseudo-element. Repeating horizontal gradient:
- 2px transparent
- 2px at 2% black opacity

Creates subtle horizontal line pattern over everything.

---

## Animated Favicon

Alternates every 2 seconds between:
1. Supabase bolt icon (green on dark, `#0a0a0a` background)
2. >commit logo (red background, white shapes)

Both are inline data URI SVGs.

---

## Responsive (≤768px)

- Canvas game: **hidden** (`display: none`)
- Loader bar: **hidden** (`display: none`)
- Logo gap: 20px (from 32px)
- Supabase SVG: 22px height (from 24px)
- Form: stacked vertically, full width, 24px horizontal padding
- Input: full width

---

## Insider (Amber) Theme

When the easter egg is unlocked, amber (`#c9952a`) replaces the default palette for interactive elements:

| Element | Change |
|---|---|
| Button background | `rgba(201,149,42,0.12)` |
| Button border | `rgba(201,149,42,0.35)` |
| Button text | `#fff`, weight 500 |
| Button hover | fill 0.22, border 0.5 |
| Input border | `rgba(201,149,42,0.2)` |
| Input focus border | `rgba(201,149,42,0.35)` |
| Loader fill | `#c9952a` |
| Loader label | "unlocked", amber text |
| Discovered icon dots | amber instead of white |
| Button glow | 4s pulsing `box-shadow` |

---

## Backlog

- ~~**Submit error tooltip**~~: Done — shows `"oops, try again"` via existing `emailError` element on network failure. Clears on next submit attempt.
- ~~**Inline all icon SVGs**~~: Done — all 6 icons use inline `{ vb: 24, d }` path data sourced from SimpleIcons. No external assets. HiDPI rendering fixed via `devicePixelRatio`-aware canvas setup.

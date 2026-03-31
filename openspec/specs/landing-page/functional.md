# Landing Page — Functional Spec

> Behavior and interaction design for the OSS Growth Index "coming soon" page.

---

## Overview

The page has two functional layers:

1. **Email capture** — collect waitlist signups, surface layer
2. **Icon discovery game** — hidden canvas-based easter egg that unlocks an "explorer" tier

Both are interconnected via a shared state machine persisted in `localStorage`.

---

## Osscar Nav Badge

A fixed top-left badge displaying the Osscar brand. Always visible.

- Position: `fixed`, top-left (`left: 24px`, `top: 20px`)
- Layout: small robot icon (PNG, 24px tall) + "Osscar" wordmark
- Default state: white at 50% opacity (icon), 40% opacity (label)
- Explorer unlocked: both icon and label transition to amber `#F59E0B` over 1.4s
- Assets: `oscar-white.png` (default), `oscar-amber.png` (unlocked) — crossfaded via opacity

---

## Email Capture

### Flow

1. Visitor enters email → clicks "notify me"
2. Client-side validation runs
3. On success: POST to backend (fire-and-forget, `no-cors`)
4. Form replaced by confirmation message: "you are on the list"
5. State persisted to `localStorage`

### Validation

| Check | Error message | Visual |
|---|---|---|
| Empty field | "enter your email" | Red border + error text below input |
| Invalid format | "invalid email format" | Red border + error text below input |

Regex: `/^[^\s@]+@[^\s@]+\.[^\s@]+$/`

Error clears on any input change (keypress). Positioned absolutely below the input field.

### Submit States

All button text is lowercase.

| State | Button text | Button disabled |
|---|---|---|
| Default | "notify me" | no |
| Submitting | "..." | yes |
| Success | form replaced by message | — |
| Error (network) | restores contextual text (see below) | no |

On submit failure, the form stays visible with the button re-enabled. The `emailError` element shows `"oops, try again"` — same position and red style as validation errors. Clears on the next submit attempt (not on input change).

---

## Icon Discovery Game (Desktop Only)

Six ecosystem icons are hidden across the page, revealed by mouse proximity.

### Icons

All icons sourced from [SimpleIcons](https://simpleicons.org) — clean, official single-path SVGs on a uniform `24×24` viewBox.

| # | Icon | Slug |
|---|---|---|
| 1 | GitHub | `github` |
| 2 | npm | `npm` |
| 3 | Hugging Face | `huggingface` |
| 4 | Discord | `discord` |
| 5 | PyPI | `pypi` |
| 6 | Docker | `docker` |

### Rendering

Icons are rasterized to offscreen canvases at **40×40 CSS pixels**, filled in grey (`#666`). On HiDPI/retina displays, offscreen canvases are physically `40 × devicePixelRatio` pixels and the main canvas context is scaled accordingly — icons remain sharp at all display densities.

An amber (`#F59E0B`) offscreen canvas is also pre-rendered per icon for the explorer-unlock state.

The path data is inlined in the JS (`{ vb: 24, d: '...' }`) — no external assets.

### Placement

Icons are placed one-per-zone across 6 screen regions. Zones avoid:
- **Top-left** (Osscar nav badge area, ~0–22% x, 0–25% y)
- **Center UI** (~25–75% x, 15–85% y — email form, logos, loader)

```
┌──────────────────────────────────────────────────────────┐
│ [OSSCAR]  │      top-center          │    top-right      │
│  AVOID    │  26–74% x · 4–16% y     │ 78–96% x·4–34% y  │
├───────────┤──────────────────────────┼───────────────────┤
│ left-mid  │                          │    right-mid      │
│ 2–20% x   │     CENTER UI            │  80–97% x         │
│ 28–58% y  │       AVOID              │  38–68% y         │
├───────────┤                          ├───────────────────┤
│ bot-left  │──────────────────────────│                   │
│ 2–22% x   │     bottom-center        │                   │
│ 65–90% y  │  28–72% x · 84–96% y    │                   │
└──────────────────────────────────────────────────────────┘
```

Zones are shuffled randomly on each load/resize — icon-to-zone assignment varies each visit.

**Minimum separation**: icons are placed at least `SZ × 3.5` px apart (centre-to-centre). Up to 25 retries per icon within its zone to satisfy this constraint.

### Reveal Mechanic

**Reveal radius**: 150px from mouse cursor to icon centre.

**Proximity reveals in 3 phases** (crossfade based on `reveal` value 0→1):

| Phase | reveal range | What renders |
|---|---|---|
| Pixel scatter | 0.0 – 0.6 | Individual pixels with jitter, random skip |
| Crossfade | 0.3 – 0.7 | Pixels fade out, clean raster fades in |
| Full icon | 0.7+ | Clean 40×40 icon, marks as "discovered" |

- Reveal ramps up at `0.08` speed, decays at `0.04` (smooth lerp)
- Pixels jitter by `(1 - reveal) * 4` pixels when not settled
- Random skip: `Math.random() > reveal * 1.5` drops pixels for glitch effect

### Bait System

If the user is idle, undiscovered icons attract attention with flickering pixels.

| Parameter | Value |
|---|---|
| Idle before first bait | 2 seconds |
| Cooldown after discovery | 3 seconds |
| Bait ramp-up duration | 1.5 seconds |
| Max bait intensity | 0.35 |
| Bait timeout (rotate) | 10 seconds, then 2s cooldown |

**Bait target selection**: Prefers undiscovered icons closer to viewport centre (picks randomly from the closest half).

**Bait rendering**: Random subset of icon pixels drawn with scatter (up to 12px jitter) and frame-by-frame flicker (30% random skip). Pixels are grey `#777`. Only renders when mouse is not near the icon (`reveal < 0.05`).

### Post-Discovery

Once discovered (`reveal > 0.7`), an icon **remains permanently visible** at `PERSIST_ALPHA = 0.3` opacity when the cursor is away. No pulsing dot — the icon shape itself is the presence indicator.

- **When mouse is away**: icon drawn at `globalAlpha = 0.3` (amber offscreen when explorer unlocked)
- **When mouse is near**: full reveal lerp takes over, icon brightens to full opacity on hover

---

## Loader Bar

Tracks icon discovery progress. Hidden until the first icon is discovered.

| Event | Behavior |
|---|---|
| First discovery | Loader fades in (0.8s ease), shows `1/6` |
| Each discovery | Fill width animates, counter updates |
| All 6 discovered | Transitions to amber "unlocked" state |

Dimensions: **110px wide × 10px tall** track.

---

## Explorer Easter Egg

Triggered when all 6 icons are discovered. Ephemeral — will be removed when the index app launches.

### Unlock Effects

1. Osscar nav badge transitions to amber (`#F59E0B`) over 1.4s
2. Loader bar fill turns amber, label changes to "unlocked"
3. All discovered icons render from amber offscreen canvas
4. Email form switches to explorer theme (amber borders, button glow)
5. Button text changes based on submission state (see state machine below)

### State Machine

```
┌──────────────────────────────────────────────────────────────┐
│                                                              │
│  localStorage keys:                                          │
│    oss-index-submitted  (bool)                               │
│    oss-index-email      (string)                             │
│    oss-index-explorer   (bool)                               │
│                                                              │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  FRESH VISITOR (nothing in localStorage)                     │
│  → Form: "notify me" (default style)                         │
│                                                              │
│  SUBMITTED, NOT EXPLORER                                     │
│  → Message: "you are on the list" (white)                    │
│  → If they then discover all 6 → "explorer access" button    │
│    (no email input, uses stored email)                       │
│                                                              │
│  EXPLORER, NOT SUBMITTED                                     │
│  → Form: "explorer access" (amber style, email input visible)│
│                                                              │
│  EXPLORER + SUBMITTED                                        │
│  → Message: "you are an explorer" (amber)                    │
│  → "gg, you are now an explorer" shown on first unlock       │
│                                                              │
│  RETURNING EXPLORER (page reload after full unlock)          │
│  → Osscar badge: instant amber                               │
│  → Loader bar: instant amber, "unlocked", 6/6               │
│  → Message: "you are an explorer" (amber, no fade-in)        │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

### Explorer Access Flow

When a user already submitted their email (is on waitlist) and then discovers all 6 icons:
- Email input is hidden
- Only the amber "explorer access" button is shown
- On click: re-submits the stored email with `insider=true` flag
- Confirmation: "gg, you are now an explorer"

---

## Analytics (PostHog)

| Event | Properties | Trigger |
|---|---|---|
| `icon_discovered` | `icon_index`, `total_discovered` | Each icon reveal > 0.7 |
| `email_submitted` | `insider` (bool), `upgrade` (bool) | Successful form submit |

PostHog instance: EU (`eu.i.posthog.com`), person profiles: `identified_only`.

---

## Mobile (≤768px)

| Feature | Behavior |
|---|---|
| Canvas game | Hidden — no icon discovery on mobile |
| Loader bar | Hidden |
| Explorer unlock | Not achievable on mobile |
| Email form | Stacked vertically, full width |
| Logos | Slightly smaller (22px), tighter gap |
| CRT overlay | Still visible |
| Animated favicon | Still active |

This is intentional due to the mouse-dependent reveal mechanic having no touch equivalent.

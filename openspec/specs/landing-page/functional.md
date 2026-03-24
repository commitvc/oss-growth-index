# Landing Page — Functional Spec

> Behavior and interaction design for the OSS Growth Index "coming soon" page.

---

## Overview

The page has two functional layers:

1. **Email capture** — collect waitlist signups, surface layer
2. **Icon discovery game** — hidden canvas-based easter egg that unlocks an "insider" tier

Both are interconnected via a shared state machine persisted in `localStorage`.

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

| State | Button text | Button disabled |
|---|---|---|
| Default | "notify me" | no |
| Submitting | "..." | yes |
| Success | form replaced by message | — |
| Error (network) | restores original text | no |

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

Icons are rasterized to offscreen canvases at **32×32 CSS pixels**, filled in grey (`#666`). On HiDPI/retina displays, offscreen canvases are physically `32 × devicePixelRatio` pixels and the main canvas context is scaled accordingly — icons remain sharp at all display densities.

The path data is inlined in the JS (`{ vb: 24, d: '...' }`) — no external assets.

### Placement

Icons are randomly placed across 6 screen zones on each load/resize:

```
┌────────────┬────────────┬────────────┐
│  top-left  │ top-center │ top-right  │
│ 5-30% x    │ 40-60% x   │ 70-95% x   │
│ 8-35% y    │ 5-20% y    │ 8-35% y    │
├────────────┤            ├────────────┤
│            │  (center   │            │
│            │   clear)   │            │
├────────────┤            ├────────────┤
│ bottom-left│ bot-center │bottom-right│
│ 3-25% x    │ 35-65% x   │ 75-97% x   │
│ 60-88% y   │ 82-95% y   │ 60-88% y   │
└────────────┴────────────┴────────────┘
```

Zones are shuffled randomly; each icon gets one zone. The center of the viewport is deliberately kept clear (that's where the UI lives).

### Reveal Mechanic

**Reveal radius**: 150px from mouse cursor to icon center.

**Proximity reveals in 3 phases** (crossfade based on `reveal` value 0→1):

| Phase | reveal range | What renders |
|---|---|---|
| Pixel scatter | 0.0 – 0.6 | Individual pixels with jitter, random skip |
| Crossfade | 0.3 – 0.7 | Pixels fade out, clean raster fades in |
| Full icon | 0.7+ | Clean 32×32 icon, marks as "discovered" |

- Reveal ramps up at `0.08` speed, decays at `0.04` (smooth lerp)
- Pixels jitter by `(1 - reveal) * 4` pixels when not settled
- Random skip: `Math.random() > reveal * 1.5` drops pixels for glitch effect

### Bait System

If the user is idle, undiscovered icons attract attention with flickering pixels.

| Parameter | Value |
|---|---|
| Idle before first bait | 5 seconds |
| Cooldown after discovery | 3 seconds |
| Bait ramp-up duration | 1.5 seconds |
| Max bait intensity | 0.35 |
| Bait timeout (rotate) | 10 seconds, then 2s cooldown |

**Bait target selection**: Prefers undiscovered icons closer to viewport center (picks randomly from the closest half).

**Bait rendering**: Random subset of icon pixels drawn with scatter (up to 12px jitter) and frame-by-frame flicker (30% random skip). Pixels are grey `#777`. Only renders when mouse is not near the icon (`reveal < 0.05`).

### Post-Discovery

Once discovered (`reveal > 0.7`), an icon shows:
- **When mouse is away**: Pulsing dot (3.5px radius, sinusoidal opacity 0.05–0.55, period ~5s per icon offset). White by default, amber when insider is unlocked.
- **When mouse is near**: Full icon re-reveals on hover

---

## Loader Bar

Tracks icon discovery progress. Hidden until the first icon is discovered.

| Event | Behavior |
|---|---|
| First discovery | Loader fades in (0.8s ease), shows `1/6` |
| Each discovery | Fill width animates, counter updates |
| All 6 discovered | Transitions to amber "unlocked" state |

---

## Insider Easter Egg

Triggered when all 6 icons are discovered. Ephemeral — will be removed when the index app launches.

### Unlock Effects

1. Loader bar turns amber: fill `#c9952a`, label changes to "unlocked"
2. All discovered icon dots turn amber
3. Email form switches to insider theme (amber borders, button glow)
4. Button text changes based on submission state (see state machine below)

### State Machine

```
┌──────────────────────────────────────────────────────────────┐
│                                                              │
│  localStorage keys:                                          │
│    oss-index-submitted  (bool)                               │
│    oss-index-email      (string)                             │
│    oss-index-insider    (bool)                               │
│                                                              │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  FRESH VISITOR (nothing in localStorage)                     │
│  → Form: "notify me" (default style)                         │
│                                                              │
│  SUBMITTED, NOT INSIDER                                      │
│  → Message: "you are on the list" (white)                    │
│  → If they then discover all 6 → "upgrade me" button         │
│    (no email input, uses stored email)                       │
│                                                              │
│  INSIDER, NOT SUBMITTED                                      │
│  → Form: "I want in" (amber style, email input visible)      │
│                                                              │
│  INSIDER + SUBMITTED                                         │
│  → Message: "you are an insider" (amber)                     │
│  → "gg, you are now an insider" shown on first unlock        │
│                                                              │
│  RETURNING INSIDER (page reload after full unlock)           │
│  → Loader bar: instant amber, "unlocked", 6/6                │
│  → Message: "you are an insider" (amber, no fade-in)         │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

### Upgrade Flow

When a user already submitted their email (is on waitlist) and then discovers all 6 icons:
- Email input is hidden
- Only the amber "upgrade me" button is shown
- On click: re-submits the stored email with `insider=true` flag
- Confirmation: "gg, you are now an insider"

---

## Analytics (PostHog)

| Event | Properties | Trigger |
|---|---|---|
| `icon_discovered` | `icon_index`, `total_discovered` | Each icon reveal > 0.7 |
| `email_submitted` | `insider` (bool), `upgrade` (bool) | Successful form submit |

PostHog instance: EU (`eu.i.posthog.com`), person profiles: `identified_only`.

---

## Backlog

- **Share moment on insider unlock**: When a user unlocks insider status, surface a subtle share prompt — pre-filled tweet (e.g. "I found all 6 hidden icons on ossgrowthindex.com") to drive organic pre-launch traffic. Should feel optional and non-intrusive, matching the understated tone of the page.

---

## Mobile (≤768px)

| Feature | Behavior |
|---|---|
| Canvas game | Hidden — no icon discovery on mobile |
| Loader bar | Hidden |
| Insider unlock | Not achievable on mobile |
| Email form | Stacked vertically, full width |
| Logos | Slightly smaller (22px), tighter gap |
| CRT overlay | Still visible |
| Animated favicon | Still active |

This is intentional due to the mouse-dependent reveal mechanic having no touch equivalent.

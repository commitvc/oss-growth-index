## Context

The canvas render loop currently handles discovered icons in a single branch: draw a small pulsing dot, then fade out the `offscreen` canvas at low opacity as the cursor leaves. The icon effectively disappears to near-invisible at idle. The insider CTA string "I want in" is a hardcoded `textContent` set in `showForm('insider')`. The loader bar height is set in CSS.

## Goals / Non-Goals

**Goals:**
- Discovered icons render at a persistent, non-zero base opacity when the cursor is away — the full icon shape remains legible
- Hovering still re-reveals to full brightness (the existing reveal mechanic is unchanged)
- Insider CTA reads "Explorer access"
- Loader bar is slightly taller (visual weight increase, not a layout change)

**Non-Goals:**
- Persisting discovery state across page reloads
- Animating the transition from dot to persistent icon (instant change on discover is fine)
- Changing the pulsing dot behaviour on the dot itself (can remove or keep as a subtle pulse on top)

## Decisions

**Persistent opacity: fixed alpha on `drawImage`, no dot**
Replace the discovered-idle branch (pulsing dot + fading `drawImage`) with a single `drawImage` call at a fixed `globalAlpha` (~0.25–0.35). The dot added visual noise for a mechanic that no longer needs it — the icon itself is the presence indicator now. On hover the existing smooth lerp to `reveal = 1` takes over naturally.

Alternative considered: keep the dot and overlay it on the persistent icon. Rejected — two competing visual elements at the same position, no added value.

**Opacity value: 0.3**
High enough to read the icon shape clearly against the dark background; low enough to feel "ambient" rather than fully revealed. Exact value to be validated visually; range 0.25–0.40 is acceptable.

**Button copy: update `showForm` string literal only**
"I want in" → "Explorer access". Single string change in the JS `showForm` function. No CSS changes needed (the amber insider style applies to the button element, not its text).

**Loader bar height: CSS only, increase from current to ~6–8px**
Current value to be confirmed by reading the CSS. A 2–4px increase is sufficient — the bar is a secondary element and shouldn't compete with the email form.

## Risks / Trade-offs

- [Persistent icon at 0.3 alpha may feel too bright on first discovery] → If so, reduce to 0.25. Can be tuned with a single constant.
- [Removing the dot removes the "found X icons" ambient count feel] → The persistent icons themselves serve that function now; each found icon stays visible as a record of progress.
- [Loader bar height change could shift layout on small viewports] → Bar is absolutely/fixed positioned or in a flex row; height increase is contained. Verify on mobile (though bar is hidden on mobile).

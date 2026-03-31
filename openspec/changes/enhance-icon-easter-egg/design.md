## Context

The landing page has a canvas-based easter egg: 6 ecosystem icons (GitHub, npm, PyPI, Discord, Docker, Hugging Face) hidden in a grid. Icons are revealed on hover but disappear when the cursor leaves. All icons are currently rendered at 24×24 px and are always grey. There is no state tracking per icon.

## Goals / Non-Goals

**Goals:**
- Increase icon render size for better visual impact
- Track which icons have been discovered in the current session
- Keep discovered icons visible after the cursor leaves
- Trigger an amber color transition on all icons when all 6 are discovered

**Non-Goals:**
- Persisting discovery state across page reloads (session-only)
- Animating individual icon discovery (beyond existing hover reveal)
- Any changes to the share moment / tweet prompt (separate backlog item)

## Decisions

**State: discovered set in JS array**
Track discovered icons as a boolean array (one entry per icon, index-matched to the icons array). Simple, no DOM dependency, survives re-renders. Initialized to all `false` on page load.

**Size: bump from 24 to 32 px icons within the same canvas grid**
The grid cell size and canvas dimensions should scale proportionally. Keeps the layout intact without reflowing the page.

**Amber trigger: check discovered count on every hover**
After each icon discovery, check if `discovered.every(Boolean)`. If true, set a global `allUnlocked = true` flag. The render loop reads this flag and substitutes `#F59E0B` (Tailwind amber-400) for `#666666` as the fill color for all icons.

**Color: amber-400 (`#F59E0B`)**
Warm, celebratory, and contrasts cleanly with the dark background. Consistent with common "unlock" / achievement UI conventions.

## Risks / Trade-offs

- [Canvas re-render on every frame] → Already the existing pattern; no new cost introduced
- [Discovered state lost on reload] → Intentional for now; easter egg stays fresh on each visit
- [Amber applies to all icons at once] → Creates a collective "gg" moment, which matches the page's voice

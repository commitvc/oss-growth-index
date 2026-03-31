## Why

The easter egg icon mechanic is the page's most delightful interaction, but icons disappear after the hover ends and feel small. Improving persistence and visual reward makes the discovery feel more meaningful.

## What Changes

- Increase icon size in the canvas grid
- Keep icons visible (persistent) once hovered/discovered
- Turn all 6 icons amber when the full set is discovered — a collective unlock moment

## Capabilities

### New Capabilities

- `icon-persistence`: Icons stay visible after discovery; canvas tracks which icons have been found across hover events
- `icon-unlock-celebration`: When all 6 icons are discovered, all icons transition to amber as a visual reward

### Modified Capabilities

- `icon-easter-egg`: Icon size increases; rendering logic extended to support per-icon discovered state and the amber celebration state

## Impact

- `index.html` — canvas rendering loop, icon data structure, hover/interaction logic
- No external dependencies, no API changes

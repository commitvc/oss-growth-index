## 1. Icon Size

- [x] 1.1 Increase icon cell size from 24 to 32 px in the canvas grid
- [x] 1.2 Update canvas width/height to match the new cell size and grid layout
- [x] 1.3 Verify all 6 icons render correctly at the new size

## 2. Icon Persistence

- [x] 2.1 Add a `discovered` boolean array (one entry per icon, all `false` on init)
- [x] 2.2 On hover, set `discovered[i] = true` for the hovered icon
- [x] 2.3 Update the render loop: draw an icon if `discovered[i]` OR cursor is over its cell

## 3. Amber Celebration

- [x] 3.1 Add an `allUnlocked` flag, set to `true` when `discovered.every(Boolean)`
- [x] 3.2 In the render loop, use `#F59E0B` as fill color when `allUnlocked`, else `#666666`
- [x] 3.3 Verify all 6 icons turn amber immediately on the final discovery hover

## 1. Icon Persistent Reveal

- [x] 1.1 Add `PERSIST_ALPHA = 0.3` constant near the top of the icon canvas block
- [x] 1.2 In the animate loop, replace the discovered-idle branch (pulsing dot + fading drawImage) with a single `drawImage` at `globalAlpha = PERSIST_ALPHA`, using `amberOffscreen` when `explorerUnlocked`
- [x] 1.3 Verify the hover re-reveal lerp still takes over naturally when cursor approaches a persisted icon

## 2. CTA Copy

- [x] 2.1 In `showForm('explorer')`, change `btn.textContent` from `'I want in'` to `'Explorer access'`

## 3. Loader Bar Size

- [x] 3.1 Find the CSS rule for `.loader-fill` or `.loader-bar` height and increase it to 8px
- [x] 3.2 Verify bar remains hidden on mobile (≤768px breakpoint unchanged)

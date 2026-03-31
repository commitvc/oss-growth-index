## ADDED Requirements

### Requirement: Discovered icons render persistently at idle
Once an icon is marked as discovered, the canvas SHALL render the icon at a fixed base opacity (`PERSIST_ALPHA = 0.3`) whenever the cursor is not near it — replacing the pulsing-dot idle state.

#### Scenario: Icon stays visible after cursor leaves
- **WHEN** an icon has been discovered and the cursor moves away (reveal decays below 0.05)
- **THEN** the icon SHALL be drawn at `globalAlpha = PERSIST_ALPHA` on every animation frame

#### Scenario: Hover re-reveals to full brightness
- **WHEN** the cursor approaches a discovered icon (reveal rises above 0)
- **THEN** the reveal lerp SHALL take over, smoothly bringing the icon to full opacity, overriding the persistent base

#### Scenario: Amber color applies to persistent render
- **WHEN** `insiderUnlocked` is true and a discovered icon is in persistent-idle state
- **THEN** the icon SHALL be drawn from `amberOffscreen` at `PERSIST_ALPHA`

### Requirement: Pulsing dot is removed from discovered-idle state
The small pulsing dot previously drawn at the icon center when idle SHALL be removed. The persistent icon render is the sole presence indicator.

#### Scenario: No dot drawn at idle
- **WHEN** an icon is discovered and the cursor is away
- **THEN** no circle/dot element SHALL be drawn at the icon's center coordinates

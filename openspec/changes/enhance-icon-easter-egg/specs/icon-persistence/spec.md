## ADDED Requirements

### Requirement: Icons persist after discovery
Once an icon has been revealed by hovering, it SHALL remain visible (rendered at full opacity) for the remainder of the page session, regardless of cursor position.

#### Scenario: Icon stays visible after cursor leaves
- **WHEN** the user hovers over an icon cell, revealing it
- **THEN** moving the cursor away SHALL NOT hide the icon — it remains rendered

#### Scenario: Undiscovered icons remain hidden
- **WHEN** an icon has not yet been hovered
- **THEN** it SHALL NOT be visible unless the cursor is currently over its cell

#### Scenario: Discovery state resets on reload
- **WHEN** the user reloads the page
- **THEN** all icons SHALL revert to hidden (discovery state is session-only, not persisted)

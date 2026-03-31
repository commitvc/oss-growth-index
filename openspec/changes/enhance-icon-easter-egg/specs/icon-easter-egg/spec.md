## MODIFIED Requirements

### Requirement: Icon render size
Icons in the canvas grid SHALL be rendered at 32×32 px (up from 24×24 px). The canvas grid cell size and overall canvas dimensions SHALL scale proportionally so the layout remains consistent.

#### Scenario: Icons render at correct size
- **WHEN** the canvas is initialized
- **THEN** each icon cell SHALL be sized to 32×32 px and the canvas dimensions SHALL match the grid at that cell size

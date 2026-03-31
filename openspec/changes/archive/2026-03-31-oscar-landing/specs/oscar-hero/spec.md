## ADDED Requirements

### Requirement: Oscar statuette displayed as hero element
The Oscar SVG statuette SHALL be displayed as the primary visual element of the landing page, inlined as an `<svg>` tag so it is animatable and colorable via CSS.

#### Scenario: Statuette visible on page load
- **WHEN** the page loads
- **THEN** the Oscar statuette SVG SHALL be visible, centered, above the main tagline

#### Scenario: Statuette rendered in gold/amber
- **WHEN** the statuette is rendered
- **THEN** its fill color SHALL be amber (`#F59E0B`) to evoke the gold Oscar award

### Requirement: Entrance animation on load
The statuette SHALL animate in on page load using CSS only (no JS).

#### Scenario: Fade and float-up on load
- **WHEN** the page finishes loading
- **THEN** the statuette SHALL fade in with a subtle upward translate over ~600ms

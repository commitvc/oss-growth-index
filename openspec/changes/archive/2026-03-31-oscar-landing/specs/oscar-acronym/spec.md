## ADDED Requirements

### Requirement: Acronym expansion displayed on the page
The full expansion of OSCAR SHALL be displayed: "Open Source Supabase Commit Analytical Ranking". It SHALL be presented in a poetic, stacked layout where each line corresponds to one word of the acronym.

#### Scenario: Acronym visible below or near the hero
- **WHEN** the page loads
- **THEN** the acronym expansion SHALL be visible, positioned near the Oscar statuette or below the tagline

### Requirement: Initial letters visually highlighted
Each initial letter (O, S, C, A, R) SHALL be visually distinguished — rendered in amber (`#F59E0B`) — so the acronym reads as a poem while the letters spell OSCAR.

#### Scenario: First letter of each word is amber
- **WHEN** the acronym is rendered
- **THEN** the first letter of each word SHALL appear in amber, with the rest of each word in the standard muted color

### Requirement: Acronym reads as a reveal, not a definition
The layout SHALL feel intentional and editorial — not a tooltip, legend, or footnote. Line-height, spacing, and font weight SHALL reinforce the poem-like cadence.

#### Scenario: Each word on its own line
- **WHEN** the acronym block is rendered
- **THEN** each word SHALL occupy its own line with consistent left-alignment and generous line spacing

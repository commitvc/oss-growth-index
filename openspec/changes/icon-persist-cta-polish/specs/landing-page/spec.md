## MODIFIED Requirements

### Requirement: Submit States
The system SHALL manage button text across all form states as follows:

| State | Button text | Button disabled |
|---|---|---|
| Default | "get early access" | no |
| Submitting | "..." | yes |
| Success | form replaced by message | — |
| Error (network) | restores original text | no |

#### Scenario: Default button text on fresh visit
- **WHEN** a visitor loads the page with no prior localStorage state
- **THEN** the submit button SHALL display "get early access"

#### Scenario: Insider button text
- **WHEN** a visitor has discovered all 6 icons but has not yet submitted their email
- **THEN** the submit button SHALL display "Explorer access"

#### Scenario: Upgrade button text
- **WHEN** a visitor has already submitted their email and then discovers all 6 icons
- **THEN** the button SHALL display "upgrade me" (no email input visible)

## MODIFIED Requirements

### Requirement: Loader Bar Visual
The loader bar SHALL be rendered at a height of **8px** (increased from prior value). All other loader bar behaviors (hidden until first discovery, fill animation, amber unlock state) remain unchanged.

#### Scenario: Bar height at discovery
- **WHEN** the first icon is discovered and the loader bar fades in
- **THEN** the bar SHALL have a visible height of 8px

#### Scenario: Bar hidden on mobile
- **WHEN** viewport width is ≤768px
- **THEN** the loader bar SHALL remain hidden regardless of height value

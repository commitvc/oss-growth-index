## ADDED Requirements

### Requirement: Amber celebration on full unlock
When all 6 ecosystem icons have been discovered in the current session, all icons SHALL immediately transition to amber (`#F59E0B`) and remain amber for the rest of the session.

#### Scenario: All icons turn amber after final discovery
- **WHEN** the user hovers the last undiscovered icon
- **THEN** all 6 icons SHALL render in amber (`#F59E0B`) instead of grey (`#666666`)

#### Scenario: Amber persists after full unlock
- **WHEN** all icons are unlocked and the user moves the cursor away
- **THEN** all icons SHALL remain amber — the celebration state is permanent for the session

#### Scenario: Partial discovery does not trigger amber
- **WHEN** fewer than 6 icons have been discovered
- **THEN** discovered icons SHALL render in grey (not amber)

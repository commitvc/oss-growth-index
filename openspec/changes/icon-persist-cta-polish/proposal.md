## Why

The icon discovery game is the page's most memorable interaction, but discovered icons vanish to a barely-noticeable dot — the reward feels transient. The insider CTA also uses generic copy that doesn't reflect the player's accomplishment. A few focused touches make the discovery feel durable and the moment of unlock feel earned.

## What Changes

- Discovered icons remain fully visible (at a persistent opacity) rather than collapsing to a pulsing dot — the player's map of the page builds up as they explore
- Insider button copy changes from "I want in" → "Explorer access" — equivocal: nods to the icon hunt, also means requesting access
- Progress / loader bar grows slightly in height for better visual presence (it earns more real estate once it's the primary feedback element)

## Capabilities

### New Capabilities

- `icon-permanent-reveal`: Once discovered, an icon stays rendered at a fixed visible opacity; hovering still re-reveals to full brightness

### Modified Capabilities

- `landing-page`: Loader bar height increases; insider CTA button text changes

## Impact

- `index.html` — canvas render loop (discovered-state rendering), loader bar CSS, insider button text constant
- No external dependencies, no API changes

## Context

The landing page at oscar.dev needs to introduce the Oscar brand. The page is a single `index.html` with all styles and logic inline — no build step, no framework. The Oscar statuette SVG is a custom asset that needs to be inlined. The existing coming-soon structure (email signup, ecosystem icons easter egg) is preserved.

## Goals / Non-Goals

**Goals:**
- Make the Oscar statuette SVG the visual hero of the page
- Present the acronym expansion ("Open Source Supabase Commit Analytical Ranking") in a memorable, poetic way
- Keep the page feeling minimal and intentional — not a wall of text

**Non-Goals:**
- Replacing the email signup or easter egg mechanics
- Full rebrand of all copy (coming-soon structure stays)
- Animation framework or external library

## Decisions

**Oscar SVG: inlined as `<svg>` in HTML, not as `<img>`**
Inlining lets us animate, colorize, and control individual paths via CSS. Same approach used for all other icons in the project. The statuette becomes a live DOM element.

**Acronym presentation: stacked poem layout**
Each word of the acronym on its own line, with the initial letter highlighted in amber or accent color. Reads like an awards ceremony reveal, not a tooltip or footnote. Example:

```
O pen Source
S upabase
C ommit
A nalytical
R anking
```

Alternatively, a two-column layout (letter | word) for precision. Decision deferred to implementation once SVG and visual context are available.

**Hero layout: SVG centered above tagline**
Statuette sits above the page's main tagline. Subtle entrance animation (fade + slight float up) on load — CSS only, no JS needed.

**Color: gold/amber accent (`#F59E0B`) for Oscar identity**
Consistent with the amber already used in the icon unlock celebration. Gold = Oscars. Applies to the SVG fill and the acronym initial letters.

## Risks / Trade-offs

- [SVG not yet received] → Design is specced around a single-path or multi-path statuette; actual SVG complexity may require adjustments to animation approach
- [Acronym readability] → "Supabase" in the acronym may need a note clarifying it refers to the data platform — consider a subtle tooltip or footnote

## Open Questions

- Final SVG asset: pending upload from user
- Acronym layout: poem vs. two-column — decide visually once SVG proportions are known
- Should the Oscar name appear as a logotype (`<h1>`) or is the SVG sufficient as the brand mark?

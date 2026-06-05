# Strategic Plan Overrides

Strategic-plan-specific shadow-DOM injections, class overrides, and custom component implementations that aren't general enough to live in `page-builder/OVERRIDES.md` or `page-builder/styles/critical.css`.

## Strategic Commitments Venn diagram

Custom full-bleed section on `pages/index.html`. Four overlapping circles in brand colors (UMD red, gold, black, grey) with label text in each segment and "FEARLESSLY FORWARD" centered at the intersection. Implemented as a custom HTML/CSS block — not a design-system component and not a candidate for the DS upstream.

Pages using this: `pages/index.html`.

## Pathway sticky (first pathway)

The first `umd-element-pathway` on `pages/index.html` is sticky (scrolls with the viewport until it reaches its scroll boundary). Standard pathway component with `position: sticky` applied via a wrapper or shadow injection as needed.

Pages using this: `pages/index.html`.

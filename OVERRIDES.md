# Strategic Plan Overrides

## Navigation: hamburger drawer requires `slot="primary-slide-links"`

`umd-element-navigation-header` only renders the mobile hamburger button when `slot="primary-slide-links"` is present. Without it, `drawer.CreateElement()` returns null and the header collapses to logo-only on mobile with no hamburger. Always include this slot mirroring the main-navigation links (each link text wrapped in `<span>`). For two-level mobile nav (dropdowns), pair with `slot="children-slides"` using matching `data-child-ref` / `data-parent-ref` attributes.

Pages using this: all pages in this project.

## Hero: use standard background hero — never `data-display="overlay"`

For landing page heroes in this project, use the standard/background hero: `data-theme="dark"` with **no** `data-display` attribute. The overlay variant (`data-display="overlay"`) composites a color layer over the image and is a different visual treatment. Omitting `data-display` gives the correct standard full-bleed image hero.

```html
<!-- ✓ Correct -->
<umd-element-hero data-theme="dark">

<!-- ✗ Wrong — overlay is a distinct variant, not the standard background hero -->
<umd-element-hero data-display="overlay" data-theme="dark">
```

Pages using this: all landing pages in this project.

## Pathway: always sticky, on `umd-layout-background-full-dark` wrapper

All `umd-element-pathway` components in this project use `data-display="sticky"` with `data-theme="dark"`. Use `class="umd-layout-background-full-dark"` on the section wrapper — this provides the black background AND the correct vertical padding (48px mobile → 80px tablet → 104px desktop). Do NOT use `umd-layout-vertical-landing` + inline `style="background:#000"` for dark pathway sections, as that omits the padding steps.

```html
<section class="umd-layout-background-full-dark">
  <umd-element-pathway data-display="sticky" data-theme="dark">
    ...
  </umd-element-pathway>
</section>
```

Pages using this: `pages/index.html`.

Strategic-plan-specific shadow-DOM injections, class overrides, and custom component implementations that aren't general enough to live in `page-builder/OVERRIDES.md` or `page-builder/styles/critical.css`.

## Strategic Commitments Venn diagram

Custom full-bleed section on `pages/index.html`. Four overlapping circles in brand colors (UMD red, gold, black, grey) with label text in each segment and "FEARLESSLY FORWARD" centered at the intersection. Implemented as a custom HTML/CSS block — not a design-system component and not a candidate for the DS upstream.

Pages using this: `pages/index.html`.

## Pathway sticky (first pathway)

The first `umd-element-pathway` on `pages/index.html` is sticky (scrolls with the viewport until it reaches its scroll boundary). Standard pathway component with `position: sticky` applied via a wrapper or shadow injection as needed.

Pages using this: `pages/index.html`.

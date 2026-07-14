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

## Pathway light panels — use the overlay variant, not a standard-pathway injection

For light pathways with the grey offset panel (strategic-plan commitment pages), use `umd-element-pathway data-display="overlay" data-theme="light"` with an explicit `data-layout-image-position` (`left` or `right`). The overlay variant is self-contained (paints its own light-gray panel — see page-builder RULES.md §5) and ships the scroll-triggered panel entrance animation (`animation-timeline: view()`).

**`data-animation` gotcha:** animation is ON by default when the attribute is absent. A bare `data-animation` (empty value) reads as *not "true"* and **disables** it. Omit the attribute entirely (or use `data-animation="false"` to opt out, as with `umd-element-quote` on index.html).

Do **not** shadow-inject a panel onto the standard (no `data-display`) pathway — `data-theme="light"` is a no-op there in cdn.js v1.18.12 and an injection was previously needed; the overlay variant replaced it.

**Pages using this:** `pages/commitment/we-reimagine-learning.html` (all five initiative pathways).

## Modal (`umd-element-modal`) — actual v1.18.12 contract differs from registry

`registry/registry-layout.json` documents the default slot and `data-visual-open`/`data-visual-closed` observed attributes. **Neither is implemented in cdn.js v1.18.12.** The real contract:

- Content must be a child with **`slot="content"`** — the component renders `<slot name="content">` inside its backdrop; unslotted children never display.
- Show/hide is driven by **`data-layout-hidden`**: the `true → false` transition opens, `false → true` closes. Start with `data-layout-hidden="true"`.
- The component provides the fixed backdrop (`rgba(0,0,0,0.9)`), backdrop-click close, focus trap, and body scroll lock. It resets `data-layout-hidden="true"` when it closes itself.
- The slotted panel is unstyled light DOM — the page provides the white box, red top rule, close button, title/eyebrow styles (`.sp-modal-panel`, `.sp-modal-title`, `.sp-modal-eyebrow`, `.sp-modal-close` on `pages/commitment/we-reimagine-learning.html`).

**Trigger gotcha:** `umd-element-call-to-action` and `umd-element-card-overlay` (cta-icon slot) **clone** their child link/button into shadow DOM, so document-level click delegation via `e.target.closest(...)` never sees the trigger — walk `e.composedPath()` instead (see the modal-wiring script on the page).

**Pages using this:** `pages/commitment/we-reimagine-learning.html` (5 initiative Details modals + 3 goal Objectives modals).

## Pathway sticky (first pathway)

The first `umd-element-pathway` on `pages/index.html` is sticky (scrolls with the viewport until it reaches its scroll boundary). Standard pathway component with `position: sticky` applied via a wrapper or shadow injection as needed.

Pages using this: `pages/index.html`.

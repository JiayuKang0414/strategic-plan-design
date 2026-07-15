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

## Impact page custom components (`pages/impact.html`)

Two light-DOM custom tags (NOT design-system web components — never registered, no shadow DOM). Both are styled by the page's own `<style>` block and driven by inline scripts at the end of `<body>`. Mechanics mirror the live strategicplan.umd.edu/impact page; visuals follow the Figma comp.

Why custom (per the CLAUDE.md registry-first rule): no DS carousel supports category filtering or these card visuals (`carousel-cards` is a dark-texture band of `umd-element-card` children with no filter UI), and `umd-element-stat` is a number/label lockup with no table or infographic rail.

### `umd-stat-chart` — metric table + animated infographic rail

- Structure: `.sp-chart-label` (bold 22px title) → `.sp-chart-body` (flex) → `.sp-chart-table[data-cols="2|4"]` + `.sp-chart-info[data-chart="up-arrow|bar|donut"]` → optional `.sp-chart-footnote` (red left rule).
- The table is a CSS grid of `.sp-chart-col` wrappers (black header cell + red Barlow Condensed Bold Italic value). Column wrappers keep header/value pairs together at every viewport: 1 col < 480px, 2 cols ≥ 480px, and `data-cols="4"` gets 4 columns at ≥ 1024px. Header cells use `flex: 1` so the black bands stay equal-height when one label wraps.
- **Supports up to 4 columns** (`data-cols="4"` — used by the expenditures chart per the comp).
- Glyphs for >100% or share stats are editorial choices per the comp: `up-arrow` (default), `bar` (mini gold+red bar chart, used for 368%), `donut` (gold SVG ring, used for 99%; arc length set via `--sp-donut-offset` inline on the SVG).
- **Scroll animations** (same contract as the live site): an IntersectionObserver at `threshold: 1` adds `.is-active` once per rail and unobserves (the live site re-fires and errors on the donut — don't copy). CSS transitions do the motion: arrow slides up 30px / 0.5s ease-in, bars grow via `scaleY` staggered, donut arc sweeps via `stroke-dashoffset` over 1s. All gated behind `prefers-reduced-motion: no-preference`; the live site's Chart.js doughnut is replaced by a dependency-free SVG ring.

### `umd-timeline` — commitment-filtered card carousel

- Filter pills are checkboxes (`.sp-timeline-filters div[data-color]`), one per commitment; checked pills show a ✕. Selection is a **union**; empty selection = all cards. `data-color` values: `red` = Reimagine Learning, `gold` = Grand Challenges, `lightGray` = Invest in People, `darkGray` = Partner/Public Good.
- Applying a filter fades the track out (0.5s), rebuilds it from the original card list (document order preserved), resets `left: 0`, fades back in. "Clear all" unchecks everything. Window resize re-applies the filter (debounced 200ms).
- The carousel is NOT scroll-snap: a flex track slid one card per click by setting inline `left` (card offsetWidth + gap, 0.5s ease-in-out CSS transition), 500ms debounce, prev/next auto-hide at the ends (`hidden` attribute), touch swipe ≥ 90px pages one card.
- Cards (`umd-timeline-card[data-color]`): 4px right+bottom border and overlapping date chip in the category color, vertical ghost year (`writing-mode: vertical-rl`, Interstate 900 44px #E6E6E6) over the image's right rail, title/deck/arrow link.

### Section intro works text-only (registry correction)

`registry-content.json` marks the section-intro `headline` slot as required, but `umd-element-section-intro` renders correctly with only `slot="text"` + `slot="actions"` — the impact page's centered intro (red separator, bold centered paragraph, CTA) uses exactly that. Don't hand-roll this pattern.

## Watermark utility — ghost word needs its own wrapper

`.umd-watermark > *` (shared style block §2) styles **every direct child** as an absolute 240px ghost word. Never put the class on a content wrapper — wrap only the ghost word and let it overlay siblings:

```html
<!-- ✓ Correct (impact.html News section) -->
<div class="umd-layout-space-horizontal-larger">
  <div class="umd-watermark" aria-hidden="true"><span>Strategic</span></div>
  <h2 ...>News</h2>
  ...
</div>

<!-- ✗ Wrong — heading and grid also become giant watermark text and break paint -->
<div class="umd-layout-space-horizontal-larger umd-watermark">
  <p>Strategic</p>
  <h2 ...>News</h2>
</div>
```

Pages using this: `pages/impact.html`.

## Pathway sticky (first pathway)

The first `umd-element-pathway` on `pages/index.html` is sticky (scrolls with the viewport until it reaches its scroll boundary). Standard pathway component with `position: sticky` applied via a wrapper or shadow injection as needed.

Pages using this: `pages/index.html`.

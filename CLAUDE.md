# Claude Code — Strategic Plan Design

This is the **Strategic Plan design project**. It builds on the design-system page builder (vendored as a submodule at `page-builder/`).

## Where to find things

| What | Location |
|---|---|
| Slash commands | `page-builder/.claude/commands/*.md` |
| Layout/spacing/component rules | `page-builder/RULES.md` |
| Component slots & attributes | `page-builder/registry/` |
| Critical CSS (canonical) | `page-builder/styles/critical.css` |
| Skeleton + inlined CSS | `page-builder/TEMPLATE.html` |
| Layout HTML patterns | `page-builder/LAYOUT-PATTERNS.md` |
| Generic page-builder overrides | `page-builder/OVERRIDES.md` |
| **Strategic-plan-specific overrides** | `OVERRIDES.md` (this repo) |

The page-builder's own `CLAUDE.md` (`page-builder/CLAUDE.md`) defines the canonical rules — read it. This file layers strategic-plan-specific guidance on top.

## Output paths

- New pages → `pages/<page-name>.html`
- New images → `images/strategic-plan/` or a new `images/<page>/` folder per page
- Briefs / source notes → `briefs/<page-name>.md`

Do **not** write to `examples/` or `test/` — those exist in the page-builder repo, not here.

## Image paths

- **Strategic-plan-owned** (logos, page-specific photography): `../images/...`
- **Shared library** (large/small/medium campus, people, events, default): `../page-builder/images/large/...` etc.

When `images-index.json` is needed, read `page-builder/images/images-index.json`.

## Shared chrome and reference pages

Every page within a single design project must use the **same site header, navigation, logo, and footer**. Pages in this project should look like a coherent site — they should not invent their own chrome, nav items, or logo treatment.

### Reference page for this project

For strategic-plan-design, the canonical reference is **`pages/index.html`** — it is the established landing-page design for this project. When building a new page in this repo:

1. Open `pages/index.html` and copy verbatim:
   - The full header stack (`umd-element-navigation-utility` + `umd-element-utility-header` + `umd-element-navigation-header` with the project nav items)
   - The footer block (`umd-element-footer data-display="visual"`)
   - End-of-body shadow-injection scripts
2. Use the same logo paths, the same nav item set, and the same footer image — do not substitute or reorder.
3. Only the `<main>` content between the header and footer is page-specific.

### Reference page is not yet built

`pages/index.html` does not exist yet. When building it:
- Establish the header/nav/logo/footer intentionally using the nav items and logos listed below
- That first page becomes the authoritative reference for all subsequent pages in this project

### Reference pages are project-scoped

Reference pages live in this repo only — never copy this project's chrome into the `page-builder/` submodule.

## Navigation

The site nav items (in order):

| Label | Notes |
|---|---|
| Home | Links to `index.html` |
| Guiding Principles | |
| Strategic Commitments | Has a dropdown |
| Implementation | |
| Metrics | |

## Logos

| Slot | Path |
|---|---|
| Header (`umd-element-navigation-header` `slot="logo"`) | `../images/logos/strategic-plan-logo.svg` |
| Footer (`umd-element-footer` `slot="logo"`) | `../images/logos/footer-logo.svg` |
| Fallback (header onerror) | `../images/logos/primary-logo-dark.svg` |

Always include the `onerror` runtime fallback for hotlink-protected URLs (see page-builder/CLAUDE.md).

## Custom components (not in the design system)

### Strategic Commitments Venn diagram

The home page includes a custom Venn diagram showing four overlapping circles ("We Reimagine Learning", "We Take On Humanity's Grand Challenges", "We Partner to Advance the Public Good", "We Invest in People and Communities") with "FEARLESSLY FORWARD" at the center. This visual does not map to any existing design-system component and should be implemented as a self-contained custom HTML/CSS/SVG block. It does **not** need to exist upstream in the page-builder design system.

See `OVERRIDES.md` for implementation notes.

## Source of truth hierarchy (strategic plan)

1. Slash commands in `page-builder/.claude/commands/*.md`
2. `page-builder/RULES.md`
3. `page-builder/registry/`
4. `page-builder/styles/critical.css`
5. `OVERRIDES.md` (this repo) — project-specific shadow injections, class overrides, and custom components
6. `page-builder/OVERRIDES.md` — generic shadow injections (visit-card, banner-promo, etc.)

# Handoff — Strategic Plan Design

_Last updated: 2026-07-27_

This note captures where the project stands so work can be picked up from a fresh
clone (e.g. from a different Claude account). Everything needed to continue lives
in this repo and its `page-builder/` submodule — there is no external/local state
required.

## Getting set up

```bash
git clone git@github.com:zaida-umd/strategic-plan-design.git
cd strategic-plan-design
git submodule update --init --recursive   # page-builder is a submodule
```

Read `CLAUDE.md` first (project rules), then `OVERRIDES.md` (project-specific
overrides and custom components), then `page-builder/CLAUDE.md` (canonical
design-system rules).

## Where things stand

**Built pages** (in `pages/`):
- `index.html` — landing page; the **canonical reference** for header/nav/logo/footer. Copy its chrome verbatim into new pages.
- `principles.html` — Guiding Principles
- `impact.html` — Impact (nav item formerly "Metrics")
- `implementation.html` — Implementation
- `commitment/we-reimagine-learning.html` — first of four Strategic Commitment pages

**Briefs** (in `briefs/`): `we-reimagine-learning.md`, `implementation.md`

**Deploy**: GitHub Pages via a submodule-aware GitHub Actions workflow (`.github/`).

## What's next (in-flight)

`pages/index.html` links to several pages that are **not built yet**:

- [ ] `pages/strategic-commitments.html` — the Strategic Commitments landing/overview page
- [ ] `pages/commitment/we-take-on-humanitys-grand-challenges.html`
- [ ] `pages/commitment/we-partner-to-advance-the-public-good.html`
- [ ] `pages/commitment/we-invest-in-people-and-communities.html`

Only `we-reimagine-learning.html` of the four commitment pages exists. Use it as
the structural template for the other three. No briefs exist yet for these three
commitments — write `briefs/<page-name>.md` first if source notes are needed.

## Conventions reminder (see CLAUDE.md for detail)

- Every page reuses the **same header/nav/logo/footer** — copy verbatim from `pages/index.html`.
- Nav order: Home · Guiding Principles · Strategic Commitments (dropdown) · Implementation · Metrics.
- **Registry first**: before hand-rolling any element, search `page-builder/registry/` for an existing `umd-element-*` component.
- New pages → `pages/`, new images → `images/strategic-plan/` or a per-page `images/<page>/` folder, briefs → `briefs/`.
- Never write to the `page-builder/` submodule to add project chrome; project-specific overrides go in this repo's `OVERRIDES.md`.

## Repo state at handoff

Working tree clean, `main` up to date with `origin/main`, submodule pinned to a
committed ref. Nothing uncommitted or stashed.

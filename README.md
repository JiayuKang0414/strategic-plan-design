# Strategic Plan Design

Design work for the UMD Strategic Plan site — multiple pages sharing a common header, footer, and design-system chrome.

## Layout

```
strategic-plan-design/
├── pages/              Page HTML (one file per page)
├── shared/             Header, footer, head, and end-scripts partials (future)
├── images/
│   ├── logos/          Strategic Plan and UMD logos
│   └── strategic-plan/ Page-specific photography and assets
├── briefs/             Page briefs / source notes
├── scripts/            Build scripts (HTML partial inlining, etc.)
├── page-builder/       Submodule → design-system-page-builder
│                       Source for critical.css, registry, RULES.md,
│                       slash commands (.claude/commands/), and shared
│                       /images/large /images/small assets.
└── OVERRIDES.md        Project-specific shadow-DOM injections, CSS overrides, and custom components
```

## Image paths

- **Project-specific**: `../images/logos/`, `../images/strategic-plan/`
- **Shared library** (campus, people, events, etc.): `../page-builder/images/large/...`, `../page-builder/images/small/...`

## Working with this repo

The page-builder submodule provides slash commands (`.claude/commands/*.md`) and the canonical `critical.css`. Run Claude Code from the root of this repo; it will read `CLAUDE.md` here and follow the project rules.

To update the page-builder pin:

```bash
cd page-builder && git pull origin main && cd ..
git add page-builder && git commit -m "Bump page-builder submodule"
```

## Pages

- `pages/index.html` — Strategic Plan home page (reference page for this project)

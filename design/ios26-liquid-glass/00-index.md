# iOS 26 Liquid Glass Design System

- **Generated output directory:** `design/ios26-liquid-glass/`
- **Source inputs used:** User description ("iOS26液态玻璃风格" / "iOS26 liquid glass style")
- **Confirmed visual decisions:**
  - Heavy reliance on translucency, backdrop blurring, and glassmorphism.
  - Dark mode by default to emphasize vibrant liquid-like colorful glows beneath glass surfaces.
  - Generous border radii (highly rounded corners) typical of modern Apple interfaces.
  - Borders have a subtle semi-transparent white stroke to simulate the edge of glass.
  - System typography with optimized tracking for readability across layers.
- **Assumptions and open points:**
  - Assumed a dark-themed base because liquid glass lighting and colorful blur effects are most prominent on dark backgrounds.
  - Assumed standard iOS-like breakpoint behavior (mobile-first, scaling to tablet and desktop).

## File Map

- **`00-index.md`**: This file. Overview of the design system and file structure.
- **`DESIGN.md`**: Canonical portable design-system source with machine-readable YAML tokens and human-readable rules.
- **`visual-spec.md`**: Expanded human-readable manual with rationale and detailed examples.
- **`tokens.json`**: Machine-readable JSON mirror of the core design tokens.
- **`preview.html`**: Standalone, interactive HTML preview of the design system components and tokens.
- **`usage.md`**: Guide on how to use and update this system.
- **`handoff/html-generation-guide.md`**: Instructions for downstream AI agents generating HTML/CSS.
- **`handoff/figma-remote-mcp-guide.md`**: Instructions for downstream AI agents using Figma Remote MCP.

## Recommended Read Order

**For Humans:**
1. Open `preview.html` in your browser to feel the liquid glass visual style.
2. Read `visual-spec.md` to understand the philosophy and design decisions.
3. Read `usage.md` to know how to maintain and expand the system.

**For AI Agents:**
1. Parse `tokens.json` or the YAML front matter of `DESIGN.md` to load the exact token values.
2. Read the body of `DESIGN.md` to understand when and how to apply these tokens.
3. Depending on your output target, read `handoff/html-generation-guide.md` or `handoff/figma-remote-mcp-guide.md`.

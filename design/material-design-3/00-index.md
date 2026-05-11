# Material Design 3 Design System

- **Generated output directory:** `design/material-design-3/`
- **Source inputs used:** User description ("Material Design 3" / "MD3")
- **Confirmed visual decisions:**
  - Employs the Material You (M3) tonal palette system with Primary, Secondary, Tertiary, and Error color roles along with their respective containers.
  - Typography is based on the M3 scale (Display, Headline, Title, Body, Label) utilizing the Roboto font stack.
  - Shapes are highly rounded, particularly for buttons (fully rounded/pill-shaped) and large components (28px for cards and dialogs).
  - Elevation is achieved through tonal surface tints combined with subtle shadows, rather than relying exclusively on heavy drop shadows.
  - High emphasis on accessibility, with strict adherence to contrast rules ("on-color" tokens).
- **Assumptions and open points:**
  - Assumed a default Light Theme for the base palette, using a standard Purple/Indigo seed color typical of M3 defaults.
  - Assumed standard Material layout breakpoints (compact, medium, expanded).

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
1. Open `preview.html` in your browser to feel the Material Design 3 visual style.
2. Read `visual-spec.md` to understand the philosophy and design decisions.
3. Read `usage.md` to know how to maintain and expand the system.

**For AI Agents:**
1. Parse `tokens.json` or the YAML front matter of `DESIGN.md` to load the exact token values.
2. Read the body of `DESIGN.md` to understand when and how to apply these tokens.
3. Depending on your output target, read `handoff/html-generation-guide.md` or `handoff/figma-remote-mcp-guide.md`.

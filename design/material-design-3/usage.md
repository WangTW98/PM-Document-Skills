# Usage Guide: Material Design 3

## Understanding the Files
- **`DESIGN.md` is canonical:** This is the primary source of truth. If there is a discrepancy between files, trust `DESIGN.md`.
- **`tokens.json` is for tooling:** Use this file if you are writing a script or an agent that needs to parse color hex codes or radius values programmatically.
- **`preview.html` is for visual inspection:** Open this file in your web browser to see what the M3 design system looks and feels like. It demonstrates the tonal color palettes and pill-shaped buttons.
- **`visual-spec.md` is for human understanding:** Read this to understand the M3 philosophy, accessibility rules, and component specific guidelines.

## Generating Pages (For AI Agents)
When you ask an AI agent to generate a web page or application using this system:
1. Provide the agent with the `DESIGN.md` file.
2. Provide the agent with `handoff/html-generation-guide.md`.
3. Tell the agent: "Generate a dashboard using the Material Design 3 design system. Strictly follow the on-color pairings and use pill-shaped buttons."

## Creating Figma Drafts (For AI Agents with Figma Remote MCP)
If you want to port this system to Figma:
1. Ensure the agent has access to the Figma Remote MCP.
2. Provide the agent with `DESIGN.md` and `handoff/figma-remote-mcp-guide.md`.
3. Ask the agent: "Create a UI library in Figma using the Material Design 3 system. Setup the primary, secondary, and surface variables, and build the Filled Button and Tonal Button components."

## How to Update the System
To safely update tokens (e.g., changing the seed color from Purple to Green):
1. Update `tokens.json` and `DESIGN.md` with the new tonal scale (Primary, On-Primary, Primary Container, etc.). 
2. Ensure you calculate accessible contrast ratios when changing the base colors.
3. Open `preview.html` and update the CSS variables in the `:root` block.
4. Verify visually that the `preview.html` still looks correct and accessible.

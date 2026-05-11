# Usage Guide: iOS26 Liquid Glass

## Understanding the Files
- **`DESIGN.md` is canonical:** This is the primary source of truth. If there is a discrepancy between files, trust `DESIGN.md`.
- **`tokens.json` is for tooling:** Use this file if you are writing a script or an agent that needs to parse color hex codes or exact radius values programmatically.
- **`preview.html` is for visual inspection:** Open this file in your web browser (no server needed) to see what the design system actually feels like. It contains the true css filters and ambient glowing backgrounds that make "liquid glass" work.
- **`visual-spec.md` is for human understanding:** Read this to understand *why* the tokens exist and the general aesthetic rules.

## Generating Pages (For AI Agents)
When you ask an AI agent to generate a web page or application using this system:
1. Provide the agent with the `DESIGN.md` file.
2. Provide the agent with `handoff/html-generation-guide.md`.
3. Tell the agent: "Generate a dashboard using the iOS26 Liquid Glass design system. Follow the tokens and layout rules strictly."

## Creating Figma Drafts (For AI Agents with Figma Remote MCP)
If you want to port this system to Figma:
1. Ensure the agent has access to the Figma Remote MCP.
2. Provide the agent with `DESIGN.md` and `handoff/figma-remote-mcp-guide.md`.
3. Ask the agent: "Create a UI library in Figma using the iOS26 Liquid Glass design system. Create the color variables and a basic Button component."

## How to Update the System
To safely update tokens without breaking consistency:
1. Always update `DESIGN.md` first.
2. Run a script or manually update the corresponding values in `tokens.json`.
3. Open `preview.html` and update the CSS variables in the `<style>` block to reflect the changes.
4. Verify visually that the `preview.html` still looks correct and accessible.

## Creating Additional Design Systems
If you want to create a completely different visual language (e.g., a brutalist flat design), ask the visual-design-spec skill to generate a new system. It will create a new slug (like `design/brutalist-flat/`) alongside this one.

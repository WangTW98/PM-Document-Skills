# Figma Remote MCP Guide: iOS26 Liquid Glass

When instructed to create a Figma draft using the iOS26 Liquid Glass design system via Figma Remote MCP, follow these instructions to accurately translate the aesthetic into Figma nodes and variables.

## Files to Read
1. Parse `../tokens.json` to get the exact hex and rgba values.
2. Read `../DESIGN.md` for context on how components should be constructed.

## Mapping Tokens to Figma Variables
You should create a local variable collection for the system.
- Map `color.background.canvas` to a color variable `#000000`.
- Map `color.background.surface` to `#FFFFFF` with `5%` opacity. 
- Ensure you set up Number variables for spacing (4, 8, 16, 24, 32) and radii (8, 14, 24, 32).

## Constructing the Liquid Glass Effect in Figma
Figma supports backdrop blurs natively. To create the primary "Glass Panel" component:
1. Create a Frame (not a Rectangle, so it can use auto-layout).
2. Set Fill to `#FFFFFF` at `5%` opacity.
3. Add a Stroke: Inside, `1px`, `#FFFFFF` at `10%` opacity.
4. Add an Effect: `Background blur`, set the value to `24`.
5. Add an Effect: `Inner shadow`, Y: `1`, Blur: `1`, Color: `#FFFFFF` at `15%` opacity.
6. Add an Effect: `Drop shadow`, Y: `8`, Blur: `24`, Color: `#000000` at `40%` opacity.
7. Set Corner Radius to `24` or `32`.
8. *Crucial Context:* Place a highly blurred, vibrant circle (e.g., `#0A84FF` with Layer Blur `100`) *behind* the Glass Panel Frame to demonstrate the effect.

## Component Creation Requirements
When building the basic UI kit:
- **Primary Button:** Height 48px, fully rounded (Radius 9999 or half of height). Fill `#FFFFFF` at 15% opacity + Background Blur 24.
- **Text:** Use "SF Pro" or the default system font if unavailable.

## Layout and Frame Setup
- **Mobile Frame:** Create a frame (e.g., iPhone 15 Pro, 393px width). Ensure side margins are 16px.
- **Desktop Frame:** Create a frame (e.g., Desktop 1440px width). Create a centered auto-layout container with a fixed max-width of 1200px and 32px side padding.

## Final Review
Before completing the MCP workflow:
1. Is the root frame background black? (It should be).
2. Do the cards have Background Blur applied? (They must).
3. Are the corner radii generous? (Small radii look like standard web, not iOS).

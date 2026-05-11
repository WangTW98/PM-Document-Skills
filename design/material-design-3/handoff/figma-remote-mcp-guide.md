# Figma Remote MCP Guide: Material Design 3

When instructed to create a Figma draft using the Material Design 3 (M3) design system via Figma Remote MCP, follow these instructions to accurately translate the aesthetic into Figma nodes and variables.

## Files to Read
1. Parse `../tokens.json` to get the exact hex values and radius sizes.
2. Read `../DESIGN.md` for context on how components should be constructed.

## Mapping Tokens to Figma Variables
You should create a local variable collection for the system.
- Create color variables for the tonal palette. Crucially, group them by role:
  - `Primary`, `On-Primary`, `Primary Container`, `On-Primary Container`
  - `Surface`, `On-Surface`, `Surface Variant`, `On-Surface Variant`
- Ensure you set up Number variables for spacing (4, 8, 12, 16, 24, 32) and radii (4, 8, 12, 16, 28, 9999).

## Component Creation Requirements
When building the basic UI kit:
- **Filled Button:** Height 40px, fully rounded (Radius 9999). Fill `Primary`. Text uses Roboto, 14px Medium, `On-Primary` fill.
- **Tonal Button:** Height 40px, fully rounded. Fill `Secondary Container`. Text uses `On-Secondary Container` fill.
- **Elevated Card:** Radius 12px. Fill `Surface`. Add the Level 1 drop shadow. Note: To simulate M3 tonal elevation in Figma, you can add an inner shadow or a second fill layer of `Primary` set to 5% opacity.
- **Floating Action Button (FAB):** Width/Height 56px. Radius 16px. Fill `Primary Container`. Icon fill `On-Primary Container`.

## Layout and Frame Setup
- **Mobile Frame (Compact):** Create a frame (e.g., Android Large, 360px width). Ensure side margins are 16px. Use a Bottom Navigation bar component.
- **Desktop Frame (Expanded):** Create a frame (e.g., Desktop 1440px width). Ensure side margins are 24px. Use a Navigation Drawer (sidebar) component on the left.

## Final Review
Before completing the MCP workflow:
1. Are buttons fully rounded? (M2 used 4px radii, M3 uses fully rounded pills).
2. Are you using the strict `On-Color` variables for all text and icons? (E.g., white text on primary).
3. Is the typography set to Roboto (or system default) with the correct M3 scale?

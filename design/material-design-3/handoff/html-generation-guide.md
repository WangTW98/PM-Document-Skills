# HTML Generation Guide: Material Design 3

When instructed to generate HTML/CSS from the Material Design 3 (M3) design system, follow these instructions strictly to achieve the correct aesthetic.

## Files to Read
1. Parse `../tokens.json` to get exact color, spacing, radius, and typography values.
2. If `tokens.json` is unavailable, parse the YAML front matter in `../DESIGN.md`.

## Color and Accessibility (Crucial)
M3 relies on strict background/foreground color pairings.
- If you use `color.primary.base` for a background, you MUST use `color.primary.on` for the text or SVG fill inside it.
- If you use `color.surface.base`, use `color.surface.on` for text.
- Never place `onPrimaryContainer` text directly on `primary` backgrounds. Follow the pairings exactly.

## Component Defaults
- **Filled Button:** `background: var(--color-primary-base); color: var(--color-primary-on); border-radius: 9999px; min-height: 40px; border: none; padding: 0 24px; font-weight: 500;`
- **Cards (Elevated):** `background: var(--color-surface-base); border-radius: 12px; box-shadow: <shadow.level1>;` (Plus add the linear gradient tint if supporting M3 tonal elevation).
- **Typography:** `font-family: Roboto, system-ui, sans-serif;` Use `16px` for body text and `14px` medium for buttons.

## Layout and Spacing
- Use `8px` (`space.2`) between related items in a row (e.g., chips, buttons).
- Use `16px` (`space.4`) or `24px` (`space.5`) padding inside cards.

## Responsive Rules
- **< 600px (Compact/Mobile):** 
  - Render a single-column layout. 
  - Use `16px` margin on the left and right of the main container.
  - Primary navigation should be a Bottom Navigation bar.
- **600px - 839px (Medium/Tablet):** 
  - Use `24px` margins.
  - Switch primary navigation to a Navigation Rail (a thin vertical bar on the left).
- **>= 840px (Expanded/Desktop):** 
  - Max container width of `1440px`, centered.
  - Switch primary navigation to a Navigation Drawer (a standard sidebar).

## Accessibility Checks
- Touch targets must be `48px` minimum. If a button is visually `40px` tall, ensure it has `4px` top/bottom margin, or ensure the interactive `<a>` or `<button>` tag actually computes to 48px height even if the visual shape is smaller.

## Drift Check
Before finalizing your generation, review your code:
- Did you use a standard 4px border-radius for buttons? If so, FIX IT. M3 buttons are fully rounded (pill shape).
- Did you use pure black `#000` text? If so, FIX IT. Use the defined `on` colors like `color.surface.on` (which is typically a dark gray like `#1C1B1F`).

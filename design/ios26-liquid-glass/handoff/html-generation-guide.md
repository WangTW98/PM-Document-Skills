# HTML Generation Guide: iOS26 Liquid Glass

When instructed to generate HTML/CSS from the iOS26 Liquid Glass design system, follow these instructions strictly to achieve the correct aesthetic.

## Files to Read
1. Parse `../tokens.json` to get exact color, spacing, radius, and typography values.
2. If `tokens.json` is unavailable, parse the YAML front matter in `../DESIGN.md`.

## The Liquid Glass Formula
The aesthetic depends entirely on layering. Flat elements will look incorrect. You must build your HTML with this structure:

1. **The Void:** `body` background MUST be `#000000` (or `color.background.canvas`).
2. **The Ambient Light:** You must create absolute-positioned `div` elements behind the main content container. Give these divs large dimensions (e.g., `400px` width/height), background colors from the `color.accent` palette (Blue, Purple, Pink), and apply massive blurs (e.g., `filter: blur(100px)`). Animate them slightly for the "liquid" feel.
3. **The Glass Panels:** Wrap your main content in cards or panels. Apply the following CSS exactly:
   ```css
   background: rgba(255, 255, 255, 0.05); /* color.background.surface */
   backdrop-filter: blur(24px) saturate(150%); /* component.glassEffect */
   -webkit-backdrop-filter: blur(24px) saturate(150%);
   border: 1px solid rgba(255, 255, 255, 0.1); /* color.border.default */
   border-radius: 24px; /* radius.lg */
   box-shadow: inset 0 1px 1px rgba(255, 255, 255, 0.15), 0 8px 24px rgba(0, 0, 0, 0.4); /* shadow.glass */
   ```

## Component Defaults
- **Buttons (Primary):** `background: rgba(255, 255, 255, 0.15); backdrop-filter: blur(24px); border-radius: 9999px; height: 48px; border: 1px solid rgba(255, 255, 255, 0.25); box-shadow: 0 0 24px rgba(10, 132, 255, 0.3); color: #FFF;`
- **Typography:** `font-family: system-ui, -apple-system, BlinkMacSystemFont, sans-serif;` Color text pure white `#FFFFFF` or translucent white `rgba(255, 255, 255, 0.6)`. Do not use black text on the glass.

## Responsive Rules
- **< 768px (Mobile):** Render a single-column layout. Set container width to 100% minus `16px` padding on each side.
- **768px - 1023px (Tablet):** Use 2-column grids.
- **>= 1024px (Desktop):** Use a maximum container width of `1200px` centered on screen with `32px` padding on each side. 3 or 4 column grids.

## Accessibility Checks
- Always ensure text sizes are at least 16px for body content.
- Verify focus rings are explicitly drawn (e.g., `outline: 3px solid rgba(10, 132, 255, 0.5)`).

## Drift Check
Before finalizing your generation, review your code:
- Did you use opaque `#333` or `#222` colors for cards? If so, FIX IT. Cards MUST be translucent `rgba` with `backdrop-filter`.
- Did you use 4px or 8px border-radius for cards? If so, FIX IT. Cards MUST use 24px or 32px radii.

---
design_system:
  name: "iOS26 Liquid Glass"
  version: "1.0.0"
  intent: "A hyper-modern, fluid, and immersive interface utilizing deep translucency, variable blurs, and colorful underlying glows."
  tokens:
    color:
      background:
        canvas: "#000000"
        surface: "rgba(255, 255, 255, 0.05)"
        surfaceMuted: "rgba(255, 255, 255, 0.02)"
        surfaceOverlay: "rgba(255, 255, 255, 0.12)"
      text:
        default: "#FFFFFF"
        muted: "rgba(255, 255, 255, 0.6)"
        disabled: "rgba(255, 255, 255, 0.3)"
      action:
        primary:
          background: "rgba(255, 255, 255, 0.15)"
          text: "#FFFFFF"
          glow: "rgba(10, 132, 255, 0.4)"
      accent:
        blue: "#0A84FF"
        purple: "#BF5AF2"
        pink: "#FF375F"
        cyan: "#32ADE6"
      status:
        success: "#30D158"
        warning: "#FF9F0A"
        error: "#FF453A"
      border:
        default: "rgba(255, 255, 255, 0.1)"
        highlight: "rgba(255, 255, 255, 0.25)"
    typography:
      family:
        sans: "system-ui, -apple-system, BlinkMacSystemFont, 'SF Pro Text', 'Segoe UI', Roboto, sans-serif"
        mono: "ui-monospace, 'SF Mono', Menlo, Consolas, monospace"
      size:
        xs: "12px"
        sm: "14px"
        base: "16px"
        lg: "20px"
        xl: "28px"
        xxl: "40px"
      weight:
        regular: "400"
        medium: "500"
        semibold: "600"
        bold: "700"
    space:
      1: "4px"
      2: "8px"
      3: "12px"
      4: "16px"
      5: "24px"
      6: "32px"
      7: "48px"
      8: "64px"
    radius:
      sm: "8px"
      md: "14px"
      lg: "24px"
      xl: "32px"
      full: "9999px"
    shadow:
      glowPrimary: "0 0 24px rgba(10, 132, 255, 0.3)"
      glowAccent: "0 8px 32px rgba(191, 90, 242, 0.2)"
      glass: "inset 0 1px 1px rgba(255, 255, 255, 0.15), 0 8px 24px rgba(0, 0, 0, 0.4)"
    border:
      thin: "1px"
      focusRing: "3px"
    motion:
      duration:
        fast: "150ms"
        normal: "300ms"
        slow: "500ms"
      easing:
        fluid: "cubic-bezier(0.2, 0.8, 0.2, 1)"
        bounce: "cubic-bezier(0.34, 1.56, 0.64, 1)"
    breakpoint:
      mobile: "0px"
      tablet: "768px"
      desktop: "1024px"
      wide: "1440px"
    container:
      max: "1200px"
      gutterMobile: "16px"
      gutterDesktop: "32px"
    component:
      button:
        minHeight: "48px"
      glassEffect:
        blur: "blur(24px)"
        saturate: "saturate(150%)"
---

# DESIGN.md

## 1. Visual Theme & Atmosphere
The iOS26 Liquid Glass style relies heavily on depth, translucency, and vibrant colors peeking through frosted glass. It uses deep blacks for the true background, allowing translucent components to overlay colorful background shapes (aurora or gradient meshes). The aesthetic is hyper-modern, feeling fluid and alive.

- **Use when:** Creating immersive applications, modern dashboards, or media-rich interfaces.
- **Do not use when:** High data density is required (like complex spreadsheets), or when users require high-contrast monochrome accessibility.

## 2. Color Palette & Roles
Colors are inherently translucent to allow background blurs to function.
- **Canvas:** `color.background.canvas` (#000000) is the absolute backdrop.
- **Surfaces:** Use `color.background.surface` for cards. Always pair surfaces with `component.glassEffect` (backdrop-filter) to achieve the glass look.
- **Accents:** Use accent colors (Blue, Purple, Pink, Cyan) as underlying blurred circles behind glass cards to give the "liquid" colorful effect.
- **Text:** `color.text.default` is white. `color.text.muted` is 60% opacity white. Never use black text on glass surfaces.

## 3. Typography Rules
Uses Apple's system font stacks to ensure native rendering on devices.
- **Scale:** Sizes scale rapidly. Huge headers (`xl`, `xxl`) use `semibold` or `bold` with slightly tighter tracking.
- **Role:** Body text uses `base` (16px) with `regular` weight. Metadata and labels use `xs` or `sm` with `medium` weight.
- **Alignment:** Center alignment is favored for empty states and hero sections; left alignment for reading data.

## 4. Spacing & Layout
Generous spacing emphasizes the physical nature of the glass.
- Use `space.4` (16px) inside small controls.
- Use `space.6` (32px) or `space.7` (48px) between major sections.
- Elements should "float" rather than pack tightly.

## 5. Component Styling
- **Cards/Panels:** Background `color.background.surface`. Border `color.border.default`. Radius `radius.lg` (24px). Must have `backdrop-filter: blur(24px) saturate(150%)`. Add `shadow.glass`.
- **Buttons (Primary):** Highly rounded (`radius.full` or `radius.md`), background `action.primary.background`, and a subtle `shadow.glowPrimary`.

## 6. Depth, Borders & Elevation
Glass needs edges to exist.
- Always apply a 1px border using `color.border.default` to glass surfaces.
- Provide an inner highlight: `inset 0 1px 1px rgba(255, 255, 255, 0.15)` to simulate light hitting the top rim of the glass.

## 7. Icons, Imagery & Illustration
- Icons should be SF Symbols or similarly rounded, minimalist line icons.
- Icons are drawn with `color.text.default` and often have a subtle drop shadow to lift them off the glass.

## 8. Motion & Interaction
- Interactions should feel like fluid physical objects.
- Hover states slightly increase surface opacity (`color.background.surfaceOverlay`) and expand the blur.
- Clicks use `motion.easing.bounce` to give a springy, tactile response.

## 9. Responsive Behavior
- **Mobile (<768px):** Containers span 100% width with `container.gutterMobile`. Grid collapses to 1 column. Navigation is typically a bottom tab bar floating over the canvas. Touch targets minimum 48px.
- **Tablet (768px - 1023px):** Grids transition to 2 columns. Sidebar navigation appears as a collapsible glass pane.
- **Desktop (1024px+):** Max container width `container.max`. Permanent sidebar or floating top navigation. 3-4 column grids.

## 10. Accessibility Rules
- Glassmorphism can reduce contrast. Ensure the text layer over the glass maintains at least a 4.5:1 contrast ratio against the *resulting* color of the background + blur + surface.
- Focus states must be visible: use `border.focusRing` with `accent.blue` outline.
- If reduced motion is preferred by the OS, remove fluid bounce easing and disable background animation.

## 11. Do's and Don'ts
- **DO** use vibrant blurred blobs behind glass to give the UI life.
- **DO** use generous border radii (24px+ for cards).
- **DON'T** use opaque gray cards; this ruins the glass illusion.
- **DON'T** layer too many glass cards on top of each other (max 2 levels) as the blur becomes computationally heavy and muddy.

## 12. Agent Prompt Guide
**Canonical source:** Read `tokens.json` for exact values.
**Core concept:** Build a dark-mode UI with `background-color: #000000`. Place absolute-positioned blurred colorful divs behind the main content. Wrap main content in glass panels using `rgba(255,255,255,0.05)`, `backdrop-filter: blur(24px)`, and `border: 1px solid rgba(255,255,255,0.1)`.
**Component defaults:** 
- Cards: 24px radius, glass shadow, translucent background.
- Buttons: 48px height, fully rounded, translucent or solid vibrant color.
**Responsive:** Collapse grids to 1 column on mobile. Preserve 48px touch targets.

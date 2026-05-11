---
design_system:
  name: "Material Design 3"
  version: "1.0.0"
  intent: "Expressive, adaptable, and accessible interface based on tonal color palettes, high border radii, and distinct elevation levels."
  tokens:
    color:
      primary:
        base: "#6750A4"
        on: "#FFFFFF"
        container: "#EADDFF"
        onContainer: "#21005D"
      secondary:
        base: "#625B71"
        on: "#FFFFFF"
        container: "#E8DEF8"
        onContainer: "#1D192B"
      tertiary:
        base: "#7D5260"
        on: "#FFFFFF"
        container: "#FFD8E4"
        onContainer: "#31111D"
      error:
        base: "#B3261E"
        on: "#FFFFFF"
        container: "#F9DEDC"
        onContainer: "#410E0B"
      background:
        base: "#FFFBFE"
        on: "#1C1B1F"
      surface:
        base: "#FFFBFE"
        on: "#1C1B1F"
        variant: "#E7E0EC"
        onVariant: "#49454F"
        tint: "#6750A4"
      outline:
        base: "#79747E"
        variant: "#CAC4D0"
    typography:
      family:
        sans: "Roboto, system-ui, -apple-system, sans-serif"
      size:
        display: "57px"
        headline: "32px"
        title: "22px"
        body: "16px"
        label: "14px"
      weight:
        regular: "400"
        medium: "500"
    space:
      1: "4px"
      2: "8px"
      3: "12px"
      4: "16px"
      5: "24px"
      6: "32px"
      7: "48px"
    radius:
      none: "0px"
      xs: "4px"
      sm: "8px"
      md: "12px"
      lg: "16px"
      xl: "28px"
      full: "9999px"
    shadow:
      level1: "0px 1px 2px 0px rgba(0, 0, 0, 0.3), 0px 1px 3px 1px rgba(0, 0, 0, 0.15)"
      level2: "0px 1px 2px 0px rgba(0, 0, 0, 0.3), 0px 2px 6px 2px rgba(0, 0, 0, 0.15)"
      level3: "0px 4px 8px 3px rgba(0, 0, 0, 0.15), 0px 1px 3px 0px rgba(0, 0, 0, 0.3)"
    elevation:
      level1: "linear-gradient(0deg, rgba(103, 80, 164, 0.05), rgba(103, 80, 164, 0.05)), #FFFBFE"
      level2: "linear-gradient(0deg, rgba(103, 80, 164, 0.08), rgba(103, 80, 164, 0.08)), #FFFBFE"
      level3: "linear-gradient(0deg, rgba(103, 80, 164, 0.11), rgba(103, 80, 164, 0.11)), #FFFBFE"
    motion:
      duration:
        short: "200ms"
        medium: "400ms"
      easing:
        emphasized: "cubic-bezier(0.2, 0.0, 0, 1.0)"
        standard: "cubic-bezier(0.2, 0.0, 0, 1.0)"
    breakpoint:
      compact: "0px"
      medium: "600px"
      expanded: "840px"
      large: "1200px"
    container:
      max: "1440px"
      marginCompact: "16px"
      marginMedium: "24px"
      marginExpanded: "24px"
    component:
      button:
        minHeight: "40px"
      fab:
        size: "56px"
        radius: "16px"
---

# DESIGN.md

## 1. Visual Theme & Atmosphere
Material Design 3 (M3) represents a shift towards expressive, personal, and accessible interfaces. It relies on a tonal palette derived from a single seed color. The atmosphere is clean, grounded in reality (through elevation and shadow), but friendly and accessible due to high border radii and clear typography.

- **Use when:** Building Android apps, Google ecosystem products, or highly accessible, structurally sound web applications.
- **Do not use when:** You need a highly stylized, translucent "glassmorphism" aesthetic or a brutalist style.

## 2. Color Palette & Roles
M3 color works on a tonal scale and strictly couples "background" colors with "on" colors for accessibility.
- **Primary:** High-emphasis actions (`color.primary.base`). Text/icons on it use `color.primary.on`.
- **Primary Container:** Lower-emphasis primary actions (`color.primary.container`). Text/icons use `color.primary.onContainer`.
- **Secondary/Tertiary:** Used for less prominent elements, filters, or complementary accents.
- **Surface:** The background for components (`color.surface.base`). Text uses `color.surface.on`.
- **Surface Variant:** Used for differentiation, like input fields (`color.surface.variant`) with `color.surface.onVariant` text.

## 3. Typography Rules
Uses the Roboto font stack.
- **Display/Headline:** Large, expressive titles.
- **Title/Body:** Reading text. Body uses 16px.
- **Label:** Small text for buttons and metadata, often medium weight.

## 4. Spacing & Layout
M3 uses a responsive layout grid.
- Margins and gutters change based on the window size (compact: 16px, medium/expanded: 24px).
- Internal component spacing typically uses `space.2` (8px) or `space.4` (16px).

## 5. Component Styling
- **Filled Button:** `color.primary.base` background, `radius.full` (pill shape), min-height 40px.
- **Tonal Button:** `color.secondary.container` background, `radius.full`.
- **Cards:** `color.surface.base` or `color.surface.variant` background. Radius `radius.md` (12px) or `radius.lg` (16px).
- **Floating Action Button (FAB):** `color.primary.container` background, 56x56px, `radius.lg` (16px, a squircle/rounded box instead of circle in M3), `shadow.level3`.

## 6. Depth, Borders & Elevation
M3 uses both shadows and tonal elevation (tinting the surface color).
- **Elevation Level 1-3:** Adds a slight tint of the primary color over the surface, plus a shadow. Use for cards, dialogs, and navigation bars.
- **Outline:** Use `color.outline.base` for high-contrast borders (e.g., text fields) and `color.outline.variant` for dividers.

## 7. Icons, Imagery & Illustration
- Icons are rounded, solid or outlined, typically 24x24px.
- Icons should always inherit the appropriate "on" color of their parent container.

## 8. Motion & Interaction
- Emphasized easing (`cubic-bezier(0.2, 0.0, 0, 1.0)`) creates a snappy, responsive feel.
- States (Hover, Focus, Pressed) overlay a semi-transparent layer of the "on" color.

## 9. Responsive Behavior
- **Compact (<600px):** 4 columns. Bottom navigation or navigation drawer. Full-width dialogs.
- **Medium (600px - 839px):** 8 columns. Navigation rail on the left.
- **Expanded (840px+):** 12 columns. Permanent or dismissible standard navigation drawer. Max width content centering.

## 10. Accessibility Rules
- Adhere strictly to the "color/on-color" pairings. Never place `color.primary.onContainer` text on `color.primary.base` background.
- Touch targets must be at least 48x48px, even if the visible component (like a button) is 40px high.

## 11. Do's and Don'ts
- **DO** use fully rounded corners for buttons.
- **DO** use tonal elevation for cards and surfaces to create depth without heavy shadows.
- **DON'T** use pure black or pure white shadows; M3 shadows are often tinted or very soft.
- **DON'T** mix arbitrary colors; stick to the tonal palette roles.

## 12. Agent Prompt Guide
**Canonical source:** Read `tokens.json`.
**Core concept:** Build a clean, accessible UI using Roboto and the M3 tonal palette. Use fully rounded pill buttons, 12-16px rounded cards, and strictly pair background colors with their designated "on" colors for text/icons.
**Component defaults:** 
- Cards: 12px radius, surface or surface variant fill, level 1 shadow/elevation.
- Buttons: 40px height, fully rounded, primary fill with white text.
**Responsive:** Switch from bottom navigation on mobile to a left-side navigation rail on tablets.

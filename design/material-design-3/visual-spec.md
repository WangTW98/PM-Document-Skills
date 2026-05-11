# Visual Spec: Material Design 3

## Design Thesis
Material Design 3 (M3) is the latest evolution of Google's design language. It focuses on personal expression, adaptability, and accessibility. The interface is constructed using a robust tonal color palette, distinct elevation layers (both shadow and color tinting), and highly legible typography.

## Brand Personality
- **Mood:** Clean, organized, friendly, accessible, and pragmatic.
- **Audience:** Broad, mainstream users who expect standard, predictable, and highly usable interfaces.
- **Fit:** Productivity tools, enterprise software, Android native apps, and cross-platform web applications.

## Color System
M3 colors are generated from a single seed color, producing tonal palettes. This specification uses a default Purple/Indigo seed.
- **Strict Pairings:** Every background color has a corresponding `on` color to guarantee accessibility.
  - Primary (`#6750A4`) must use On-Primary (`#FFFFFF`) text.
  - Primary Container (`#EADDFF`) must use On-Primary Container (`#21005D`) text.
- **Surface Roles:** 
  - `surface.base` (`#FFFBFE`) for standard backgrounds.
  - `surface.variant` (`#E7E0EC`) for distinct, slightly darker areas like filled text fields or secondary cards.

## Typography
- **Font:** Roboto (or system-sans fallback).
- **Scale:**
  - **Display:** Large, expressive (57px). Used sparingly.
  - **Headline:** Standard large titles (32px).
  - **Title:** Medium titles for app bars or card headers (22px).
  - **Body:** Standard reading text (16px).
  - **Label:** Small, medium-weight text for buttons and metadata (14px).

## Layout & Density
- **Density:** Medium. M3 provides comfortable spacing.
- **Grid:** Responsive column grids (4 columns on mobile, 8 on tablet, 12 on desktop). Margins scale from 16px to 24px.

## Component Specs

### Cards
- **Background:** `surface.base` or `surface.variant`.
- **Border:** Optional 1px solid `outline.variant` (`#CAC4D0`).
- **Radius:** 12px (`radius.md`) or 16px (`radius.lg`).
- **Elevation:** Level 1 tonal elevation + shadow for interactive cards.

### Buttons
- **Filled Button:** `primary.base` background, fully rounded (`radius.full`), 40px height. Text is `label` size, `primary.on` color.
- **Tonal Button:** `secondary.container` background, fully rounded, 40px height. Text is `label` size, `secondary.onContainer` color.
- **Text Button:** Transparent background, `primary.base` text color.

### Floating Action Button (FAB)
- **Background:** `primary.container`
- **Shape:** 56x56px square with 16px radius (Squircle).
- **Elevation:** Level 3 shadow.

## Responsive Adaptation
- **Mobile (Compact):** Navigation moves to a Bottom Navigation Bar. Cards span full width.
- **Tablet (Medium):** Navigation moves to a Navigation Rail on the left edge.
- **Desktop (Expanded):** Navigation moves to a permanent Navigation Drawer (Sidebar). Content is centered with a max width (e.g., 1440px).

## Accessibility Requirements
- High contrast is built into the M3 "on-color" system. Do not violate these pairings.
- Touch targets must be at least 48x48px. If a button is visually 40px high, its interactive area must extend 4px above and below.
- Interactive states (hover, focus, pressed) are achieved by overlaying the `on` color at specific opacities (e.g., 8% for hover, 12% for focus).

## Dos and Don'ts
- **DO** use pill-shaped (fully rounded) buttons.
- **DO** use tonal containers (e.g., Primary Container) for lower-emphasis active states instead of gray.
- **DON'T** use pure black `#000000` for shadows or text; M3 uses tinted dark grays (`#1C1B1F`).
- **DON'T** use sharp corners for cards or dialogs. Use 12px, 16px, or 28px radii.

## Assumptions
- Assumed a Light Mode default for this spec. A full implementation would invert the tonal palette for Dark Mode (e.g., Primary becomes a lighter tone like `#D0BCFF` and Surface becomes a dark tone like `#1C1B1F`).

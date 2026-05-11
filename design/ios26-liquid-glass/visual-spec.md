# Visual Spec: iOS26 Liquid Glass

## Design Thesis
The iOS26 Liquid Glass system represents the bleeding edge of modern digital interfaces. It rejects flat, opaque surfaces in favor of deep, layered translucency. The interface should feel like a pane of frosted glass hovering over a vibrant, fluid, and breathing environment. 

## Brand Personality
- **Mood:** Immersive, hyper-modern, tactile, premium, and alive.
- **Audience:** Users expecting state-of-the-art mobile and desktop experiences, akin to spatial computing paradigms.
- **Fit:** Consumer apps, media consumption, creative tools, dashboards emphasizing aesthetic delight.

## Color System
- **Canvas (`#000000`):** True black is the foundation, creating an infinite void where colors shine.
- **Surface (`rgba(255, 255, 255, 0.05)`):** Used for cards and modals. It must always be combined with a strong backdrop blur (e.g., 24px) to simulate glass.
- **Accents:** 
  - Blue (`#0A84FF`), Purple (`#BF5AF2`), Pink (`#FF375F`), Cyan (`#32ADE6`).
  - These are rarely used as flat fills. They should be used as luminous glows behind the glass panels.

## Typography
- Uses system fonts (`system-ui`, San Francisco) to maintain platform familiarity.
- **Scale:**
  - `xxl` (40px) to `xl` (28px) for large, expressive hero titles.
  - `base` (16px) for primary reading, maintaining high legibility against blurred backgrounds.
  - `xs` (12px) for timestamps and metadata.

## Layout & Density
- **Density:** Low to medium. Glassmorphism requires space to breathe; tightly packed glass panels create visual noise and performance issues.
- **Spacing:** Use 24px (`space.5`) to 32px (`space.6`) padding inside major cards to emphasize volume.

## Component Specs

### Cards
- **Background:** `rgba(255, 255, 255, 0.05)`
- **Backdrop Filter:** `blur(24px) saturate(150%)`
- **Border:** 1px solid `rgba(255, 255, 255, 0.1)`
- **Inner Shadow:** `inset 0 1px 1px rgba(255, 255, 255, 0.15)` for the top edge highlight.
- **Radius:** 24px to 32px.

### Buttons
- **Primary:** `rgba(255, 255, 255, 0.15)` background, fully rounded (`radius.full`), glowing shadow `0 0 24px rgba(10, 132, 255, 0.3)`.
- **Secondary:** Transparent background, 1px white border at 10% opacity.
- **Height:** Minimum 48px to maintain an excellent touch target size.

## Responsive Adaptation
- **Mobile:** 1 column. Navigation is typically a floating pill or bottom tab bar to save vertical space.
- **Tablet:** 2 columns. Sidebars become translucent overlapping panes.
- **Desktop:** Expand to 3-4 columns. The max container width is 1200px.

## Accessibility Requirements
- High contrast is tricky with glass. Always test text against the lightest possible blurred background patch.
- Focus states must be explicitly drawn (3px solid blue outline) since native outlines might get lost in blurs.
- Do not animate the background blurs if the user prefers reduced motion.

## Dos and Don'ts
- **DO** use absolute positioning to place vibrant blurred colored circles behind the main content flow.
- **DO** use large corner radii. Small 4px corners break the liquid glass illusion.
- **DON'T** use opaque backgrounds for cards, dropdowns, or modals.
- **DON'T** stack more than two glass elements on top of each other. The blur becomes muddy.

## Assumptions
- Assumed dark mode is the default and preferred rendering mode for this aesthetic, as "liquid glass" relies heavily on bright lights cutting through dark space.

# DESIGN.md Contract

Use this reference when generating `design/<design-system-slug>/DESIGN.md`.

## Purpose

`DESIGN.md` is the canonical portable design-system file. It must work as persistent context for any AI agent that reads project files and must also be readable by humans.

Follow the Google Stitch-style convention: combine machine-readable design tokens in YAML front matter with Markdown rationale and usage rules in the body.

## Required Shape

```markdown
---
design_system:
  name: "..."
  version: "0.1.0"
  intent: "..."
  tokens:
    color: {}
    typography: {}
    space: {}
    radius: {}
    shadow: {}
    border: {}
    motion: {}
    breakpoint: {}
    container: {}
    component: {}
---

# DESIGN.md

## 1. Visual Theme & Atmosphere
## 2. Color Palette & Roles
## 3. Typography Rules
## 4. Spacing & Layout
## 5. Component Styling
## 6. Depth, Borders & Elevation
## 7. Icons, Imagery & Illustration
## 8. Motion & Interaction
## 9. Responsive Behavior
## 10. Accessibility Rules
## 11. Do's and Don'ts
## 12. Agent Prompt Guide
```

## Token Rules

- Use semantic names first, raw values second.
- Include exact values for colors, sizes, weights, line heights, spacing, radii, shadows, border widths, opacity, and motion duration/easing.
- Include exact responsive values for breakpoints, maximum container widths, grid columns, gutters, minimum touch targets, and component collapse behavior.
- Use platform-neutral units. `px`, `rem`, hex, numeric weights, and CSS-like shadow strings are acceptable because they are widely understood.
- Do not require Tailwind, CSS variables, React, SwiftUI, Flutter, Figma variables, or any single implementation technology.
- If a downstream platform needs conversion, explain the mapping in the body instead of encoding platform-specific tokens as the source of truth.

## Body Rules

Each section must explain:

- What to use.
- When to use it.
- When not to use it.
- How an AI agent should apply it in generated UI.

The `Responsive Behavior` section must define:

- Mobile, tablet, desktop, and wide desktop breakpoint ranges.
- Container width and gutter behavior at each range.
- Navigation adaptation, such as sidebar to top tabs or drawer.
- Grid/list/table collapse behavior.
- Component sizing rules, including minimum 44px touch targets.
- Typography role adjustments, if any, without using viewport-width-scaled font formulas.
- Content-priority rules for hiding, stacking, truncating, or converting dense UI on small screens.

Keep examples concrete. Prefer:

```text
Primary buttons use color.action.primary.background on solid surfaces, 8px radius, 44px minimum height, and visible focus ring.
```

Avoid:

```text
Buttons should feel clean and modern.
```

## Agent Prompt Guide

End with a compact guide that later agents can reuse:

- Canonical files to read first.
- Top 8 tokens that define the system.
- Component defaults.
- Layout defaults.
- Responsive defaults.
- Hard constraints and anti-patterns.
- Instruction to preserve token names when generating code or Figma objects.

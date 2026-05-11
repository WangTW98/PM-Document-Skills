# Preview HTML Contract

Use this reference when generating `design/<design-system-slug>/preview.html`.

## Requirements

- Create one self-contained HTML file.
- Inline CSS in a `<style>` block.
- Do not use external fonts, CDNs, JavaScript packages, images, build tools, or remote assets.
- Use system font stacks unless the user provided local font assets or explicitly allowed web fonts.
- Use the exact tokens chosen for `DESIGN.md`.
- Include responsive CSS so the preview remains readable on narrow and wide screens.
- Include visible explanations for key design decisions, especially color roles, contrast intent, typography purpose, layout density, and responsive behavior.

## Required Preview Sections

1. Hero summary showing the design system name, thesis, and key mood words.
2. Color palette with swatches, token names, hex values, usage roles, contrast notes, and short guidance for when to use or avoid each color.
3. Typography scale with H1, H2, H3, body, small text, labels, and code/metadata style if relevant.
4. Spacing, layout grid, radius, border, and shadow samples.
5. Buttons: primary, secondary, quiet/ghost, destructive if relevant, disabled, focus.
6. Form controls: input, select-like field, checkbox/toggle if relevant, validation error, helper text.
7. Cards or panels using the real surface, border, radius, and shadow rules.
8. Navigation sample: header/sidebar/tab pattern appropriate for the described product.
9. Data/content sample: table, list, metric row, or content grid depending on product type.
10. States: empty, success, warning, error.
11. Responsive sample showing how navigation, grids, cards, tables/lists, and action groups collapse.
12. Agent handoff summary listing the most important tokens and anti-drift rules.

## Visual QA

Before finalizing:

- Ensure text does not overflow buttons, cards, swatches, or narrow columns.
- Ensure foreground/background color pairs meet stated contrast targets.
- Ensure focus rings are visible.
- Ensure the preview is not dominated by one hue unless the user explicitly requested monochrome.
- Ensure every displayed token exists in `DESIGN.md` and `tokens.json`.
- Ensure responsive sections demonstrate mobile, tablet, and desktop behavior with explicit labels or examples.

## Optional Starter Pattern

Use `assets/preview-starter.html` as a starting point when helpful, but replace all placeholder tokens, copy, and examples with the generated design system.

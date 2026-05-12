# Design2Figma Quality Checklist

Use this checklist after creating the Figma design and before finalizing.

## Required Checks

- Only one page design MD was processed.
- Only one Figma page design was created.
- The page design MD contains `布局完整性审核`, and every audit item is `通过` or `已解决`.
- Target application form was determined before writing: mobile app, PC web, responsive web, admin console, mini-program, tablet app, or another explicit form.
- The selected design system supports the target application form.
- If the user did not specify a form, `product/release/product-overview-release.md` and the selected page MD were used to infer the selected page's form.
- If the product overview contains multiple forms, the created Figma design matches the selected page's specific form rather than an unrelated product surface.
- The design was created in the intended Figma file.
- The design was created on the intended Figma page.
- The top-level Figma layer/frame name exactly follows `<md-file-name> / <page-name-from-md>`, for example `010-login.md / 登录页`.
- The `md-file-name` portion is the selected MD basename including `.md`.
- The `page-name-from-md` portion matches the MD's declared page name, preferring the `页面名称` metadata field.
- Layer hierarchy follows the page MD structure and AI-readable style structure.
- Parent-child hierarchy follows the MD and is not visually ambiguous.
- Main sections from the page MD exist as Figma frames or groups.
- Major visible elements from the page MD exist as Figma nodes.
- Text content matches the release design MD.
- Typography, colors, spacing, radius, strokes, and elevation follow token references from the MD/design system.
- Auto-layout direction, padding, gap, alignment, and resizing behavior were applied where specified.
- Min/max sizes, fill/hug/fixed sizing, wrapping/truncation, overflow, clip-content, scroll-axis, and layer-order rules match the MD.
- Responsive variants or frame sizes were created when specified.
- Frame size, navigation pattern, component behavior, and responsive variants match the verified application form.
- Responsive variants do not contain visible collisions, hidden key controls, unreadable compression, or inconsistent hierarchy.
- State display variants were created when specified as visual states.
- No unintended overlap, stacking, clipping, compressed unreadable text, hidden controls, or incorrect layer order exists in the created Figma output.
- Any intentional overlay, floating element, modal, badge, tooltip, or media overlap follows the MD's documented layer order and collision/viewport rules.
- No interaction prototypes, analytics layers, API annotations, backend diagrams, business workflow nodes, or implementation-code artifacts were created.
- Existing Figma content was not overwritten unless the user explicitly requested replacement.
- If screenshot or metadata verification is available, the created node was checked after writing.
- If screenshot or metadata verification revealed layout problems, the Figma nodes were adjusted before finalizing.

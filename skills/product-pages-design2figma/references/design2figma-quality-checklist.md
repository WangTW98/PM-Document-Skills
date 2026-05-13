# Design2Figma Quality Checklist

Use this checklist after creating the Figma design and before finalizing.

## Required Checks

- Only one page design MD was processed.
- Only one Figma page design was created.
- The page design MD contains `布局完整性审核`, and every audit item is `通过` or `已解决`.
- The page design MD contains `Figma Remote MCP 生成提示`.
- The page design MD contains an App Shell / Navigation Contract or equivalent structured `app_shell` data.
- Product-level shell/navigation rules were derived from `product/release/product-sitemap-release.md` when present.
- The selected page was classified as login/independent, tab-root, pushed child, or another explicit shell category before writing.
- Required shell regions were known before writing: root device frame, safe-area frame, top navigation, main scroll, bottom tab, fixed footer/bottom action, and named overlays.
- Every actionable item in `Figma Remote MCP 生成提示` was parsed and applied, or marked not applicable with a concrete reason.
- Frame creation order follows `Figma Remote MCP 生成提示`.
- Auto Layout, size constraints, overflow/clipping, layer order, token application, component grouping, text node naming, media placeholders, responsive variants, layout QA, and prohibited actions follow `Figma Remote MCP 生成提示`.
- Any conflict between `Figma Remote MCP 生成提示` and `AI 可读样式结构` was resolved by the more specific instruction, or stopped before writing if it affected hierarchy, sizing, layer order, responsive behavior, or visible content.
- Target application form was determined before writing: mobile app, PC web, responsive web, admin console, mini-program, tablet app, or another explicit form.
- The selected design system supports the target application form.
- If the user did not specify a form, `product/release/product-sitemap-release.md` and the selected page MD were used to infer the selected page's form.
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
- App Shell regions match the selected page category and product-level navigation: tab-root pages include Bottom Tab Bar; pushed child pages include Top Navigation Bar with back affordance; exceptions are explicitly justified.
- Bottom Tab Bar item labels, dimensions, selected-state styling, and layer naming are consistent across tab-root pages generated in the same run.
- Fixed Footer / Bottom Action reserves safe-area and scroll bottom inset and does not collide with Bottom Tab Bar, FAB, or main content.
- Responsive variants do not contain visible collisions, hidden key controls, unreadable compression, or inconsistent hierarchy.
- State display variants were created when specified as visual states.
- No unintended overlap, stacking, clipping, compressed unreadable text, hidden controls, or incorrect layer order exists in the created Figma output.
- No required navigation region is missing or visually inconsistent with sibling pages.
- Normal content regions, forms, lists, cards, navigation bars, and footers are implemented as Auto Layout frames, not loose absolute-positioned nodes.
- No abnormal wrapper dimensions exist, including small fixed frames containing wider/taller buttons, inputs, cards, or list rows.
- Parent frames do not clip non-overlay children unintentionally.
- Any intentional overlay, floating element, modal, badge, tooltip, or media overlap follows the MD's documented layer order and collision/viewport rules.
- No interaction prototypes, analytics layers, API annotations, backend diagrams, business workflow nodes, or implementation-code artifacts were created.
- Existing Figma content was not overwritten unless the user explicitly requested replacement.
- If screenshot or metadata verification is available, the created node was checked after writing.
- Metadata verification checked shell-region existence, Auto Layout presence on normal containers, wrapper-child size compatibility, and missing navigation.
- If screenshot or metadata verification revealed layout problems, the Figma nodes were adjusted before finalizing and not marked complete until fixed.

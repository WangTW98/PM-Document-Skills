# Page Design Release Quality Checklist

Use this checklist before writing the final response.

## Required Checks

- Exactly one `product/release/mock/...` page was used as the content source.
- `product/release/layout/product-layout-release.md` was read and used as the primary project-level layout dependency.
- Exactly one `design/<design-system>/` directory was used as the design constraint source.
- `DESIGN.md` from the selected design system was read.
- `tokens.json`, `visual-spec.md`, and `handoff/figma-remote-mcp-guide.md` were used when present and relevant.
- `product/release/product-overview-release.md` was read when present to reconcile sitemap/product context with the layout draft.
- Exactly one output file was created or updated under `product/release/design`.
- The output filename preserves the input mock filename relative to `product/release/mock`.
- The output document version is `Release`.
- The output contains both human-readable natural language style description and AI-readable structured style description.
- The output contains an App Shell / Navigation Contract section derived from `product-layout-release` Surface, Shell, page template, navigation position, global regions, and responsive rules.
- The design release does not depend on unresolved layout assumptions or any `LA-*` / `LQ-*` ID.
- The App Shell / Navigation Contract explicitly states Top Navigation Bar, Main Scroll Container, Bottom Tab Bar, Fixed Footer / Bottom Action, Safe Area, and any exception reasons.
- Tab-root mobile App pages include the product-level Bottom Tab Bar with consistent item labels, dimensions, selected state, and layer naming.
- Pushed L2/L3 mobile App pages include a consistent Top Navigation Bar with title and back affordance unless an explicit product-level exception exists.
- Fixed Footer / Bottom Action rules reserve bottom inset and do not collide with Bottom Tab Bar, FAB, or scroll content.
- Every major content section from the release mock page maps to a layout/style treatment.
- Every major visible element from the release mock page maps to an element-level visual definition.
- The design uses token names and exact values from the selected design system where available.
- Layout integrity audit is present and every audit row is `通过` or `已解决`.
- Every major frame has a clear parent, layout mode, sizing rule, padding, gap, alignment, overflow policy, and responsive behavior.
- Every major visible element has a clear parent container, minimum size, text wrapping/truncation behavior, and alignment rule.
- No unresolved page design rule would cause unintended overlap, stacking, clipping, compressed unreadable text, hidden controls, or incorrect layer order.
- No page omits required product-level navigation or shell regions without a recorded exception.
- Normal page structure uses Auto Layout frames; absolute positioning is limited to named background effects and intentional overlays with collision rules.
- Any intentional overlay, floating element, modal, badge, or media overlap has an explicit layer order, viewport/collision rule, and justification.
- Responsive rules are included for mobile, tablet, desktop, and wide desktop when applicable.
- Responsive rules explain how navigation, grids, cards, tables/lists, button groups, forms, media, and long text adapt without visual collisions.
- State display styles are present for relevant loading, empty, error, disabled, success, and media failure states.
- Figma Remote MCP handoff notes are included.
- Figma Remote MCP handoff notes include root frame hierarchy, safe-area regions, navigation bars, bottom tabs/footers, Auto Layout, constraints, overflow/clipping, layer order, and post-generation metadata QA requirements.
- Figma Remote MCP handoff notes include a check for abnormal wrapper dimensions, for example a small fixed frame containing a wider button or content node.
- The output contains no interaction execution, analytics, tracking, API contracts, backend behavior, implementation code, or business process logic.
- The output contains no `MA-*`, `MQ-*`, assumptions, open questions, or pending confirmation sections.

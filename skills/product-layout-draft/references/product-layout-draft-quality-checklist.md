# Product Layout Draft Quality Checklist

Use this checklist before writing the final response.

## Required Checks

- `product/release/product-sitemap-release.md` was read.
- `Sitemap 页面生成总表` was parsed.
- `product/development/layout/product-layout-draft.md` was created or updated.
- Every sitemap row appears in the sitemap-to-layout mapping table.
- Every product Surface has a Shell definition.
- Every root page has a navigation position.
- Every child page has a parent-child presentation rule.
- Every page has a page template ID or a clear exception.
- Global shell regions are explicit: top nav, side nav, bottom tabs, main content, fixed footer/action area, modal/drawer layer, and safe-area behavior when relevant.
- Responsive/adaptive behavior is defined for each surface.
- Global loading, empty, error, offline, permission, quota/payment states have layout treatment when relevant.
- Role/permission impacts on layout are described.
- The Mermaid diagram matches the surface/shell/page hierarchy.
- Every `LA-*` and `LQ-*` used in the document appears in the final unified list.
- Inferred layout choices are marked with `LA-*` or `LQ-*`.
- The document ends with `## 12. 用户补充描述`.
- The `用户补充描述` section contains an editable fenced text area for user natural-language layout modifications and does not contain generated layout requirements.
- The document does not define detailed page-level elements, API contracts, analytics events, or final visual design tokens.

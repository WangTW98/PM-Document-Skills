# Release Quality Checklist

Use this checklist before writing the final response.

## Required Checks

- `product/release/product-overview-release.md` exists.
- The document version is `Release`.
- The document contains no `A-001`, `A-002`, `Q-001`, `Q-002`, or other `A-\d+` / `Q-\d+` references.
- The document contains no `假设`, `待确认`, `待用户确认`, `Release 处理方式`, `用户确认状态`, or `用户确认结果`.
- The final release document does not include the draft's `假设与待确认统一清单`.
- All formerly uncertain content has been applied, replaced, or removed according to the draft's release handling.
- Sitemap table, Mermaid hierarchy, surface tree, and page generation queue refer to the same pages.
- Every sitemap row has a valid page ID, parent page ID, level, generation order, page name, function description, and page-level Markdown path.
- No sitemap row references a deleted parent page.
- Page-level Markdown paths are unique.
- If release is blocked, `product/release/product-overview-release-blockers.md` lists only unresolved blocking items and no release file is written.

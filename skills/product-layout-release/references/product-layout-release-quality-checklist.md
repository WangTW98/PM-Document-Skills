# Product Layout Release Quality Checklist

Use this checklist before writing the final response.

## Required Checks

- `product/development/layout/product-layout-draft.md` was read.
- The final `布局假设与待确认统一清单` was located and parsed.
- Every inline `LA-*` and `LQ-*` reference was represented in the final list before release processing.
- Every `LA-*` row has a usable `用户确认状态` and `Release 处理方式`.
- Every `LQ-*` row has a usable `用户确认结果` and `Release 处理方式`.
- Exactly one release layout file was created or updated at `product/release/layout/product-layout-release.md`, or one blocker file was written.
- The release layout document version is `Release`.
- The release layout contains no `LA-001`, `LA-002`, `LQ-001`, `LQ-002`, or other `LA-\d+` / `LQ-\d+` references.
- The release layout contains no `布局假设`, `布局待确认`, `假设`, `待确认`, `待用户确认`, `Release 处理方式`, `用户确认状态`, `用户确认结果`, or `置信度`.
- The final release layout does not include the draft's `布局假设与待确认统一清单`.
- All formerly uncertain layout content was applied, replaced, or removed according to release handling.
- Surface/Shell definitions, global shell regions, page templates, sitemap-to-layout mapping, navigation rules, global states, and role/permission layout effects remain internally consistent.
- Every sitemap row remains mapped to a layout Surface, Shell, page template, navigation position, and responsive rule.
- Downstream usage rules point to `product/release/layout/product-layout-release.md`.
- If release is blocked, the blocker file lists only unresolved blocking items and no release layout file is written.

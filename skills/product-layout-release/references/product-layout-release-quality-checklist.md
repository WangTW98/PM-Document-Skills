# Product Layout Release Quality Checklist

Use this checklist before writing the final response.

## Required Checks

- The relevant layout draft file or files under `product/development/layout` were read.
- The final `布局假设与待确认统一清单` was located and parsed.
- `## 12. 用户补充描述` was located when present, and its content was classified as empty/placeholder or non-empty user modification instructions.
- Every non-empty user supplement instruction was analyzed and applied to all affected release layout sections, or release was blocked with a clear conflict/ambiguity reason.
- Every inline `LA-*` and `LQ-*` reference was represented in the final list before release processing.
- Every `LA-*` row has a usable `用户确认状态` and `Release 处理方式`.
- Every `LQ-*` row has a usable `用户确认结果` and `Release 处理方式`.
- One or more release layout files were created or updated under `product/release/layout`, or matching blocker files were written for blocked families.
- The release layout document version is `Release`.
- The release layout contains no `LA-001`, `LA-002`, `LQ-001`, `LQ-002`, or other `LA-\d+` / `LQ-\d+` references.
- The release layout contains no `布局假设`, `布局待确认`, `假设`, `待确认`, `待用户确认`, `Release 处理方式`, `用户确认状态`, `用户确认结果`, or `置信度`.
- The release layout contains no `用户补充描述` section or raw user supplement notes.
- The final release layout does not include the draft's `布局假设与待确认统一清单`.
- All formerly uncertain layout content was applied, replaced, or removed according to release handling.
- Surface/Shell definitions, global shell regions, page templates, sitemap-to-layout mapping, navigation rules, global states, and role/permission layout effects remain internally consistent.
- User supplement changes are reflected consistently in Surface/Shell definitions, global shell regions, page templates, sitemap-to-layout mapping, navigation rules, global states, responsive behavior, Mermaid layout map, and downstream usage rules when those areas are affected.
- Every sitemap row remains mapped to a layout Surface, Shell, page template, navigation position, and responsive rule.
- Each release layout file contains `Layout Key`, `适用 Surface`, `适用端形态`, and `覆盖页面ID / 页面级MD文件范围` metadata.
- If multiple release layout files exist, each filename is clearly distinguished by a meaningful `layout-key`.
- Downstream usage rules point to the matched `product/release/layout/product-layout-release*.md` file, not blindly to one generic file.
- If release is blocked, the blocker file lists only unresolved blocking items and no release layout file is written.

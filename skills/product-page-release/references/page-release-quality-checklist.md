# Page Release Quality Checklist

Use this checklist before writing the final response.

## Required Checks

- The selected input file is one page under `product/development/pages`.
- No more than one draft page file was read for release processing.
- Exactly one release page file was created or updated under `product/release/pages`.
- The release page filename preserves the input page filename relative to `product/development/pages`.
- The document version is `Release`.
- The document contains no `PA-001`, `PA-002`, `PQ-001`, `PQ-002`, or other `PA-\d+` / `PQ-\d+` references.
- The document contains no `页面假设`, `页面待确认`, `假设`, `待确认`, `待用户确认`, `Release 处理方式`, `用户确认状态`, `用户确认结果`, or `置信度`.
- The final release page does not include the draft's `页面假设与待确认统一清单`.
- All formerly uncertain content has been applied, replaced, or removed according to the draft page's release handling.
- Mermaid diagram, layout section table, element inventory, state matrix, action matrix, data model, API contracts, edge cases, and media/resources remain internally consistent.
- No element state references a deleted element.
- No action references a deleted element.
- No API references a deleted action.
- If release is blocked, the blocker file lists only unresolved blocking items and no release page is written.

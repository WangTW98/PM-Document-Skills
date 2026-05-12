# Design Release Orchestration Quality Checklist

Use this checklist before finishing an orchestration run.

- `product/release/product-overview-release.md` was read.
- `Sitemap 页面生成总表` was parsed.
- Exactly one `design/<design-system>/` directory was selected.
- The selected design system contains `DESIGN.md`.
- `product/release/design/_generation-status.md` exists.
- Every sitemap row appears in the status queue unless the row is malformed and recorded as blocked.
- The current run selected pages by ascending `生成顺序`.
- Each selected page used a release mock input source under `product/release/mock`.
- The sitemap row's `页面级MD文件` was used only as a filename / relative filename key.
- Each design release output path preserves the same relative filename under `product/release/design`.
- No page was marked `已完成` unless the design release file exists.
- No completed design release page was overwritten unless the user explicitly requested regeneration.
- Blocked pages include concrete, actionable reasons.
- Design release pages contain natural language style description.
- Design release pages contain AI-readable style structure.
- Design release pages contain Figma Remote MCP handoff notes.
- Design release pages contain layout integrity audit sections.
- No page was marked `已完成` unless its layout integrity audit is present and every item is `通过` or `已解决`.
- No page was marked `已完成` if it contains unresolved stacking, compression, clipping, unintended overlap, hidden-control, ambiguous hierarchy, or layer-order risks.
- Existing design release files were not reused as complete unless they satisfy the current product-page-design-release layout integrity requirements.
- Design release pages contain no `MA-*`, `MQ-*`, `假设`, or `待确认`.
- Design release pages remain display-only and contain no interaction execution, analytics, API contracts, backend behavior, business process logic, or implementation code.
- If the run stopped before all pages were complete, `_generation-status.md` clearly shows the next `待生成` page.
- If all pages are complete, `_generation-status.md` status counts match the queue.

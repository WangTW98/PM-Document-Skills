# Mock Release Orchestration Quality Checklist

Use this checklist before finishing an orchestration run.

- `product/release/product-overview-release.md` was read.
- `Sitemap 页面生成总表` was parsed.
- `product/release/mock/_generation-status.md` exists.
- Every sitemap row appears in the status queue unless the row is malformed and recorded as blocked.
- The current run selected pages by ascending `生成顺序`.
- Each selected page used a mock draft input source under `product/development/mock`.
- The sitemap row's `页面级MD文件` was used only as a filename / relative filename key.
- Each release mock output path was derived by replacing `product/development/mock` with `product/release/mock`.
- No page was marked `已完成` unless the release mock file exists.
- No completed release mock page was overwritten unless the user explicitly requested regeneration.
- Blocked pages include concrete, actionable reasons.
- Release mock pages contain no `MA-*`, `MQ-*`, `Mock 假设`, `Mock 待确认`, `假设`, or `待确认`.
- Release mock pages remain content-only and contain no interaction execution, analytics, API contracts, backend behavior, or implementation code.
- Release mock pages preserve confirmed `内容来源类型` values and dynamic source labels where applicable.
- If the run stopped before all pages were complete, `_generation-status.md` clearly shows the next `待生成` page.
- If all pages are complete, `_generation-status.md` status counts match the queue.

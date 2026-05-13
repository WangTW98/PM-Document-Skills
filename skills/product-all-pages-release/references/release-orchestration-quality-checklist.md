# Release Orchestration Quality Checklist

Use this checklist before finishing an orchestration run.

- `product/release/product-sitemap-release.md` was read.
- `Sitemap 页面生成总表` was parsed.
- `product/release/pages/_generation-status.md` exists.
- Every sitemap row appears in the status queue unless the row is malformed and recorded as blocked.
- The current run selected pages by ascending `生成顺序`.
- Each selected page used the row's `页面级MD文件` as the draft source file.
- Draft source pages used for release contain `## 7. 埋点事件统计设计`, `埋点事件ID`, and at least one `EVT-*` event ID.
- Each release page path was derived by replacing `product/development/pages` with `product/release/pages`.
- No page was marked `已完成` unless the release file exists.
- No completed release page was overwritten unless the user explicitly requested regeneration.
- Blocked pages include concrete, actionable reasons.
- Release pages contain no `PA-*`, `PQ-*`, `页面假设`, `页面待确认`, `假设`, or `待确认`.
- Release pages contain `埋点事件ID` and at least one `EVT-*` event ID.
- If the run stopped before all pages were complete, `_generation-status.md` clearly shows the next `待生成` page.
- If all pages are complete, `_generation-status.md` status counts match the queue.

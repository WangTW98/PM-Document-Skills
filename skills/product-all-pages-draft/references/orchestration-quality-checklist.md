# Orchestration Quality Checklist

Use this checklist before finishing an orchestration run.

- `product/release/product-overview-release.md` was read.
- `Sitemap 页面生成总表` was parsed.
- `product/development/pages/_generation-status.md` exists.
- Every sitemap row appears in the status queue unless the row is malformed and recorded as blocked.
- The current run selected pages by ascending `生成顺序`.
- Each generated page used the exact `页面级MD文件` output path.
- No page was marked `已完成` unless the file exists.
- No page was marked `已完成` unless it contains `## 7. 埋点事件统计设计`.
- No page was marked `已完成` unless it contains `埋点事件ID` and at least one `EVT-*` event ID.
- No completed page was overwritten unless the user explicitly requested regeneration.
- Blocked pages include concrete, actionable reasons.
- If the run stopped before all pages were complete, `_generation-status.md` clearly shows the next `待生成` page.
- If all pages are complete, `_generation-status.md` status counts match the queue.

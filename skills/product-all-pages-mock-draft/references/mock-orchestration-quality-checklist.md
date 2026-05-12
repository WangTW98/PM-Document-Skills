# Mock Orchestration Quality Checklist

Use this checklist before finishing an orchestration run.

- `product/release/product-overview-release.md` was read.
- `Sitemap 页面生成总表` was parsed.
- `product/development/mock/_generation-status.md` exists.
- Every sitemap row appears in the status queue unless the row is malformed and recorded as blocked.
- The current run selected pages by ascending `生成顺序`.
- Each selected page used a release page source derived from the row's `页面级MD文件`.
- Each mock output path was derived by replacing `product/release/pages` with `product/development/mock`.
- No page was marked `已完成` unless the mock file exists.
- No completed mock file was overwritten unless the user explicitly requested regeneration.
- Blocked pages include concrete, actionable reasons.
- Mock files contain `来源对照矩阵`.
- Mock files contain `内容来源类型` values for content rows.
- Mock files map `页面布局与内容区块`, `页面元素清单`, `元素状态矩阵`, and `交互 Action 与执行效果` or give explicit unmapped reasons.
- Mock files do not include interaction execution logic, analytics/tracking logic, API definitions, request/response schemas, backend behavior, or implementation code.
- If the run stopped before all pages were complete, `_generation-status.md` clearly shows the next `待生成` page.
- If all pages are complete, `_generation-status.md` status counts match the queue.

# Page Processing Orchestration Quality Checklist

Use this checklist before finishing an orchestration run.

- Mode was determined first. Draft revision mode is the default unless the user explicitly asked for final/release/正式版 pages.
- `product/release/product-sitemap-release.md` was read.
- `Sitemap 页面生成总表` was parsed.
- In draft revision mode, `product/development/pages/_revision-status.md` exists.
- In final release mode, `product/release/pages/_generation-status.md` exists.
- Every sitemap row appears in the status queue unless the row is malformed and recorded as blocked.
- The current run selected pages by ascending `生成顺序`.
- Each selected page used the row's `页面级MD文件` as the canonical draft source file and the latest versioned sibling as the actual processing source when one exists.
- Draft source pages contain `## 7. 埋点事件统计设计`, `埋点事件ID`, and at least one `EVT-*` event ID.
- In draft revision mode, each output is the next versioned draft beside the source draft.
- In final release mode, each release page path was derived from the canonical `页面级MD文件` by replacing `product/development/pages` with `product/release/pages`; version suffixes were not carried into release filenames.
- No page was marked `已完成` unless the selected mode's expected output file exists.
- No completed release page or versioned draft was overwritten unless the user explicitly requested regeneration.
- Blocked pages include concrete, actionable reasons.
- In draft revision mode, revised drafts keep remaining/new `PA-*` / `PQ-*` workflow rows and an empty final `用户补充描述`.
- In final release mode, release pages contain no `PA-*`, `PQ-*`, `页面假设`, `页面待确认`, `假设`, or `待确认`.
- Output pages contain `埋点事件ID` and at least one `EVT-*` event ID.
- If the run stopped before all pages were complete, the selected mode's status file clearly shows the next `待生成` page.
- If all pages are complete, the selected mode's status counts match the queue.

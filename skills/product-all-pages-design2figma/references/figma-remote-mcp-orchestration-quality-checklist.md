# Figma Remote MCP Orchestration Quality Checklist

Use this checklist before finishing an orchestration run.

- `product/release/product-overview-release.md` was read.
- `Sitemap 页面生成总表` was parsed.
- Exactly one Figma link was provided and recorded.
- The Figma `fileKey` was resolved.
- The target Figma page or target node was resolved before writing.
- `product/release/design/_figma-remote-mcp-status.md` exists.
- Every sitemap row appears in the status queue unless the row is malformed and recorded as blocked.
- The current run selected pages by ascending `生成顺序`.
- Each selected page used a design release input source under `product/release/design`.
- The sitemap row's `页面级MD文件` was used only as a filename / relative filename key.
- No row was marked `已完成` unless Figma output was created in the intended file and target page.
- No completed Figma frame was overwritten unless the user explicitly requested replacement/regeneration.
- Blocked rows include concrete, actionable reasons.
- Each completed row records target Figma page, top-level frame name, and created node ID when available.
- Each Figma creation followed `product-pages-design2figma` single-page rules.
- No row was marked `已完成` unless the source design MD contained an App Shell / Navigation Contract or equivalent structured `app_shell` data.
- No row was marked `已完成` unless post-write verification checked required shell regions: root device frame, safe-area frame, top navigation, main scroll, bottom tab or fixed footer when applicable.
- No row was marked `已完成` if metadata showed abnormal wrapper dimensions, missing navigation, child nodes wider/taller than non-overlay parents, hidden key controls, or normal content implemented as loose absolute-positioned nodes.
- Tab-root pages share consistent Bottom Tab labels, dimensions, selected states, and naming when generated in the same run.
- No interaction prototypes, analytics layers, API annotations, backend diagrams, business workflow nodes, or implementation-code artifacts were created.
- If the run stopped before all pages were complete, `_figma-remote-mcp-status.md` clearly shows the next `待生成` page.
- If all pages are complete, `_figma-remote-mcp-status.md` status counts match the queue.

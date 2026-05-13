# Gemini Adapter

Use the workflow at `../../SKILL.md`, but this skill requires a Figma Remote MCP-compatible environment.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for orchestration workflow.
3. Read `references/figma-remote-mcp-status-template.md` before creating `_figma-remote-mcp-status.md`.
4. Read `references/figma-remote-mcp-orchestration-quality-checklist.md` before ending a run.
5. Load `product/release/product-sitemap-release.md` and parse `Sitemap 页面生成总表`.
6. Confirm one Figma link and one target Figma page or target node.
7. Process pages by ascending `生成顺序`.
8. For each selected page, use only `product/release/design/<same-relative-page-filename>.md` as the single-page source; the row's `页面级MD文件` is only a filename key.
9. Follow `skills/product-pages-design2figma/SKILL.md` and create exactly one Figma page/frame before updating status.
10. Continue until all rows are complete or until the run should pause to preserve accuracy; status must make resumption unambiguous.

# Gemini Adapter

Use the workflow at `../../SKILL.md`, but this skill requires a Figma Remote MCP-compatible environment.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for orchestration workflow.
3. Read `references/figma-remote-mcp-status-template.md` before creating the status file.
4. Read `references/figma-remote-mcp-orchestration-quality-checklist.md` before ending a run.
5. Require one explicit design specification created by `visual-design-spec`, such as `design/<slug>/DESIGN.md` or `design/<slug>/`.
6. If the user did not explicitly specify the design specification, stop immediately and ask for it.
7. Require one Figma link from the user. If missing, stop immediately and ask for it.
8. Load `product/release/product-sitemap-release.md` and parse `Sitemap 页面生成总表`.
9. Read all release layout files under `product/release/layout` and all release pages under `product/release/pages`.
10. If the user did not specify the application form, infer it from the sitemap and release layout metadata. If some rows remain ambiguous, stop and ask.
11. For each sitemap row, resolve one unique matched release layout family from `product-sitemap-release.md` plus all release layout md files. If no unique match exists, mark that row blocked.
12. Create or reuse all release layout components first.
13. Maintain `product/release/pages/_design2figma-status.md` as the orchestration status file.
14. Process pages by ascending `生成顺序`.
15. For each row, follow `skills/product-pages-design2figma/SKILL.md`, create exactly one release page frame using only its matched layout family, and update status immediately.
16. Name every created page frame as `<sitemap 页面级MD文件 basename>-<页面ID>-<页面标题>`.
17. Do not use `product/release/design/*.md` as the primary source.
18. Continue until all rows are complete or until the run should pause to preserve accuracy; status must make resumption unambiguous.

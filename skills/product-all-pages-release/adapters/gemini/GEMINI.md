# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for orchestration workflow and mode rules.
3. Read `references/release-generation-status-template.md` before creating the selected mode's status file.
4. Read `references/release-orchestration-quality-checklist.md` before ending a run.
5. Load `product/release/product-sitemap-release.md` and parse `Sitemap 页面生成总表`.
6. Process pages by ascending `生成顺序`.
7. For each sitemap row, use the latest versioned page draft when present; derive final release filenames from the canonical `页面级MD文件`.
8. Determine mode first. Unless the user explicitly asks for final/release/正式版 pages, use draft revision mode and maintain `product/development/pages/_revision-status.md`.
9. For each selected page, follow `skills/product-page-release/SKILL.md` in the selected mode and process exactly one page before updating status, including analysis and application of non-empty `用户补充描述`.
10. In final release mode, maintain `product/release/pages/_generation-status.md` and generate release pages under `product/release/pages`.
11. Continue until all rows are complete or until the run should pause to preserve accuracy; status must make resumption unambiguous.

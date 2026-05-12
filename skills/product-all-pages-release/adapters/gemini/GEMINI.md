# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for orchestration workflow.
3. Read `references/release-generation-status-template.md` before creating `_generation-status.md`.
4. Read `references/release-orchestration-quality-checklist.md` before ending a run.
5. Load `product/release/product-overview-release.md` and parse `Sitemap 页面生成总表`.
6. Process pages by ascending `生成顺序`.
7. For each selected page, follow `skills/product-page-release/SKILL.md` and release exactly one page before updating status, including analysis and application of non-empty `用户补充描述`.
8. Continue until all rows are complete or until the run should pause to preserve accuracy; status must make resumption unambiguous.

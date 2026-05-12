# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for orchestration workflow.
3. Read `references/design-release-generation-status-template.md` before creating `_generation-status.md`.
4. Read `references/design-release-orchestration-quality-checklist.md` before ending a run.
5. Load `product/release/product-overview-release.md` and parse `Sitemap 页面生成总表`.
6. Confirm exactly one `design/<design-system>/` directory is selected.
7. Process pages by ascending `生成顺序`.
8. For each selected page, use only `product/release/mock/<same-relative-page-filename>.md` as the single-page content source; the row's `页面级MD文件` is only a filename key.
9. Follow `skills/product-page-design-release/SKILL.md` and generate exactly one design release page before updating status.
10. Continue until all rows are complete or until the run should pause to preserve accuracy; status must make resumption unambiguous.

# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for orchestration workflow.
3. Load `product/release/product-sitemap-release.md` and parse `Sitemap 页面生成总表`.
4. Maintain `product/development/pages/_revision-status.md`.
5. Process pages by ascending `生成顺序`.
6. For each sitemap row, use the latest versioned page draft when present.
7. For each selected page, follow `skills/product-page-draft-update/SKILL.md` and update exactly one page before updating status.
8. Reject outputs that omit required page draft template sections.
9. Reject version-only copies and do not write `product/release/pages`.
10. Continue until all rows are complete or until the run should pause to preserve accuracy; status must make resumption unambiguous.

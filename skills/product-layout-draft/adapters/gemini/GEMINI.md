# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the layout-draft workflow.
3. Read `references/product-layout-draft-template.md` before writing the output.
4. Read `references/product-layout-draft-quality-checklist.md` before finalizing.
5. Load `product/release/product-overview-release.md`.
6. Parse `Sitemap 页面生成总表`.
7. Create `product/development/layout/product-layout-draft.md` for user confirmation before `product-layout-release` creates the formal release layout.
8. Mark incomplete layout decisions with `LA-*` and `LQ-*`, and include them in the final unified list.

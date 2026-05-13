# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the layout-draft workflow.
3. Read `references/product-layout-draft-template.md` before writing the output.
4. Read `references/product-layout-draft-quality-checklist.md` before finalizing.
5. Load `product/release/product-sitemap-release.md`.
6. Parse `Sitemap 页面生成总表`.
7. If the product has one layout family, create `product/development/layout/product-layout-draft.md`; if it has multiple layout families, create one clearly named `product-layout-draft-<layout-key>.md` file per family for user confirmation before `product-layout-release` creates the formal release layouts.
8. Mark incomplete layout decisions with `LA-*` and `LQ-*`, and include them in the final unified list.
9. End the draft with `## 12. 用户补充描述` so the user can add natural-language layout modifications before release.

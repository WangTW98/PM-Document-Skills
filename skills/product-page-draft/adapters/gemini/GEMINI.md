# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the single-page workflow and hard rules.
3. Use `references/page-draft-template.md` as the required page MD structure.
4. Use `references/page-draft-quality-checklist.md` before finalizing output.
5. Load `product/release/product-overview-release.md`, load `product/release/layout/product-layout-release.md`, and parse `Sitemap 页面生成总表`.
6. Generate exactly one target page per invocation.
7. Save the result to the exact path in the target row's `页面级MD文件` column.
8. End the draft with `## 12. 用户补充描述` so the user can add natural-language page modifications before release.

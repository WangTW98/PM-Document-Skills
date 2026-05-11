# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the workflow and quality bar.
3. Use `references/product-overview-template.md` as the required Markdown document structure.
4. Write generated output to `product/development/product-overview-draft.md` or `product/development/product-overview-release.md`.
5. Keep all unconfirmed inferences visibly marked with stable IDs such as `假设 A-001` or `待确认 Q-001`.
6. Treat the sitemap table as the canonical downstream task list. Every page, child page, tertiary page, and meaningful subview must have a row with `页面ID`, `父页面ID`, `层级`, `生成顺序`, and `页面级MD文件`.
7. Ensure the Mermaid hierarchy mirrors the sitemap table exactly.
8. Mark all assumptions and open questions inline with stable IDs (`A-001`, `Q-001`) and include every ID in the final `假设与待确认统一清单`.

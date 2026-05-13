# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the workflow and quality bar.
3. Use `references/product-overview-template.md` as a strict section contract, not loose guidance.
4. Write generated output to `product/development/product-sitemap-draft.md`.
5. If the user asks for a release version, hand off to `product-sitemap-release` so it writes `product/release/product-sitemap-release.md`.
6. Keep all unconfirmed inferences visibly marked with stable IDs such as `假设 A-001` or `待确认 Q-001`.
7. Treat the sitemap table as the canonical downstream task list. Every page, child page, tertiary page, and meaningful subview must have a row with `页面ID`, `父页面ID`, `层级`, `生成顺序`, and `页面级MD文件`.
8. Ensure the Mermaid hierarchy mirrors the sitemap table exactly.
9. Mark all assumptions and open questions inline with stable IDs (`A-001`, `Q-001`) and include every ID in the final `假设与待确认统一清单`.
10. Preserve the exact top-level section order from the template and end draft output with `## 6. 用户补充描述` so the user can add natural-language product and sitemap modifications before release.

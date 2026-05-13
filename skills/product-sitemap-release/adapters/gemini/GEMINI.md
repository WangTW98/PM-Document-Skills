# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the workflow and release rules.
3. Read `references/release-quality-checklist.md` before finalizing output.
4. Load `product/development/product-sitemap-draft.md`.
5. Apply every release handling decision from the final `假设与待确认统一清单`.
6. Locate `## 6. 用户补充描述`; if it contains non-placeholder user modifications, analyze and apply them to the release content and sitemap.
7. Write `product/release/product-sitemap-release.md` only when all material assumptions, confirmations, and supplement instructions have concrete release handling.
8. Ensure the release document contains no `A-*`, `Q-*`, assumptions, open questions, confirmation workflow sections, or raw `用户补充描述` notes.

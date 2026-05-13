# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the workflow, mode rules, and release rules.
3. Read `references/release-quality-checklist.md` before finalizing output.
4. Load the explicitly named draft, or the latest `product/development/product-sitemap-draft-v<N>.md` when present; otherwise load `product/development/product-sitemap-draft.md`.
5. Determine mode first. Unless the user explicitly asks for final/release/正式版 output, use draft revision mode.
6. Apply every completed handling decision from the final `假设与待确认统一清单`.
7. Locate `## 6. 用户补充描述`; if it contains non-placeholder user modifications, analyze and apply them to the document and sitemap.
8. In draft revision mode, substantively regenerate the next versioned draft beside the source draft by applying user confirmations and `用户补充描述` into the document body; keep remaining/new `A-*` / `Q-*` workflow rows, reset `用户补充描述` to an empty placeholder, and reject version-only copies.
9. In final release mode, write `product/release/product-sitemap-release.md` only when all material assumptions, confirmations, and supplement instructions have concrete release handling.
10. Ensure the final release document contains no `A-*`, `Q-*`, assumptions, open questions, confirmation workflow sections, or raw `用户补充描述` notes.

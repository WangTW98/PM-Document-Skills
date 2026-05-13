# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the layout workflow and mode rules.
3. Read `references/product-layout-release-quality-checklist.md` before finalizing.
4. Load the explicitly named draft, or the latest `product/development/layout/product-layout-draft-v<N>.md` when present; otherwise load `product/development/layout/product-layout-draft.md`.
5. Determine mode first. Unless the user explicitly asks for final/release/正式版 output, use draft revision mode.
6. Apply every completed `LA-*` / `LQ-*` handling decision from the final `布局假设与待确认统一清单`.
7. Locate `## 12. 用户补充描述`; if it contains non-placeholder user modifications, analyze and apply them to the layout.
8. In draft revision mode, write the next versioned draft beside the source draft, keep remaining/new `LA-*` / `LQ-*` workflow rows, and reset `用户补充描述` to an empty placeholder.
9. In final release mode, save the confirmed release layout to `product/release/layout/product-layout-release.md`.
10. Do not include assumptions, open questions, draft-only release handling, raw `用户补充描述` notes, or uncertainty markers in final release output.

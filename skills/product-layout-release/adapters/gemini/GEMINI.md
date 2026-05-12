# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the layout release workflow.
3. Read `references/product-layout-release-quality-checklist.md` before finalizing.
4. Load `product/development/layout/product-layout-draft.md`.
5. Apply every concrete `LA-*` / `LQ-*` Release handling decision from the final `布局假设与待确认统一清单`.
6. Save the confirmed release layout to `product/release/layout/product-layout-release.md`.
7. Do not include assumptions, open questions, draft-only release handling, or uncertainty markers.

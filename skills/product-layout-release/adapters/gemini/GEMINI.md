# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the layout release workflow.
3. Read `references/product-layout-release-quality-checklist.md` before finalizing.
4. Read `product/release/product-sitemap-release.md`, then load the explicitly named layout draft files or the latest draft for each active layout family under `product/development/layout`.
5. Apply every concrete `LA-*` / `LQ-*` Release handling decision from the final `布局假设与待确认统一清单`.
6. Locate `## 12. 用户补充描述`; if it contains non-placeholder user modifications, analyze and apply them to the release layout.
7. Save the confirmed release layout set under `product/release/layout`: use `product-layout-release.md` for single-family products or one `product-layout-release-<layout-key>.md` file per family for multi-family products.
8. Do not include assumptions, open questions, draft-only release handling, raw `用户补充描述` notes, or uncertainty markers.

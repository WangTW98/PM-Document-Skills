# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the draft update workflow.
3. Load the explicitly named draft, or the latest `product/development/layout/product-layout-draft-v<N>.md` when present; otherwise load `product/development/layout/product-layout-draft.md`.
4. Merge user-confirmed LA-/LQ- decisions and non-placeholder `用户补充描述` into the layout body.
5. Reconcile Surface/Shell definitions, sitemap-to-layout mapping, responsive rules, downstream usage rules, and Mermaid layout map.
6. Preserve or add `LA-*` / `LQ-*` rows for remaining/new uncertainty and reset `用户补充描述` to an empty placeholder.
7. Save the next versioned draft under `product/development/layout`; do not write `product/release/layout`.

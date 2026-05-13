# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the draft update workflow.
3. Load `skills/product-layout-draft/references/product-layout-draft-template.md`.
4. Read `product/release/product-sitemap-release.md`, then load the explicitly named layout draft files or the latest draft for each active layout family under `product/development/layout`.
5. Merge user-confirmed LA-/LQ- decisions and non-placeholder `用户补充描述` into the layout body.
6. Reconcile Surface/Shell definitions, sitemap-to-layout mapping, responsive rules, downstream usage rules, and Mermaid layout map.
7. Preserve every required template chapter/table in order; restore missing sections from the template.
8. Preserve or add `LA-*` / `LQ-*` rows for remaining/new uncertainty and reset `用户补充描述` to an empty placeholder.
9. Save the next versioned draft under `product/development/layout`; do not write `product/release/layout`.

# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the draft update workflow.
3. Load the explicitly named draft, or the latest `product/development/product-sitemap-draft-v<N>.md` when present; otherwise load `product/development/product-sitemap-draft.md`.
4. Merge user-confirmed A-/Q- decisions and non-placeholder `用户补充描述` into the product overview and sitemap body.
5. Reconcile sitemap table, Mermaid hierarchy, page generation queue, dependencies, and source notes.
6. Preserve or add `A-*` / `Q-*` rows for remaining/new uncertainty and reset `用户补充描述` to an empty placeholder.
7. Save the next versioned draft under `product/development`; do not write `product/release/product-sitemap-release.md`.

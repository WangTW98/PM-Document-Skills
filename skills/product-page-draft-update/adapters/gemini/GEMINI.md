# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the draft update workflow.
3. Load `skills/product-page-draft/references/page-draft-template.md`.
4. Load exactly one draft page under `product/development/pages`, using the latest versioned sibling when present unless the user explicitly selected an exact file.
5. Merge user-confirmed PA-/PQ- decisions and non-placeholder `用户补充描述` into the page body.
6. Reconcile element inventory, state matrix, action matrix, analytics events, data/API sections, edge cases, media/resources, and Mermaid diagram.
7. Preserve every required template chapter/table in order; restore missing sections from the template.
8. Preserve or add `PA-*` / `PQ-*` rows for remaining/new uncertainty, keep analytics content, and reset `用户补充描述` to an empty placeholder.
9. Save the next versioned draft under `product/development/pages`; do not write `product/release/pages`.

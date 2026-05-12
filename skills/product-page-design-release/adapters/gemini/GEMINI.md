# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the single-page design release workflow.
3. Read `references/page-design-release-template.md` before writing the output.
4. Read `references/page-design-release-quality-checklist.md` before finalizing.
5. Load exactly one `product/release/mock/...` page.
6. Load `product/release/layout/product-layout-release.md`.
7. Load exactly one selected `design/<design-system>/` directory, starting with `DESIGN.md`.
8. Save the output to `product/release/design` with the same relative filename.
9. Include both natural language style description and AI-readable style structure.
10. Do not include interaction execution, analytics, API contracts, backend behavior, business logic, or implementation code.

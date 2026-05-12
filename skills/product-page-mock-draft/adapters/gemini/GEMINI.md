# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the content-only workflow and hard rules.
3. Use `references/page-mock-draft-template.md` as the required mock MD structure.
4. Use `references/page-mock-quality-checklist.md` before finalizing output.
5. Load exactly one release page under `product/release/pages`.
6. Save the mock draft to `product/development/mock` using the same relative filename.
7. Do not include interaction execution, analytics, API contracts, or backend behavior.

# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the single-page release workflow and hard rules.
3. Use `references/page-mock-release-quality-checklist.md` before finalizing output.
4. Load exactly one mock draft page under `product/development/mock`.
5. Apply every concrete `MA-*` / `MQ-*` Release handling decision from the final `Mock 假设与待确认统一清单`.
6. Save the confirmed release mock to `product/release/mock` using the same relative filename.
7. Do not process directories, all pages, or multiple mock files in one execution. Stop after one page to avoid context overflow and inaccurate content.
8. Do not include assumptions, open questions, interaction execution, analytics, API contracts, backend behavior, or implementation code.

# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the workflow and release rules.
3. Read `references/page-release-quality-checklist.md` before finalizing output.
4. Load exactly one draft page under `product/development/pages`.
5. If multiple pages or a directory are provided, stop and request one target page; do not batch release pages.
6. Apply every release handling decision from the final `页面假设与待确认统一清单`.
7. Locate `## 12. 用户补充描述`; if it contains non-placeholder user modifications, analyze and apply them to the release content.
8. Write exactly one release page to `product/release/pages` using the same relative filename.
9. Ensure the release document contains no `PA-*`, `PQ-*`, assumptions, open questions, confirmation workflow sections, or raw `用户补充描述` notes.

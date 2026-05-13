# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the workflow, mode rules, and release rules.
3. Read `references/page-release-quality-checklist.md` before finalizing output.
4. Load exactly one draft page under `product/development/pages`, using the latest versioned sibling when present unless the user explicitly selected an exact file.
5. If multiple pages or a directory are provided, stop and request one target page; do not batch release pages.
6. Determine mode first. Unless the user explicitly asks for final/release/正式版 output, use draft revision mode.
7. Apply every completed handling decision from the final `页面假设与待确认统一清单`.
8. Locate `## 12. 用户补充描述`; if it contains non-placeholder user modifications, analyze and apply them to the page content.
9. In draft revision mode, substantively regenerate the next versioned draft beside the source draft by applying user confirmations and `用户补充描述` into the page body; keep remaining/new `PA-*` / `PQ-*` workflow rows, keep analytics content, reset `用户补充描述` to an empty placeholder, and reject version-only copies.
10. In final release mode, write exactly one release page to `product/release/pages` using the same relative filename.
11. Ensure the final release document contains no `PA-*`, `PQ-*`, assumptions, open questions, confirmation workflow sections, or raw `用户补充描述` notes.

# Gemini Adapter

Use the runner-neutral workflow at `../../SKILL.md`, but this skill requires a Figma Remote MCP-compatible environment.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata and output contract.
2. Read `SKILL.md` for the Figma creation workflow.
3. Read `references/figma-target-resolution-checklist.md` before any Figma write.
4. Read `references/design2figma-quality-checklist.md` after creating the Figma design.
5. Load exactly one `product/release/design/...` page MD.
6. Parse the provided Figma link and inspect the target Figma file/page before writing.
7. If the target Figma page is ambiguous, stop and ask for clarification.
8. Create exactly one page design in Figma.
9. Do not process directories, all pages, or multiple page MD files in one execution.

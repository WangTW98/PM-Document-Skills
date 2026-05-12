# Gemini Adapter

Use the runner-neutral skill at `../../SKILL.md`.

When invoking this skill from Gemini or another Markdown-oriented agent runner:

1. Read `skill.yaml` for metadata, output paths, and compatibility rules.
2. Read `SKILL.md` for the workflow and quality bar.
3. Read these references before writing generated artifacts:
   - `references/design-md-contract.md`
   - `references/output-file-contract.md`
   - `references/preview-html-contract.md`
4. Create outputs under a slugged `design/<design-system-slug>/` directory.
5. Generate all required files: `00-index.md`, `DESIGN.md`, `visual-spec.md`, `tokens.json`, `preview.html`, `usage.md`, `handoff/html-generation-guide.md`, and `handoff/figma-remote-mcp-guide.md`.
6. Keep generated artifacts runner-neutral and usable by both Codex and Gemini.
7. In `usage.md`, include concise invocation examples for Codex and Gemini.
8. Do not assume Codex-only tools, Gemini-only tools, VS Code, Antigravity, Google Stitch, Figma, Tailwind, React, a build step, network access, CDNs, or external fonts.

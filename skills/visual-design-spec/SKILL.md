---
name: visual-design-spec
description: Generate a platform-neutral visual design system from a user's visual style description, brand direction, screenshots, product context, or UI mood references. Use when an AI agent needs to create a Google Stitch DESIGN.md-style specification, structured design tokens, a human-readable visual design guide, a standalone HTML preview, and usage instructions for later HTML page generation or Figma Remote MCP design creation while remaining compatible with Codex, Gemini, and other Markdown-oriented AI runners without depending on Codex, VS Code, Antigravity, Stitch, Figma, or any specific runtime.
---

# Visual Design Spec

## Overview

Create a complete, portable visual design specification from the user's design description. The output must be usable by humans and AI agents, and must include both machine-readable rules and a standalone preview that shows the generated visual language.

This skill is runner-neutral. Any AI system can use it by reading this file and the resources under `references/`; do not make the generated artifacts depend on a specific editor, agent, framework, design tool, or operating environment. Codex-specific and Gemini-specific adapters live under `adapters/`, but the canonical workflow remains this `SKILL.md`.

## Runner Compatibility

This skill must work in both Codex and Gemini style runners:

- Codex metadata: `adapters/codex/agents/openai.yaml`.
- Gemini instructions: `adapters/gemini/GEMINI.md`.
- Legacy Codex metadata may also exist at `agents/openai.yaml`; keep it aligned when changing prompts.
- Generated design artifacts must be usable by either runner without requiring runner-specific tools.
- Any generated `usage.md` must include a short section for Codex and a short section for Gemini, both pointing to the same canonical files and avoiding platform-specific assumptions.

## Inputs

Accept any combination of:

- User visual description, such as mood, brand adjectives, audience, industry, product type, layout density, or inspiration references.
- Existing product overview or sitemap files.
- Screenshots, mockups, brand documents, logos, color notes, or typography notes.
- Target downstream use, such as generating HTML pages, creating Figma designs through Remote MCP, or aligning multiple AI agents.

If inputs are incomplete, infer a coherent visual system and mark material assumptions in the generated `00-index.md` and `visual-spec.md`. Do not block on follow-up questions unless a missing brand constraint would make the system misleading.

## Default Output Structure

Always create generated design systems under the current project's `design/` directory unless the user provides a more specific path. The design-system directory name must be generated from the user's prompt so multiple visual systems can coexist.

Directory naming rules:

- Generate a concise lowercase slug from the user's core design intent, product name, brand name, or style direction, such as `design/finance-console-dark/`, `design/kids-learning-warm/`, or `design/saas-ops-minimal/`.
- Use only lowercase letters, digits, and hyphens.
- Keep the slug under 48 characters when possible.
- Avoid generic names such as `visual-system`, `design-system`, `new-design`, or `preview`.
- If a generated directory already exists, create a non-destructive variant such as `design/finance-console-dark-v2/` rather than overwriting user-edited files.

Default folder shape:

```text
design/<design-system-slug>/
├── 00-index.md
├── DESIGN.md
├── visual-spec.md
├── tokens.json
├── preview.html
├── usage.md
└── handoff/
    ├── html-generation-guide.md
    └── figma-remote-mcp-guide.md
```

The structure is part of the product. Keep filenames stable so later AI skills can locate artifacts without guessing.

## Workflow

1. Gather design intent.
   - Read all user-provided files and visual references when accessible.
   - Extract concrete style signals: palette, contrast, density, typography personality, component shape, image treatment, motion attitude, accessibility constraints, and platform expectations.
   - Separate confirmed facts from assumptions.

2. Define the design direction.
   - Write a short design thesis: what the interface should feel like and what it must avoid.
   - Choose values that are specific enough for implementation: hex colors, font stacks, type scale, spacing scale, radii, shadows, borders, component states, and responsive rules.
   - Use semantic token names rather than tool-specific names. Prefer `color.background.canvas`, `space.4`, `radius.card`, `component.button.primary.background` over names tied to CSS frameworks or Figma internals.
   - Define responsive behavior as first-class design rules: breakpoints, container widths, grid collapse behavior, navigation changes, typography adjustments by role, minimum touch targets, and density changes across mobile, tablet, and desktop.

3. Generate `DESIGN.md`.
   - Follow `references/design-md-contract.md`.
   - Use YAML front matter for machine-readable design tokens.
   - Use Markdown sections for rationale, rules, examples, do/don't guidance, and agent instructions.
   - Keep it portable: no Tailwind-only, React-only, Figma-only, or Codex-only requirements.

4. Generate expanded support files.
   - `visual-spec.md`: human-readable full design manual with rationale, examples, assumptions, and implementation notes.
   - `tokens.json`: JSON mirror of the key tokens from `DESIGN.md` for parsers that prefer structured data.
   - `usage.md`: explain how humans and AI agents should use the files, which file is canonical, and how to update the system. Include equivalent Codex and Gemini usage examples that read the same `DESIGN.md`, `tokens.json`, `visual-spec.md`, and preview files.
   - `handoff/html-generation-guide.md`: instructions for an AI page-generation skill to apply the visual system.
   - `handoff/figma-remote-mcp-guide.md`: instructions for an AI agent using Figma Remote MCP to create design drafts from the system.
   - All generated files must reference the actual `design/<design-system-slug>/` path chosen for the current run.

5. Generate `preview.html`.
   - Use `references/preview-html-contract.md`.
   - Make it standalone: inline CSS, no external fonts, no CDN, no build step, no framework.
   - Show swatches, color role explanations, typography hierarchy, spacing rhythm, buttons, form fields, cards, navigation, table/list patterns, empty/error/success states, accessibility notes, and at least one responsive layout sample.
   - The preview must visually demonstrate the actual chosen tokens, not generic examples.

6. Verify before finishing.
   - Confirm all output files exist.
   - Confirm the files were written under the generated `design/<design-system-slug>/` directory, not a fixed shared directory.
   - Check that `DESIGN.md`, `tokens.json`, and `preview.html` use the same token values.
   - Ensure the preview can be opened directly in a browser from the filesystem.
   - Confirm all usage instructions are platform-neutral and do not assume a specific AI agent or design tool.
   - Confirm `usage.md` includes both Codex and Gemini invocation guidance.

## Quality Bar

- Use precise values, not vague styling requests. Avoid instructions like "make it modern" unless paired with concrete implementation rules.
- Preserve the user's visual intent while making it operational for downstream generation.
- Include accessibility rules: contrast targets, focus states, touch targets, text sizing, motion reduction, and error-state visibility.
- Include responsive adaptation rules in every relevant artifact, not only in the preview: breakpoints, layout behavior, component resizing, table/list fallback, and mobile navigation.
- Include negative guidance. State what future agents must avoid so the system does not drift.
- Keep the generated system cohesive. Do not combine many unrelated aesthetic references without resolving them into one visual language.
- Make the preview useful to non-designers: it should reveal whether the system feels correct before any page is generated.
- Keep Codex and Gemini adapter instructions consistent with this file. Adapter files may explain how to invoke the skill, but must not redefine the output contract.

## Resources

- `references/design-md-contract.md`: required structure and content rules for `DESIGN.md`.
- `references/preview-html-contract.md`: required structure and quality rules for `preview.html`.
- `references/output-file-contract.md`: required contents for every generated file.
- `assets/preview-starter.html`: optional starter markup for standalone previews; adapt token names and sections to the project.

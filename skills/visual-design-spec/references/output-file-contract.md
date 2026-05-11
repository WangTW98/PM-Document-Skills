# Output File Contract

Use this reference to ensure every generated artifact has a clear purpose.

## `00-index.md`

Include:

- Design system name.
- Generated output directory, for example `design/<design-system-slug>/`.
- Source inputs used.
- Confirmed visual decisions.
- Assumptions and open points.
- File map with one-sentence purpose for each file.
- Recommended read order for humans and AI agents.

## `DESIGN.md`

Canonical portable design-system source. Follow `design-md-contract.md`.

## `visual-spec.md`

Expanded human-readable manual. Include:

- Design thesis.
- Brand personality and audience fit.
- Color system with swatches expressed as names and hex values.
- Typography scale and use cases.
- Layout grid, spacing, density, and page composition rules.
- Responsive adaptation rules for mobile, tablet, desktop, and wide desktop.
- Component specs for navigation, buttons, forms, cards, lists/tables, dialogs, alerts, tabs, and empty states.
- Accessibility requirements.
- Responsive behavior.
- Dos and don'ts.
- Assumptions and rationale.

## `tokens.json`

Machine-readable mirror of the core tokens.

Rules:

- Keep JSON valid.
- Use the same values as `DESIGN.md`.
- Avoid comments.
- Include a `meta` object with name, version, generated purpose, and canonical source path.
- Include the generated output directory and slug in `meta`.

## `preview.html`

Standalone visual preview. Follow `preview-html-contract.md`.

## `usage.md`

Explain:

- `DESIGN.md` is canonical.
- `tokens.json` is for tooling.
- `preview.html` is for visual inspection.
- How to ask an AI agent to generate pages from the system.
- How to ask an AI agent using Figma Remote MCP to create a Figma draft.
- How to update tokens safely without breaking consistency.
- How to validate whether generated UI follows the system.
- How to create an additional design system later by generating a new slugged directory under `design/`.

## `handoff/html-generation-guide.md`

Write a practical instruction file for downstream HTML/page-generation skills:

- Files to read.
- Required token application order.
- Component defaults.
- Responsive rules.
- Accessibility checks.
- Drift checks before completion.

Do not assume a specific framework. If examples are useful, write them as plain HTML/CSS concepts.

## `handoff/figma-remote-mcp-guide.md`

Write a practical instruction file for AI agents creating Figma drafts through Remote MCP:

- Files to read.
- How to map tokens to Figma variables/styles/components.
- Recommended page/frame structure.
- Required components to create.
- Naming conventions.
- Accessibility and responsive frame checks for mobile, tablet, desktop, and wide desktop frames.

Do not assume a specific Figma plugin implementation beyond generic Remote MCP capability to create and edit nodes.

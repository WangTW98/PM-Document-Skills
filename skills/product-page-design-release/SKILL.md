---
name: product-page-design-release
description: "Generate exactly one page-level release design Markdown document by combining one product/release/mock/... content mock page with one user-selected design/<design-system>/ constraint set. Output a same-name product/release/design/... Markdown file that describes complete page content, visual style, layout, responsive behavior, and AI-readable structure for downstream Figma Remote MCP generation. The output must exclude interaction execution, analytics, tracking, API contracts, backend behavior, and business logic."
---

# Product Page Design Release

## Overview

Create a complete page-level design release MD for exactly one page. The output combines:

- Confirmed page content from one `product/release/mock/...` file.
- User-selected design constraints from one top-level `design/<design-system>/` directory.

The generated file under `product/release/design` must be readable by humans and structured enough for AI agents to create a Figma design through Figma Remote MCP. It describes what appears on the page and how it should look: layout, hierarchy, typography, color roles, spacing, component styling, media treatment, responsive adaptation, and accessibility notes. It must also prove that the page has a clear layout structure with no predictable stacking, compression, clipping, layer-order, or overlap problems.

This skill is display-only. It must not include interaction execution, analytics, tracking, API contracts, backend behavior, implementation code, or business process logic.

This skill is runner-neutral. Any AI system can use it by reading this file and the references under `references/`; platform-specific metadata belongs under `adapters/`.

## Inputs And Outputs

Required inputs:

- Exactly one release mock page under `product/release/mock/*.md`.
- Exactly one selected design constraint directory under `design/<design-system>/`.

Recommended design constraint files to read from the selected directory:

- `DESIGN.md`: canonical design tokens and rules.
- `tokens.json`: machine-readable token mirror when present.
- `visual-spec.md`: expanded human-readable style guidance when present.
- `handoff/figma-remote-mcp-guide.md`: downstream Figma generation guidance when present.

Default output:

- `product/release/design/<same-relative-page-filename>.md`

Examples:

- Input mock: `product/release/mock/010-login.md`
- Selected design: `design/material-design-3/`
- Output: `product/release/design/010-login.md`

If the user provides multiple mock pages, a directory of mock pages, or multiple design systems, stop before reading page content and ask for exactly one mock page and one design system.

## Workflow

1. Select one page and one design system.
   - Use exactly one `product/release/mock/...` page as the content source.
   - Use exactly one `design/<design-system>/` directory as the design constraint source.
   - If the design system is not specified, list available directories under `design/` and ask the user to choose one.
   - Preserve the input mock filename relative to `product/release/mock` when writing under `product/release/design`.

2. Read the release mock page.
   - Extract page name, content overview, source mapping, section content, element content inventory, form/option content, list/card/table mock data, media descriptions, state-specific display copy, action visible-content mapping, and content source labels.
   - Use only user-visible display implications from action-visible-content sections, such as toast copy or modal copy. Do not describe execution actions.
   - Do not import assumptions, open questions, or draft-only confirmation content. If the release mock page still contains `MA-*`, `MQ-*`, `假设`, or `待确认`, block output and require the mock release source to be fixed first.

3. Read the selected design constraints.
   - Read `design/<design-system>/DESIGN.md` first.
   - Read `tokens.json` when present for exact machine-readable values.
   - Read `visual-spec.md` and `handoff/figma-remote-mcp-guide.md` when they clarify component styling or Figma handoff.
   - Extract concrete constraints: color tokens, type scale, font family, spacing, radii, borders, elevation/shadow, layout grid, responsive breakpoints, component defaults, icon/media treatment, and accessibility rules.
   - Do not merge multiple design systems unless the user explicitly selects a hybrid approach.

4. Create the page design release.
   - Use `references/page-design-release-template.md` as the required structure.
   - Include a human-readable design narrative explaining the page atmosphere, layout hierarchy, visual emphasis, section-by-section styling, and responsive behavior.
   - Include an AI-readable style structure with stable IDs, component hierarchy, token references, layout properties, responsive variants, and Figma-oriented construction notes.
   - Bind every major content section and visible element from the release mock page to a design treatment.
   - Use design token names and exact values from the selected design system where available.
   - When a visual decision is necessary but not explicitly covered by the design system, choose a reasonable design decision consistent with the selected system and mark it as a `设计决策` inside the release file, not as a pending assumption.
   - Define layout safety rules for every frame and major element: parent-child hierarchy, layout direction, sizing mode, min/max dimensions, padding, gap, alignment, wrapping, overflow policy, z-index/layer order, and responsive collapse behavior.
   - Add a layout integrity audit section that checks the proposed design for overlapping content, ambiguous hierarchy, compressed text, clipped media, hidden controls, uncontrolled absolute positioning, and conflicting responsive rules.
   - If a layout risk exists, resolve it in the design release before saving. Do not leave unresolved risks as notes for the downstream Figma agent.

5. Remove business-layer content.
   - Exclude API endpoints, request/response fields, database/data-processing logic, analytics event IDs, tracking details, permissions workflow, payment workflow, review workflow, and backend execution rules.
   - Exclude instructions such as "click triggers", "submit calls", "埋点", "接口", or "执行动作" unless they appear only as visible text labels in the UI.
   - Keep state display styles, but describe them as visual presentation only: loading appearance, empty-state illustration, error color usage, disabled visual style, success copy style.

6. Save and verify.
   - Ensure `product/release/design` exists.
   - Write exactly one same-name page design MD under `product/release/design`.
   - Run `references/page-design-release-quality-checklist.md` manually before finishing.
   - Do not finish if the output lacks a layout integrity audit or contains unresolved overlap, clipping, stacking, compression, or layer-order risks.

## Single-Page Rule

- Process exactly one `product/release/mock/...` page per invocation.
- Write at most one `product/release/design/...` page per invocation.
- Do not scan and convert every file in `product/release/mock`.
- If the user wants all pages converted, this skill must not perform the batch itself; use or create an orchestration skill that calls this page-level skill once per page.

## Output Requirements

The page design release must include:

- Source metadata: mock page path, selected design system path, output path, and document version.
- Human-readable design narrative.
- Page layout mindmap or Mermaid structure.
- Section-by-section layout and style table.
- Element-level visual treatment table.
- Layout integrity audit table covering hierarchy, spacing, sizing, overflow, responsive behavior, and layer order.
- Typography/color/spacing/elevation/token binding table.
- Responsive layout table for mobile, tablet, desktop, and wide desktop when applicable.
- State display style table for loading, empty, error, disabled, success, permission/limited-content, and media failure states when relevant.
- AI-readable style structure suitable for Figma Remote MCP generation.
- Figma handoff notes that describe frame hierarchy, auto-layout direction, constraints, token usage, and component grouping.

## Hard Rules

- Use `product/release/mock/...` as the only page content source.
- Use exactly one selected `design/<design-system>/` as the only design constraint source.
- Output filename must match the release mock page filename relative to `product/release/mock`.
- Do not include interaction execution, analytics, tracking, API contracts, backend behavior, implementation code, or business process logic.
- Do not include `MA-*`, `MQ-*`, assumptions, open questions, or pending confirmation sections.
- Do not invent a new design system if a selected design system exists. Apply the selected one.
- Do not use vague style descriptions alone; pair natural language with token references, exact values, layout dimensions, and structured tables.
- Do not define ambiguous layouts. Every major frame and element must have a clear parent, layout mode, sizing constraint, spacing rule, alignment rule, and responsive behavior.
- Do not rely on overlapping layers unless the page explicitly needs an overlay, modal, tooltip, badge, floating action button, or media treatment. Any intentional overlap must be named, justified, assigned a layer order, and given collision/viewport rules.
- Do not allow generated design descriptions that would cause visible elements to stack on top of each other, squeeze unreadably, clip text/media, hide controls, or break hierarchy on mobile/tablet/desktop/wide layouts.
- Do not create Figma nodes directly. This skill only writes the design release MD that later Figma Remote MCP steps can consume.

## Resources

- `references/page-design-release-template.md`: required output structure.
- `references/page-design-release-quality-checklist.md`: final verification checklist.

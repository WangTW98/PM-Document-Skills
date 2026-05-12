---
name: product-pages-design2figma
description: "Create exactly one Figma page design from one product/release/design/... page Markdown file using Figma Remote MCP. Use when the user provides one page design MD, a Figma link, and a target Figma page. The skill must parse the Figma link, inspect the target Figma file/page, verify the intended destination before writing, then create the page design into the correct Figma page. Never process multiple page MD files in one execution; this avoids context overflow, hallucination, and inaccurate Figma output."
---

# Product Pages Design2Figma

## Overview

Create a Figma design from exactly one page-level design release MD under `product/release/design`.

This skill is for the final handoff step from Markdown design specification to Figma. It consumes one page MD produced by `product-page-design-release` and writes the corresponding visual design into the user-provided Figma file and target Figma page through Figma Remote MCP. Because this is the step where layout rules become actual nodes, it must preserve and verify the MD's layout integrity requirements: clear hierarchy, stable spacing, explicit sizing, responsive behavior, overflow handling, and no unresolved stacking, compression, clipping, unintended overlap, hidden-control, or layer-order issues.

This skill must never guess the destination. It must analyze both:

- The user-provided Figma link.
- The user-specified Figma page or target node inside the Figma file.

The purpose is to create the design in the correct Figma file, correct Figma page, and correct insertion location.

## Inputs

Required inputs:

- Exactly one page design MD under `product/release/design/*.md`.
- One Figma link provided by the user.
- One target Figma page, provided as a page name, page node, selected node, or an explicit target node in the Figma link.

Optional inputs:

- Insert mode: create new top-level frame, append next to an existing frame, replace a marked frame, or create inside a section.
- Frame size preference: mobile, tablet, desktop, responsive set, or dimensions from the page design MD.
- Whether to reuse existing Figma styles/components when available.

If multiple page MD files, a directory, or "all pages" are provided, stop and ask the user to choose exactly one `product/release/design/...` file. Do not batch pages in this skill.

## Single-Page Execution Limit

This limit is mandatory and exists to avoid context overflow, hallucination, and inaccurate Figma output:

- Read exactly one `product/release/design/...` file per invocation.
- Create exactly one page design in Figma per invocation.
- Do not summarize, merge, compare, or partially process multiple page design MD files.
- Do not keep looping after one page is created.
- If the user wants all pages created in Figma, use or create an orchestration skill that calls this single-page skill once per page and records status between pages.

## Figma Link And Target Resolution

Before writing anything, resolve and verify the destination.

1. Parse the Figma link.
   - Extract `fileKey` from supported URLs such as `figma.com/design/<fileKey>/...`.
   - If the URL contains `/branch/<branchKey>/`, use the branch key according to the active Figma MCP convention.
   - Extract `node-id` when present and convert URL format like `1-2` to Figma node format `1:2`.
   - Preserve the original URL in notes for traceability.

2. Inspect the target file and page.
   - Use Figma Remote MCP metadata or Plugin API inspection to list pages and identify the linked node.
   - If the user specified a page name, match it against actual Figma page names.
   - If the Figma link points to a page node, use that page as the target page.
   - If the Figma link points to a frame, section, or other node, determine its containing Figma page and use that as the target page, unless the user explicitly requested a different page.
   - If the destination page is ambiguous, stop and ask the user to specify the target Figma page. Do not write to a default page.

3. Confirm insertion location.
   - If the user requested a specific frame/section/node, insert relative to that node according to the requested insert mode.
   - If the target page is clear but no target node is specified, create a new top-level frame on that Figma page using the required MD-derived layer name.
   - If replacing an existing frame, only replace when the user explicitly requested replacement or the target node is clearly marked for this page.

## Codex Figma MCP Notes

When using Codex with the Figma MCP tools:

- Load the `figma:figma-use` skill before every `use_figma` call.
- Prefer `get_metadata` or `get_design_context` to inspect the target node/page before writing.
- Use `use_figma` for write operations: create pages, frames, auto-layout groups, text nodes, rectangles, images/placeholders, styles, and variables.
- Do not call `generate_figma_design`; this skill creates Figma nodes from a design MD, not by capturing a web page.

## Workflow

1. Select and read one page MD.
   - Load exactly one file under `product/release/design/*.md`.
   - Verify the MD contains:
     - `文档版本 | Release`
     - Natural language style description.
     - `AI 可读样式结构`.
     - Figma Remote MCP handoff notes.
     - `布局完整性审核`.
   - If the MD contains unresolved `MA-*`, `MQ-*`, `假设`, or `待确认`, stop and require a fixed release design MD.
   - If the MD lacks layout integrity audit, or any audit item is not `通过` / `已解决`, stop and require the page design release to be regenerated or fixed before Figma creation.

2. Extract creation plan from the MD.
   - Parse page name, output source metadata, design system path, frame hierarchy, component list, token references, content-to-style bindings, responsive rules, state display styles, and Figma handoff notes.
   - Derive the required top-level Figma layer/frame name as `<md-file-name> / <page-name-from-md>`.
   - `md-file-name` is the selected page MD basename including `.md`, for example `010-login.md`.
   - `page-name-from-md` is the page name declared inside the MD, preferring the `页面名称` metadata field, then the top-level Design Release heading if metadata is unavailable.
   - Treat the `AI 可读样式结构` as the primary machine-readable build plan.
   - Treat the `布局完整性审核` and Figma handoff layout notes as mandatory constraints, not advisory notes.
   - Extract parent-child hierarchy, layout mode, sizing constraints, min/max dimensions, padding, gap, alignment, wrapping/truncation, overflow policy, clip-content behavior, scroll containers, layer order, and intentional overlay rules.
   - Use natural language style sections to resolve visual nuance and hierarchy.
   - Exclude interaction execution, analytics, API contracts, backend behavior, business process logic, and implementation code.

3. Resolve Figma destination.
   - Parse the provided Figma URL.
   - Inspect the Figma file, actual pages, and target node when available.
   - Decide the exact target page and insertion node.
   - If there is any doubt about the target page, stop before writing.

4. Create Figma structure.
   - Create a top-level frame named exactly `<md-file-name> / <page-name-from-md>`, such as `010-login.md / 登录页`.
   - Use the same naming rule for a target insertion frame when the user asks to create inside a section or append near a target node.
   - Apply frame size and responsive variants from the MD.
   - Build the hierarchy from top to bottom: page frame, layout regions, sections, groups, cards, text, media placeholders, form controls, tables/lists, and state variants when relevant.
   - Use auto layout directions, padding, gaps, alignment, constraints, and resizing rules from the MD.
   - Apply explicit min/max size, fill/hug/fixed sizing, wrapping/truncation, overflow, clip-content, scroll-axis, and layer-order rules from the MD.
   - Use absolute positioning only for explicitly intentional overlays such as modal scrims, badges, tooltips, floating actions, or specified media treatments. Preserve their documented layer order and collision/viewport rules.
   - Use token values from the MD/design system for fill, text, stroke, radius, elevation, typography, and spacing.
   - Preserve text content exactly from the release design MD unless Figma line wrapping requires visual layout adjustment.
   - When text or content would visibly collide or overflow at the target frame size, adjust the Figma layout according to the MD's responsive/overflow rules rather than shrinking text arbitrarily or allowing overlap.

5. Verify created design.
   - Re-inspect or screenshot the created target node when tooling allows.
   - Confirm the design was created in the intended file and page.
   - Confirm the top-level Figma layer/frame name exactly equals `<md-file-name> / <page-name-from-md>`.
   - Confirm the Figma node names follow the MD hierarchy.
   - Confirm visible content, layout hierarchy, token usage, and responsive variants match the MD.
   - Confirm layout integrity in the created Figma nodes: no unintended overlap, clipped text/media, compressed unreadable controls, hidden key elements, ambiguous parent-child hierarchy, incorrect layer order, or responsive variant collisions.
   - Check that Auto Layout, constraints, resizing behavior, overflow/clip settings, and wrapping/truncation match the MD.
   - If screenshots or metadata reveal layout problems, fix the Figma nodes before finalizing. Do not report success with unresolved visual layout issues.
   - Confirm no interaction execution, analytics, API, backend, or business logic nodes were created.

## Output In Figma

The Figma output should contain:

- One named top-level page frame or target insertion frame.
- The top-level Figma layer/frame must be named exactly `<md-file-name> / <page-name-from-md>`, for example `010-login.md / 登录页`.
- Section frames matching the page MD structure.
- Element nodes matching the page MD content and style definitions.
- Text nodes using confirmed display copy.
- Media placeholders or imported assets when specified.
- State display variants when the page MD includes visual states.
- Responsive variants when requested or specified in the MD.
- Clear layer names based on page, section, and element IDs.
- Layout-safe frame structure with clear parent-child hierarchy, Auto Layout, constraints, overflow handling, wrapping/truncation, and layer order matching the MD.

## Hard Rules

- Process exactly one `product/release/design/...` page MD per invocation.
- Create exactly one page design in Figma per invocation.
- Do not process directories or all pages.
- Do not write to Figma before parsing and verifying the Figma URL.
- Do not write to Figma before identifying the correct target page.
- Do not use a default page when the destination is ambiguous.
- Do not create or alter unrelated Figma pages, frames, or components.
- Do not create interaction prototypes, analytics layers, API annotations, backend diagrams, or business workflow nodes.
- Do not invent content missing from the release design MD unless it is necessary as a visual placeholder; if so, label it as a visual placeholder node.
- Do not overwrite existing Figma content unless the user explicitly requested replacement.
- Do not use an alternative top-level layer name such as only the page name, only the file name, or `<Page Name> - Design Release`. The required name is `<md-file-name> / <page-name-from-md>`.
- Do not create from a page design MD that lacks layout integrity audit or has unresolved audit items.
- Do not leave Figma output with unintended overlap, stacking, clipping, squeezed unreadable text, hidden controls, ambiguous hierarchy, or incorrect layer order.
- Do not use absolute positioning as a shortcut for normal layout. Use Auto Layout and constraints for normal structure; reserve absolute positioning for documented overlays only.

## Resources

- `references/figma-target-resolution-checklist.md`: required destination verification checklist.
- `references/design2figma-quality-checklist.md`: final Figma creation quality checklist.

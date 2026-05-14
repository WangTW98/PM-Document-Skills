---
name: product-design-draw-figma
description: Restore exactly one explicit Figma design JSON file under product/design into Figma by validating the product-md-design figma contract and recreating a single visible page frame tree. Use when an AI agent needs to draw one page from a `*-figma.json` artifact and must not batch multiple pages.
---

# Product Design Draw Figma

## Overview

Create exactly one Figma-rendered page from one explicit `*-figma.json` file under `product/design/`.

This skill is a single-page renderer.
It does not regenerate JSON.
It does not re-interpret source Markdown.
It restores the JSON artifact into Figma frames, auto-layout containers, text nodes, shapes, images, and icon nodes.

The JSON source of truth is the output contract already defined by:

- `../product-md-design/SKILL.md`
- `../product-md-design/references/figma-json-contract.md`

This skill must load `figma:figma-use` before any `use_figma` write call.

## Inputs And Outputs

Required input:

- exactly one explicit file under `product/design/**` with suffix `-figma.json`

Optional input:

- a target Figma file URL or `fileKey`
- a target page or node location inside an existing Figma file
- an explicit instruction to create a new Figma file when no target file is provided

Default behavior:

- if a target Figma file is provided, create one new top-level page frame in that file
- otherwise create a new Figma file and render one page there

Output:

- one rendered page frame tree in Figma

Optional blocker output:

- `product/design/<same-relative-dir>/<same-basename>-figma-draw-blockers.md`

## Single-Page Rule

- Never process multiple JSON files in one invocation.
- If the user asks for a directory, all pages, or multiple files, stop and ask for one explicit `*-figma.json`.
- If the user names a page conceptually but not a file, scan `product/design/` and propose matching candidates, then wait for one exact file.

## Workflow

1. Resolve exactly one source file.
   - The file must live under `product/design/`.
   - The file must end with `-figma.json`.
   - Ignore `-pencil.json`, blocker files, and status files.

2. Validate the JSON contract before drawing.
   - Read `../product-md-design/references/figma-json-contract.md`.
   - Require the top-level keys:
     - `meta`
     - `layout_match`
     - `style_source`
     - `page`
     - `state_examples`
     - `nodes`
   - Require at minimum:
     - `meta.source_md`
     - `meta.output_file`
     - `meta.page_id`
     - `meta.page_name`
     - `meta.generated_mode`
     - `layout_match.layout_file`
     - `layout_match.layout_key`
     - `layout_match.surface`
     - `layout_match.shell_type`
     - `layout_match.page_template`
     - `page.frame_name`
     - `page.frame_type`
   - If required fields are missing, do not guess. Write a blocker file.

3. Prepare the Figma target.
   - Load `figma:figma-use` first.
   - If the user provides a `fileKey`, write into that file.
   - Otherwise create a new Figma file.
   - Render exactly one page frame for this invocation.

4. Create the root page frame.
   - Use `page.frame_name` when present.
   - Fall back to `meta.page_id + meta.page_name` only when needed.
   - Restore the root as a Figma frame with explicit auto-layout semantics.
   - Preserve top-level width, height, padding, gap, alignment, and overflow intent.

5. Restore the node tree recursively.
   - Allowed source node types:
     - `frame`
     - `text`
     - `rectangle`
     - `ellipse`
     - `line`
     - `group`
     - `instance`
     - `icon`
     - `image`
   - Preserve visible order exactly.
   - For each container, map:
     - `layout.mode`
     - `layout.gap`
     - `layout.padding`
     - `layout.width`
     - `layout.height`
     - `layout.min_width`
     - `layout.min_height`
     - `layout.max_width`
     - `layout.max_height`
     - `layout.children_width`
     - `layout.children_height`
     - `layout.overflow`
     - `layout.align`
   - For each visible node, map:
     - fill
     - stroke
     - radius
     - shadow
     - size
     - text copy and typography

6. Handle icons semantically.
   - Never restore UI chrome icons as plain text.
   - Prefer an existing design-system icon component when one is explicitly available in the target file.
   - Otherwise restore from the JSON icon metadata:
     - `icon.name`
     - `icon.style`
     - `icon.size`
     - `icon.source_family`
     - `icon.stroke_weight`
     - `icon.render_preference`
     - `icon.geometry_hint`
   - If the icon family is unknown, draw the icon as vector-described geometry rather than substituting unrelated primitives.

7. Handle states conservatively.
   - Render the primary visible page tree represented by `nodes`.
   - Use `page.visible_state_set`, `states`, and `state_examples` for validation and optional labels.
   - Do not automatically expand every visible state into separate canvases unless the user explicitly asks for all states to be laid out.

8. Handle component instances safely.
   - Use `instance` only when the target file contains a resolvable component mapping.
   - If no matching component exists, do not silently swap in a different component.
   - Prefer blocking over inventing a misleading substitute.
   - If the JSON already contains enough concrete structure to draw the visible result without an unresolved instance, use that structure.

9. Write and verify.
   - Ensure exactly one page has been rendered in this invocation.
   - Verify the root frame exists.
   - Verify key title text and major sections from the JSON were created.
   - Verify major auto-layout direction, spacing, and hierarchy were preserved.
   - If the draw cannot be completed reliably, stop and write a blocker file.

## Rendering Rules

- Treat the JSON as the only rendering source of truth.
- Do not rewrite copy.
- Do not infer backend logic, actions, routes, or analytics.
- Do not re-run `product-md-design`.
- Do not batch multiple pages into one Figma rendering pass.
- Do not silently drop required visible nodes because they are inconvenient to represent.
- Do not flatten auto-layout containers into free-positioned layers when the JSON expresses container semantics explicitly.
- Do not degrade semantic icons into text glyphs or unrelated blobs.
- Do not change spacing, sizing, or column relationships unless the JSON is invalid and a blocker is required.

## Recovery Rules

- If the JSON is malformed, stop.
- If the target Figma file is not provided, create a new file when possible.
- If a node cannot be restored without changing visible semantics, write a blocker file instead of approximating.
- If a style token or component reference is missing from the target file, prefer direct local rendering that preserves appearance, but only when the visible result remains faithful to the JSON.

## Compatibility

This skill is runner-neutral at the workflow level.

Any AI runner must be able to:

- discover one explicit `-figma.json` source
- validate the same contract
- load the required Figma write workflow before mutation
- draw one page only
- either render the page or write a blocker artifact

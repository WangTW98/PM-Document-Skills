---
name: product-design-draw-pencil
description: Restore exactly one explicit Pencil design JSON file under product/design into a Pencil document or canvas by validating the product-md-design pencil contract and recreating a single visible page tree. Use when an AI agent needs to draw one page from a `*-pencil.json` artifact and must not batch multiple pages.
---

# Product Design Draw Pencil

## Overview

Create exactly one Pencil-rendered page from one explicit `*-pencil.json` file under `product/design/`.

This skill is a single-page renderer.
It does not regenerate JSON.
It does not read source Markdown under `product/release/pages/` unless needed only for debugging a blocker.
It restores the JSON artifact into Pencil-compatible nodes and writes or updates one visible page tree.

The JSON source of truth is the output contract already defined by:

- `../product-md-design/SKILL.md`
- `../product-md-design/references/pencil-json-contract.md`

## Inputs And Outputs

Required input:

- exactly one explicit file under `product/design/**` with suffix `-pencil.json`

Optional input:

- an explicit target `.pen` file path if the user wants to draw into an existing Pencil document
- an explicit instruction to create a new Pencil document when no active document should be reused

Default behavior:

- if an active Pencil document exists, add one top-level page frame into that document
- otherwise create a new Pencil document and add one top-level page frame

Output:

- one rendered page tree in Pencil

Optional blocker output:

- `product/design/<same-relative-dir>/<same-basename>-pencil-draw-blockers.md`

## Single-Page Rule

- Never process multiple JSON files in one invocation.
- If the user asks for a directory, all pages, or multiple files, stop and ask for one explicit `*-pencil.json`.
- If the user names a page conceptually but not a file, scan `product/design/` and propose matching candidates, then wait for one exact file.

## Workflow

1. Resolve exactly one source file.
   - The file must live under `product/design/`.
   - The file must end with `-pencil.json`.
   - Ignore `-figma.json`, blocker files, and status files.

2. Validate the JSON contract before drawing.
   - Read `../product-md-design/references/pencil-json-contract.md`.
   - Require the top-level keys:
     - `meta`
     - `layout_match`
     - `style_source`
     - `state_examples`
     - `document`
   - Require at minimum:
     - `meta.source_md`
     - `meta.output_file`
     - `meta.page_id`
     - `meta.page_name`
     - `meta.generated_mode`
     - `layout_match.layout_file`
     - `layout_match.layout_key`
     - `layout_match.page_template`
   - If required fields are missing, do not guess. Write a blocker file.

3. Resolve the target Pencil surface.
   - If the user explicitly names a `.pen` file, use that document.
   - Otherwise use the currently active Pencil document when available.
   - If no active document exists, create a new one.

4. Restore the page root.
   - The contract `document` node is the visible page root, not the whole `.pen` file root.
   - Create one top-level frame or screen whose name is derived from:
     - `meta.page_id`
     - `meta.page_name`
   - Preserve the root layout direction, padding, gap, size behavior, and fill.

5. Restore the node tree recursively.
   - Map contract node types only to supported Pencil node types:
     - `frame`
     - `group`
     - `rectangle`
     - `ellipse`
     - `line`
     - `polygon`
     - `text`
     - `icon_font`
     - `ref`
   - Preserve visible order exactly.
   - Preserve container layout fields including:
     - `layout`
     - `gap`
     - `padding`
     - `justifyContent`
     - `alignItems`
     - `width`
     - `height`
     - `minWidth`
     - `minHeight`
     - `clipContent`
   - Preserve visual style fields when present, such as:
     - `fill`
     - `stroke`
     - `cornerRadius`
     - text font properties

6. Handle repeated states conservatively.
   - Render the main `document` tree as the primary visible page.
   - Use `meta.visible_states` and `state_examples` only as validation and optional labeling context unless the JSON explicitly embeds multiple visible state sections in the tree.
   - Do not invent extra canvases for `loading`, `empty`, or `error` unless the user explicitly asks for all states to be laid out.

7. Handle component references safely.
   - Use `ref` only when the target Pencil document already contains the referenced reusable node and the reference is resolvable.
   - If a `ref` target cannot be resolved safely, do not silently change semantics.
   - Prefer writing a blocker file rather than replacing a component instance with an unrelated placeholder.
   - If the JSON already expands the visible subtree without needing the unresolved `ref`, use the expanded structure.

8. Write and verify.
   - Ensure exactly one page has been rendered in this invocation.
   - Verify the root frame exists.
   - Verify key title text and major sections from the JSON tree were created.
   - Verify child order and major layout direction were preserved.
   - If the draw cannot be completed reliably, stop and write a blocker file.

## Rendering Rules

- Treat the JSON as the only rendering source of truth.
- Do not rewrite copy.
- Do not infer backend logic, navigation targets, or interaction flows.
- Do not re-run `product-md-design`.
- Do not batch multiple pages into one `.pen` output.
- Do not silently drop required visible nodes because they are inconvenient to render.
- Do not degrade semantic icons into plain text glyphs when `icon_font` or a vector-capable Pencil node is expected.
- Do not change spacing, sizing, or column relationships unless the JSON is invalid and a blocker is required.

## Recovery Rules

- If the JSON is malformed, stop.
- If the target document is unavailable, create a new Pencil document when possible.
- If Pencil tool constraints make one node type impossible to represent directly, choose the closest contract-compatible Pencil node type only when the visible semantics remain intact.
- If visible semantics would change, block instead of approximating.

## Compatibility

This skill is runner-neutral.

Any AI runner must be able to:

- discover one explicit `-pencil.json` source
- validate the same contract
- draw one page only
- either render the page or write a blocker artifact

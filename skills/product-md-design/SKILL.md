---
name: product-md-design
description: Generate design-ready JSON for exactly one explicit release page Markdown file per invocation under product/release/pages by resolving the correct release layout from product/release/product-sitemap-release.md and product/release/layout, stripping all non-visual product logic, and rewriting only layout, style, visible states, representative example data, and page copy into tool-oriented Figma JSON and/or Pencil JSON under product/design. Use when an AI agent such as Codex or Gemini CLI needs a single page design source that avoids backend, interaction-flow, API, analytics, and business-positioning noise.
---

# Product MD Design

## Overview

Create exactly one design-ready page artifact set from one explicit release Markdown file under `product/release/pages`.

The skill converts a release page spec into a visual-only intermediate representation for downstream design tools. The generated artifacts must preserve page structure, layout hierarchy, visible states, representative example content, and UI copy, while removing any information that can mislead a design-generation agent into drawing non-visible business logic or backend concepts.

This skill is runner-neutral. Any AI system can use it by reading this file and the resources under `references/`. Codex- and Gemini-specific invocation hints live under `adapters/`.

## Inputs And Outputs

Required inputs:

- exactly one explicit Markdown file under `product/release/pages/**`
- `product/release/product-sitemap-release.md`
- all valid `product/release/layout/product-layout-release*.md` files

Optional input:

- one explicit design spec under `design/`, such as `design/<slug>/` or `design/<slug>/DESIGN.md`

Do not process a directory or multiple page files in one invocation.
Do not batch multiple pages even if the user references a whole module or sitemap range.
This single-page restriction is mandatory so orchestration skills can schedule pages deterministically without conflicting outputs.

Output root:

- `product/design/`

Path preservation rule:

- Preserve the source path relative to `product/release/pages/`.
- Replace the `.md` suffix with target-specific output names.
- If the user explicitly asks for `figma`, write only `-figma.json`.
- If the user explicitly asks for `pencil`, write only `-pencil.json`.
- If the user does not specify an output target, write both `-figma.json` and `-pencil.json`.

Examples:

- source: `product/release/pages/110-order-detail/index.md`
- output when target is `figma`: `product/design/110-order-detail/index-figma.json`
- output when target is `pencil`: `product/design/110-order-detail/index-pencil.json`
- output when target is omitted: both files above

- source: `product/release/pages/110-order-detail/弹窗-退款申请.md`
- output when target is `figma`: `product/design/110-order-detail/弹窗-退款申请-figma.json`
- output when target is `pencil`: `product/design/110-order-detail/弹窗-退款申请-pencil.json`
- output when target is omitted: both files above

Optional blocker output:

- `product/design/<same-relative-dir>/<same-basename>-design-blockers.md`

## Workflow

1. Select exactly one source file.
   - Use the explicit file path named by the user.
   - The input must be a single `.md` file under `product/release/pages/`.
   - If the user gives multiple files, a directory, a sitemap subset, or "all pages", stop and ask for one file.
   - Ignore status files such as `_generation-status.md`.

2. Resolve the requested output target.
   - Inspect the user's language request before generating files.
   - If the user explicitly asks for `figma`, generate only Figma JSON.
   - If the user explicitly asks for `pencil`, generate only Pencil JSON.
   - If the user explicitly asks for both, generate both.
   - If the user does not specify a target, generate both.
   - Do not infer `figma-only` or `pencil-only` merely from the current tool environment unless the user language requires it.

3. Resolve the owning page and layout family.
   - Read `product/release/product-sitemap-release.md`.
   - Resolve the source file to its owning sitemap row.
   - If the source file is the main page file, match it directly by `页面级MD文件`, page ID, or page name.
   - If the source file is an overlay or sibling file inside a page directory, resolve the owning page by its parent page directory, then reuse that page's sitemap row.
   - Read all `product/release/layout/product-layout-release*.md` files, excluding blockers or non-release files.
   - Match exactly one layout family using the sitemap row and the layout file's `Sitemap 到 Layout 映射总表`.
   - If no unique layout match exists, do not generate JSON; write a blocker file.

4. Load optional design spec.
   - If the user explicitly provides a design spec under `design/`, load it.
   - Accepted forms:
     - `design/<slug>/`
     - `design/<slug>/DESIGN.md`
   - When a design spec directory is provided, read at least:
     - `DESIGN.md`
     - `tokens.json` when present
     - `visual-spec.md` when present
   - If the user does not specify a design spec, do not block. Infer a local style direction from:
     - the matched layout family
     - the page's visible hierarchy
     - the page copy tone
     - any style wording already present in the page file
   - Spacing priority rules:
     - first, use spacing tokens, spacing scales, or explicit component spacing rules from the user-specified design spec
     - second, use spacing rules stated in the matched layout file
     - third, infer a coherent spacing system from common UI density patterns for this page type
   - When inferring spacing, prefer a small reusable scale such as `4 / 8 / 12 / 16 / 24 / 32` or an equivalent design-spec scale, rather than arbitrary one-off values.

5. Extract only design-relevant content.
   - Keep only information that changes what should be visible on screen.
   - Retain:
     - page name, page type, layout key, page template, shell category
     - visible navigation, breadcrumb, tabs, headers, cards, lists, tables, forms, dialogs, drawers, footers, banners, badges, empty states, loading states, error states, success states
     - element hierarchy, grouping, section ordering, container relationships
     - visible default copy and visible alternate-state copy
     - style clues already stated in the page doc, such as card, table, tabs, highlighted background, danger outline, fixed footer, sticky filter bar, hero summary
     - media and icon requirements when they are visible
     - layout constraints from the matched layout file, such as shell regions, page template regions, navigation placement, scroll/fixed relationships, and desktop/mobile adaptation rules
   - Remove:
     - product positioning, business goals, KPI definitions, success metrics
     - interaction logic that does not change visible structure
     - action execution steps, routing logic, navigation targets, analytics events, API contracts, data models, permissions logic, role gating logic, backend processing, validation internals, field schemas, event IDs, request/response payloads, upload timing, deduplication rules
     - implementation notes that should not appear in design output

6. Rewrite the page into a visual-only design model.
   - Build a clean intermediate model with:
     - `meta`
     - `layout_match`
     - `style_source`
     - `content_summary`
     - `visible_states`
     - `state_examples`
     - `design_nodes`
   - For every container node, explicitly encode parent-child sizing behavior so downstream Figma creation can preserve auto-layout:
     - whether the parent is `hug`, `fill`, `fixed`, or viewport-bound on each axis
     - whether children should stretch, hug, or stay fixed on each axis
     - whether the container clips overflow, allows visible overflow, or acts as a scroll viewport
   - For every section, row, card, toolbar, tab strip, form group, table area, modal body, and action area, explicitly encode spacing:
     - outer padding
     - inter-item gap
     - sectional spacing between sibling blocks
     - tighter inner spacing for related label/control pairs when appropriate
   - For every list-like or table-like structure, explicitly encode one shared column model across header and item rows:
     - consistent column order
     - consistent width behavior per column, such as `fixed`, `fill`, or proportional rules
     - consistent horizontal padding and inter-column gap
     - consistent text alignment for corresponding header and body cells
   - If a list or table has a visible header row, do not describe header widths and body widths independently unless both are clearly derived from the same column definition.
   - Spacing must be intentional and justified by either:
     - the user-specified design spec
     - the matched layout file
     - a reasonable inferred spacing scale for the page
   - Do not rely on child content text alone to imply height. Parent frames must be describable as being expanded by children without guessing.
   - For controls that visually include icons, chevrons, check marks, radios, close glyphs, password toggles, search icons, filter icons, upload icons, status icons, or QR refresh icons, output them as icon/media nodes with icon metadata, not as plain text placeholders.
   - For every icon node, make downstream rendering intent explicit:
     - semantic icon name, such as `chevron-down`, `search`, `checkbox-check`, `circle-alert`
     - target rendering family when known, such as design-spec icon set, Lucide, Material Symbols, or a local asset family
     - stroke-versus-fill intent, such as outlined chevron versus filled badge symbol
     - fallback priority, preferring vector path or known icon component before any geometric substitute
   - If the exact icon asset family is unknown, describe the icon's visible geometry precisely enough for a downstream Figma agent to draw it as a vector icon, not a placeholder blob.
   - Rewrite textual content as visible UI copy only.
   - Rewrite state information as visible variants only, such as:
     - default
     - loading
     - empty
     - success
     - warning
     - error
     - disabled
     - selected
     - active
     - sticky
     - expanded
     - collapsed
   - For every visible state described in the source page or matched layout, generate enough example content to make that state drawable without additional invention.
   - For list, table, card, queue, timeline, and feed pages:
     - do not stop at a single happy-path item if the source mentions multiple row or card states
     - include at least one representative example item per distinct visible status, tag, urgency level, progress phase, or action state that changes appearance
     - include explicit placeholder content for `loading`, `empty`, `error`, `expired`, `done`, `warning`, or similar states when the source describes them
   - If multiple business examples share the exact same visible treatment, one representative example is enough.
   - If multiple business examples produce visibly different treatment, provide separate representative examples instead of collapsing them into one.
   - If a section exists only for backend or logic explanation and has no visible manifestation, delete it.
   - If a page mentions an action such as "点击跳转", keep only the visible control itself unless the navigation changes the current screen structure.
   - If a page mentions data binding without changing visible hierarchy, keep only the displayed label/value slot and discard the data source details.

7. Generate target-appropriate tool-oriented JSON files.
   - Create Figma-oriented JSON by following `references/figma-json-contract.md` when Figma output is required.
   - Create Pencil-oriented JSON by following `references/pencil-json-contract.md` when Pencil output is required.
   - If both outputs are requested, both files must describe the same visible page, not two different interpretations.
   - The JSON must be machine-readable, deterministic, and free of Markdown fences.
   - If the source requires icons but the exact icon asset family is unknown, still emit icon nodes with semantic names such as `checkbox-check`, `select-chevron-down`, `search`, `close`, `eye`, `arrow-left`, `warning`, or `success`, instead of substituting literal text.
   - When the page contains repeatable content such as tables, cards, timelines, or list rows, encode explicit example datasets or state-specific example items instead of leaving the downstream renderer to invent them.

8. Save and verify.
   - Create the mirrored output directory under `product/design/`.
   - Write only the target-specific JSON file set for the selected source file, or one blocker file.
   - Run `references/md-design-quality-checklist.md` manually before finishing.

## Filtering Rules

- Treat the release page Markdown as a source of visual truth only after filtering.
- A retained line must answer at least one of these questions:
  - What visible region exists?
  - What visible component exists?
  - What visible copy appears?
  - What visible state changes appearance?
  - What shell or layout constraint controls placement?
  - What style clue affects hierarchy, emphasis, density, spacing, or component choice?
- If a line answers only "why the business wants this" or "how the system executes this", delete it.
- If a line mixes visual and non-visual information, keep only the visual fragment.

Read `references/content-filter-rules.md` when the source page contains mixed sections that are hard to classify.

## Layout Resolution Rules

- Never guess the layout family without checking both:
  - `product/release/product-sitemap-release.md`
  - `product/release/layout/product-layout-release*.md`
- The matched layout file controls:
  - shell type
  - global regions
  - page template
  - navigation placement
  - fixed versus scrollable containers
  - responsive adaptation expectations
- If the page Markdown disagrees with the matched release layout on a shell-level rule, prefer the more specific instruction:
  - page-local overlay, modal, drawer, or dedicated full-page behavior in the page file wins for that page
  - global shell region definitions in the layout file win for shared shell structure
- If the disagreement changes the page frame type materially and cannot be resolved confidently, block instead of guessing.

## Design Spec Rules

- If the user explicitly specifies a design spec, use it as the primary style source.
- If the design spec provides tokens, map them into both JSON outputs using semantic references whenever possible.
- If no design spec is specified:
  - infer a minimal, coherent style system for this page only
  - keep the style conservative and layout-led
  - do not invent a branding direction unrelated to the source page
- The absence of a design spec must not block output.

## Hard Constraints

- Process exactly one source Markdown file per invocation.
- Never batch the entire `product/release/pages` tree.
- Never generate JSON for more than one page in a single invocation, even if multiple pages share a layout.
- Never let this skill behave like an orchestrator; page sequencing belongs to a separate orchestration skill.
- Never write `-figma.json` when the user explicitly asked for Pencil only.
- Never write `-pencil.json` when the user explicitly asked for Figma only.
- Never output Markdown when JSON is required.
- Never keep API tables, analytics tables, event IDs, request/response shapes, or business-goal prose in the JSON.
- Never create nodes for invisible business concepts such as workflows, state machines, backend objects, or integration channels.
- Never invent extra pages, overlays, tabs, cards, metrics, or controls that are not supported by the source page or matched layout.
- Never ignore the matched release layout family.
- Never require Codex-only or Gemini-only behavior in the generated artifacts.
- Never leave a container's width/height behavior implicit when children are expected to size it.
- Never encode UI icons, checkboxes, radios, selects, or disclosure arrows as text content merely because the final asset is unknown.
- Never allow unknown UI icons to degrade into generic filled circles, generic filled rectangles, or unlabeled placeholder geometry when the visible intent is a specific control icon.
- Never leave spacing implicit on containers that visibly group, separate, or sequence content.
- Never collapse unrelated sections or controls into zero-gap stacks unless the source explicitly calls for dense adjacency.
- Never let table headers and list item columns use unrelated width, padding, gap, or alignment rules that would create visible header/body drift.
- Never describe list-like rows as free-positioned text fragments when a shared row-and-cell structure is required for alignment.
- Never output only a single default sample row or card when the source page explicitly describes additional visible states that require different visual treatment.
- Never list `loading`, `empty`, `error`, `expired`, `warning`, `done`, or similar UI states in metadata without enough example content to render them.

## Output Quality Bar

- The generated JSON target set must be sufficient for a downstream design agent to draw the page without reading the original release Markdown.
- The generated JSON target set must preserve the same visible hierarchy and region order.
- The generated JSON target set must not contain backend-only fields masquerading as UI content.
- Visual states must be represented as visible variants, not as backend flags.
- When both targets are generated, the Figma JSON and Pencil JSON must also describe equivalent example-data coverage for those visible states.
- Copy must be limited to on-screen text or short content placeholders.
- When both targets are generated, the Figma JSON and Pencil JSON must describe equivalent structure and equivalent visible states.
- Parent containers must contain enough sizing metadata for downstream Figma generation to avoid collapsed frames, clipped children, or guessed heights.
- Icon-bearing controls must contain enough icon metadata for downstream Figma generation to render graphic icons instead of text glyph placeholders.
- Figma-targeted icon nodes must contain enough source and fallback metadata for downstream generation to prefer semantic vector icons over generic geometric placeholders.
- The generated JSON target set must contain reasonable, explicit spacing values for sections, groups, and controls.
- When a design spec is provided, spacing should follow that spec's scale or tokens before any inferred values are introduced.
- List and table outputs must preserve one-to-one column correspondence between headers and item rows, with matching width behavior, spacing, and alignment semantics.
- List, table, queue, and card outputs must include enough representative sample data to cover every visible state called out by the source page and matched layout.
- Output paths must mirror the source page path under `product/design/`.

## Resources

- `references/content-filter-rules.md`: how to separate visible content from product logic
- `references/figma-json-contract.md`: required Figma JSON structure
- `references/pencil-json-contract.md`: required Pencil JSON structure
- `references/md-design-quality-checklist.md`: final verification checklist

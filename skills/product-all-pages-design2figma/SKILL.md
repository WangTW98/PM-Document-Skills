---
name: product-all-pages-design2figma
description: Create all release layout components and all release pages in a user-specified Figma file by using one explicit visual design specification created by visual-design-spec, inferring application forms from product/release/product-sitemap-release.md when the user does not specify them, reusing existing layout components when present, and then creating all release pages with strict hierarchy, nesting, sizing, and layer-order rules. Use when an AI agent needs to create the full release sitemap in Figma.
---

# Product All Pages Design2Figma

## Overview

Create the complete release Figma structure from:

- one explicit visual design specification created by `visual-design-spec`
- all release layout files under `product/release/layout`
- all release page bundles under `product/release/pages`
- one user-provided Figma link

This skill always creates layout components first, then pages.

For every sitemap row, the skill must determine one unique release layout family from:

- `product/release/product-sitemap-release.md`
- all release layout files under `product/release/layout`

and must use only that matched layout family for that row. Mixing multiple layout families in one page is forbidden.

If the user does not specify which design specification to use, stop immediately and ask for it.
If the user does not provide a Figma link, stop immediately and ask for it.

## Required Inputs

- One explicit design specification created by `visual-design-spec`
- One Figma link provided by the user
- `product/release/product-sitemap-release.md`
- all release page bundles under `product/release/pages`
- all release layout files under `product/release/layout`

Accepted design specification forms:

- `design/<design-system-slug>/DESIGN.md`
- `design/<design-system-slug>/`

The design specification is valid only when at least these files exist:

- `DESIGN.md`
- `visual-spec.md`
- `tokens.json`
- `handoff/figma-remote-mcp-guide.md`

## Immediate Stop Conditions

Stop immediately and ask the user when any of these is true:

- no design specification was explicitly provided
- no Figma link was provided
- the design specification path is missing or incomplete
- release layouts do not exist
- release pages do not exist
- sitemap inference cannot determine a valid application form for some rows

## Application Form Resolution

The user may explicitly specify one or more application forms.

If the user does not specify them:

1. Read `product/release/product-sitemap-release.md`.
2. Infer the form per release layout family and per sitemap row from:
   - `产品类型`
   - `Surface 划分`
   - row `Surface`
   - release layout file metadata such as `Layout Key`, `适用 Surface`, `适用端形态`
3. If the product contains multiple forms, keep them grouped per page row; do not force a fake single global form.
4. If any row remains ambiguous after reading sitemap and layout files, stop and ask the user.

## Release Source Contract

This skill creates Figma from release artifacts only.

Required sources:

- `product/release/product-sitemap-release.md`
- all valid `product/release/layout/product-layout-release*.md`
- all release page bundles under `product/release/pages`

Block the run if:

- release page bundles are missing for some sitemap rows
- release layout files are missing
- a release page still contains `PA-*`, `PQ-*`, `假设`, or `待确认`

## Layout Family Resolution Gate

Before any page creation, resolve a unique release layout family for every sitemap row.

Required inputs for matching:

- each page row in `Sitemap 页面生成总表`
- all valid release layout files under `product/release/layout`
- each layout file's metadata and `Sitemap 到 Layout 映射总表`

Resolution order for each row:

1. Match by exact row in a layout file's `Sitemap 到 Layout 映射总表`.
2. Validate that the matched row also agrees with:
   - `Layout Key`
   - `适用 Surface`
   - `适用端形态`
   - `页面ID`
   - canonical `页面级MD文件`
3. If more than one layout file matches the same row, mark that row blocked.
4. If no layout file matches the row, mark that row blocked.

Once matched:

- record the unique matched layout file path
- record the matched `Layout Key`
- record the matched template / shell / navigation contract

Hard constraint:

- a row may reuse only layout components that belong to its uniquely matched layout family
- do not borrow shell regions, navigation, templates, or status containers from another layout family

## Figma Destination Resolution

The user must provide a Figma link.

Supported behavior:

1. If the link points to a specific page/section/frame:
   - use that file as destination
   - use the linked page as default destination page
   - create or reuse standardized sections/pages inside it as needed
2. If the link points only to a file:
   - create or reuse:
     - `00 Layout Components`
     - `10 Release Pages`

Within the destination file:

- all layout components live under `00 Layout Components`
- all release pages live under `10 Release Pages`, unless the user explicitly requests another destination page/section

If destination parsing is ambiguous, stop before writing.

## Layout Component Creation Rule

Before any page creation, process all release layout files under `product/release/layout`.

For every valid release layout file:

1. Parse:
   - file basename
   - `Layout Key`
   - `适用 Surface`
   - `适用端形态`
   - shell regions
   - template library
   - sitemap-to-layout mapping
2. Create or reuse a layout component group in Figma.
3. Build reusable layout components before building any page frames.

Recommended Figma grouping:

- page: `00 Layout Components`
- section: `<layout-file-name> | <Layout Key>`
- component sets:
  - `Shell Regions`
  - `Page Templates`
  - `Navigation Patterns`
  - `Global Status Containers`

Reuse rule:

- if an exact stable-name compatible component group already exists, inspect and reuse it
- do not duplicate compatible existing layout component sets

## Page Naming Rule

Each created page frame must be named:

- `<logical-pages-md-filename>-<页面ID>-<页面标题>`

Important clarification:

- release page bundles use `index.md`, which is not unique
- therefore the required `logical-pages-md-filename` must be derived from the sitemap row's canonical `页面级MD文件` basename
- example:
  - sitemap `页面级MD文件`: `product/development/pages/330-order-admin-list.md`
  - final Figma frame name: `330-order-admin-list.md-M-510-订单列表`

## Status Output

Maintain a resumable status file:

- `product/release/pages/_design2figma-status.md`

The status file must record:

- `生成顺序`
- `页面ID`
- `页面名称`
- inferred or explicit application form
- source release page path
- source release layout key
- Figma file URL / file key
- created or reused layout component group names
- created page frame name
- created node ID when available
- status and blocker reason

## Workflow

1. Validate required inputs.
   - Confirm explicit design specification.
   - Confirm Figma link.
   - Confirm release layouts and release pages exist.

2. Read design specification.
   - Load `DESIGN.md`, `visual-spec.md`, `tokens.json`, and `handoff/figma-remote-mcp-guide.md`.
   - Extract token mappings, typography, spacing, radius, shadows, responsive rules, platform constraints, and Figma construction guidance.

3. Read release product sources.
   - Read `product/release/product-sitemap-release.md`.
   - Parse all rows from `Sitemap 页面生成总表`.
   - Read all release layout files.
   - Read all release page bundles referenced by the sitemap.

4. Infer or validate application forms.
   - Use user-specified form when present.
   - Otherwise infer per row / per layout family.
   - Stop if any row cannot be resolved.

5. Resolve Figma destination.
   - Parse the Figma URL.
   - Inspect the file.
   - Create or reuse standardized Figma pages/sections for layouts and pages.

6. Initialize or update status.
   - Create or refresh `product/release/pages/_design2figma-status.md`.
   - Preserve completed, blocked, skipped, and source-removed rows when safe.
   - If the Figma link changed from a previous run, do not silently mark old rows reusable; record that regeneration may be required.

7. Create or reuse all layout components.
   - Process every release layout file first.
   - Build or reuse component groups in Figma.
   - Record reuse/build results in status context.

8. Create release pages one row at a time.
   - Sort by `生成顺序`.
   - For each unfinished row:
     - resolve the unique matching release layout family from sitemap + all release layout files
     - resolve the release page bundle
     - create one page frame named `<logical-pages-md-filename>-<页面ID>-<页面标题>`
     - use the design specification plus only the matched release layout family plus release page content
     - record completion or blocker immediately

9. Verify before completion.
   - Confirm all layout component groups exist or were reused.
   - Confirm every completed row was created in the intended Figma file.
   - Confirm every page follows the required naming rule.
   - Confirm no critical layout integrity issue remains.

## Structure And Integrity Rules

These rules are mandatory for both layout components and pages:

- Create parents before children.
- Keep explicit parent-child nesting.
- Use Auto Layout for normal structure.
- Use absolute positioning only for intentional overlays or decorative layers explicitly required by release layout or release page content.
- Keep decorative layers below content; keep overlays/fixed bars above content.
- Do not allow child content to overflow non-overlay parents unless the spec explicitly requires it.
- Do not leave placeholder wrapper sizes that clip children.
- Separate scroll containers, fixed regions, and overlays structurally.
- Keep text, tables, forms, action bars, and state containers readable and not compressed.
- Make width, height, fill, hug, and fixed sizing explicit.
- Do not leave hidden controls, broken z-order, clipped content, or missing safe-area padding unresolved.

## Hard Rules

- Do not proceed without an explicit design specification created by `visual-design-spec`.
- Do not proceed without a user-provided Figma link.
- Do not create any page before all release layout files have been processed into reusable components or confirmed reusable.
- Do not create any page before its unique release layout family has been resolved from sitemap + all release layout files.
- Do not use `product/release/design/*.md` as the primary source.
- Do not create from draft pages.
- Do not infer a unique page name from `index.md`; use the sitemap canonical `页面级MD文件` basename.
- Do not duplicate an existing compatible layout component set in Figma.
- Do not mix multiple release layout families in one page.
- Do not reuse shell, template, navigation, or status components from a non-matching layout family.
- Do not silently choose a destination file or page when the Figma link is ambiguous.
- Do not silently choose an application form for a row when sitemap + layout inference is still ambiguous.
- Do not create loose, flat layers when nested containers are required.
- Do not leave width, height, or overflow behavior implicit when it affects visibility or structure.
- Do not allow stacking mistakes that cause content to hide behind backgrounds, masks, fixed bars, or overlays.
- Do not overwrite unrelated Figma content.
- Do not mark a row complete until that row's page frame passes hierarchy and layout integrity verification.

## Suggested Destination Convention

Unless the user explicitly requests another convention, use:

- Figma page: `00 Layout Components`
- Figma page: `10 Release Pages`
- layout section: `<layout-file-name> | <Layout Key>`
- page section: `<应用形态>` or `<Surface>`
- page frame: `<logical-pages-md-filename>-<页面ID>-<页面标题>`

## Resources

- `skills/visual-design-spec/SKILL.md`: upstream design specification contract
- `references/figma-remote-mcp-status-template.md`: optional status structure reference
- `references/figma-remote-mcp-orchestration-quality-checklist.md`: optional orchestration verification reference

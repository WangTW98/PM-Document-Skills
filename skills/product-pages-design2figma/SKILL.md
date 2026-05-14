---
name: product-pages-design2figma
description: Create user-selected release layout components and release pages in a user-specified Figma file by using one explicit visual design specification created by visual-design-spec, inferring or validating the application form, reusing existing layout components when present, and then creating pages from product/release/pages with strict hierarchy, parent-child nesting, and overflow-safe structure. Use when an AI agent needs to create one or more specific release pages in Figma, but not the entire sitemap.
---

# Product Pages Design2Figma

## Overview

Create Figma content from:

- one explicit visual design specification created by `visual-design-spec`
- release layout files under `product/release/layout`
- one or more selected release page bundles under `product/release/pages`
- one user-provided Figma link

This skill is not allowed to guess the visual system or the destination file.

If the user does not specify which design specification to use, stop immediately and ask for it.
If the user does not provide a Figma link, stop immediately and ask for it.

This skill first creates or reuses layout components from all release layout files, then creates the selected release pages.

For every selected page, the skill must determine one unique release layout family from:

- `product/release/product-sitemap-release.md`
- all release layout files under `product/release/layout`

and must use only that matched layout family when building that page in Figma. Mixing multiple layout families in one page is forbidden.

This skill must also prevent three common failure classes during Figma generation:

- content is not fully visible
- frames or sections overlap or clip each other unintentionally
- icons are missing, empty, or silently skipped

This skill is for selected pages only. If the user wants the entire release sitemap written to Figma, use `product-all-pages-design2figma`.

## Required Inputs

- One explicit design specification created by `visual-design-spec`
- One Figma link provided by the user
- One or more selected release pages under `product/release/pages`

Accepted design specification forms:

- `design/<design-system-slug>/DESIGN.md`
- `design/<design-system-slug>/`

The design specification is valid only when at least these files exist:

- `DESIGN.md`
- `visual-spec.md`
- `tokens.json`
- `handoff/figma-remote-mcp-guide.md`

Accepted page targeting forms:

- one or more explicit `product/release/pages/<page-key>/` directories
- one or more page IDs such as `M-510`
- one or more page names when each resolves to exactly one sitemap row

## Immediate Stop Conditions

Stop immediately and ask the user when any of these is true:

- no design specification was explicitly provided
- no Figma link was provided
- the provided design specification path does not exist or is incomplete
- no release pages were specified and the request does not clearly imply a specific subset
- the selected page set crosses multiple application forms and automatic inference cannot resolve them unambiguously

## Application Form Resolution

The skill accepts a user-described application form such as:

- responsive web
- desktop web
- admin console
- mobile web
- mini-program
- native app
- tablet app
- kiosk

Resolution order:

1. If the user explicitly specifies an application form, use it.
2. Otherwise read `product/release/product-sitemap-release.md` and infer the form from:
   - `产品类型`
   - `Surface 划分`
   - selected page rows in `Sitemap 页面生成总表`
   - selected pages' `Surface`
   - matching release layout file metadata such as `Layout Key`, `适用 Surface`, `适用端形态`
3. If the product has multiple forms, infer per selected page row rather than forcing one global form.
4. If a selected page still has multiple plausible forms after sitemap and layout analysis, stop and ask the user to choose.

For the current project, common inferred forms may include:

- `user-web`
- `admin-web`

Do not silently convert one form into another.

## Design Specification Compatibility Gate

Before any Figma write:

1. Read the selected design specification:
   - `DESIGN.md`
   - `visual-spec.md`
   - `tokens.json`
   - `handoff/figma-remote-mcp-guide.md`
2. Extract supported surfaces, device patterns, breakpoints, component rules, token mappings, and anti-drift constraints.
3. Compare the selected or inferred application form against the design specification.
4. If unsupported, stop before writing to Figma.

Do not adapt a desktop/admin visual system into a mobile-native design unless the design specification explicitly supports it.

## Release Source Contract

This skill creates Figma from release artifacts, not from design-draft MD files and not from `product/release/design/*.md`.

Required project sources:

- `product/release/product-sitemap-release.md`
- `product/release/layout/product-layout-release*.md`
- selected release page bundles under `product/release/pages`

Block the run if:

- release page bundles do not exist for the selected pages
- release layout files do not exist
- selected release page content still contains `PA-*`, `PQ-*`, `假设`, or `待确认`

## Layout Family Resolution Gate

Before creating any page in Figma, resolve its unique release layout family.

Required inputs for matching:

- the selected page row in `Sitemap 页面生成总表`
- all valid release layout files under `product/release/layout`
- each layout file's metadata and `Sitemap 到 Layout 映射总表`

Resolution order:

1. Match by exact page row in a layout file's `Sitemap 到 Layout 映射总表`.
2. Validate that the matched row also agrees with:
   - `Layout Key`
   - `适用 Surface`
   - `适用端形态`
   - `页面ID`
   - canonical `页面级MD文件`
3. If more than one layout file matches the same page, stop and report all conflicting layout files.
4. If no layout file matches the page, stop and report the page as blocked.

Once matched:

- record the unique matched layout file path
- record the matched `Layout Key`
- record the matched template / shell / navigation contract used by that page

Hard constraint:

- a page may reuse only layout components that belong to its uniquely matched layout family
- do not borrow shell regions, navigation, templates, or status containers from another layout family

## Figma Destination Resolution

The user must provide a Figma link.

Supported behavior:

1. If the link points to a specific Figma page, section, or frame:
   - use that file as the destination file
   - use the linked page as the default destination page
   - create or reuse standardized sections/pages inside it when needed
2. If the link points only to a Figma file:
   - use that file as the destination file
   - create or reuse two standardized Figma pages:
     - `00 Layout Components`
     - `10 Release Pages`

Within the destination file:

- all layout components must live under `00 Layout Components`
- all page frames must live under `10 Release Pages`, unless the user explicitly asked for another page/section in the provided link

If destination parsing is ambiguous, stop before writing.

## Visibility Integrity Gate

Before any page node creation, verify that the page can be built with a complete visible structure.

Mandatory checks:

- every section, card, list, table, form, tab group, action bar, drawer, and modal must have an explicit parent container
- every non-leaf parent container must define:
  - layout mode
  - width behavior: fixed / fill / hug
  - height behavior: fixed / fill / hug
  - overflow or clip policy
- every scroll container must define its scroll axis and visible viewport relationship
- every fixed header, fixed footer, bottom tab, sticky filter bar, or floating action area must define the inset it consumes from scrollable content
- every decorative background layer must be explicitly marked decorative and placed below interactive content

Stop before writing if any of the above is unresolved.

## Icon Resolution Gate

Before any icon node is created, resolve its source.

Allowed icon sources:

- an icon component or icon set explicitly defined by the selected visual design specification
- an icon component already present in the target Figma file and clearly compatible with the selected design specification
- an explicit placeholder icon policy defined by this skill

Rules:

- do not silently skip icons
- do not leave an empty frame in place of an icon without marking it
- every icon node must have a semantic name and a resolved source
- if a required icon cannot be resolved, create a placeholder node named `ICON-MISSING-<semantic-name>` and mark the page as not complete until it is repaired or explicitly accepted by the user

Placeholder behavior:

- placeholder icons must preserve the intended size and position of the missing icon
- placeholder icons must remain visibly distinct from production icons
- placeholder icons must be recorded in verification output and batch status

## Layout Component Creation Rule

Before creating any page frame, process all release layout files under `product/release/layout`.

For every valid release layout file:

1. Parse:
   - file basename
   - `Layout Key`
   - `适用 Surface`
   - `适用端形态`
   - shell regions
   - template library
   - sitemap-to-layout mapping table
2. Create or reuse a layout component group in Figma.
3. Build the layout content as reusable components, not ad hoc page-only frames.

Recommended Figma grouping:

- page: `00 Layout Components`
- section: `<layout-file-name> | <Layout Key>`
- component sets:
  - `Shell Regions`
  - `Page Templates`
  - `Navigation Patterns`
  - `Global Status Containers`

Reuse rule:

- if a component group with the exact stable name already exists in the target Figma file, inspect and reuse it
- do not duplicate an existing compatible layout component set
- only replace or rebuild when the user explicitly requests regeneration or when the existing component set is clearly incompatible with the current release layout file

## Build Order Contract

Every page must be built in this order:

1. page root frame
2. safe area / shell frame
3. fixed navigation and fixed action regions
4. main scroll container
5. section containers
6. section content blocks
7. text and media nodes
8. icons
9. overlays such as drawer, modal, tooltip, badge, or floating action
10. post-write verification and repair

Hard rules:

- do not insert icons before their parent component size is stable
- do not create overlays before the normal content structure is complete
- do not create fine-grained leaf nodes before the parent layout behavior is explicit

## Page Naming Rule

Each created page frame must be named:

- `<logical-pages-md-filename>-<页面ID>-<页面标题>`

Important clarification:

- release page bundles use `index.md`, which is not unique
- therefore the required `logical-pages-md-filename` must be derived from the sitemap row's canonical `页面级MD文件` basename
- example:
  - sitemap `页面级MD文件`: `product/development/pages/330-order-admin-list.md`
  - logical pages md filename: `330-order-admin-list.md`
  - page ID: `M-510`
  - page title: `订单列表`
  - final Figma frame name: `330-order-admin-list.md-M-510-订单列表`

Do not use `index.md` as the page frame prefix.

## Workflow

1. Validate required inputs.
   - Confirm the user explicitly specified a design specification.
   - Confirm the user provided a Figma link.
   - Confirm the requested release pages are explicit and resolvable.
   - If any required input is missing, stop and ask.

2. Read the design specification.
   - Load `DESIGN.md`, `visual-spec.md`, `tokens.json`, and `handoff/figma-remote-mcp-guide.md`.
   - Extract token mappings, typography, spacing, radius, shadows, responsive rules, platform constraints, and Figma construction guidance.

3. Read product release sources.
   - Read `product/release/product-sitemap-release.md`.
   - Resolve selected sitemap rows from user-specified page IDs / names / release page paths.
   - Read all release layout files under `product/release/layout`.
   - Read the selected release page bundles under `product/release/pages`.

4. Determine application form.
   - Use the user-specified form when present.
   - Otherwise infer from sitemap + layout metadata.
   - If unresolved, stop.

5. Resolve Figma destination.
   - Parse the Figma URL into file key and optional page/node context.
   - Inspect the target file.
   - Create or reuse `00 Layout Components` and `10 Release Pages` when the link does not already anchor the exact destination page/section.

6. Create or reuse layout components first.
   - Process every release layout file.
   - Build layout shell, navigation, template, and status components using the design specification tokens and layout structure.
   - Reuse exact-name compatible component groups when they already exist.

7. Create the selected release pages.
   - For each selected release page:
     - resolve its unique matching release layout family from sitemap + all release layout files
     - reuse only the corresponding layout components created in step 6 for that matched family
     - create one top-level frame named `<logical-pages-md-filename>-<页面ID>-<页面标题>`
     - build the page using release page content plus release layout rules plus visual design specification
     - follow the Build Order Contract strictly
     - resolve every required icon through the Icon Resolution Gate before marking the page complete
     - do not end the page step until the Visibility Integrity Gate and post-write repair loop both pass

8. Verify before completion.
   - Confirm layout components exist or were reused.
   - Confirm pages were created under the intended Figma file and destination page/section.
   - Confirm page names follow the required naming rule.
   - Confirm no critical layout integrity issue remains.
   - Confirm no required icon is missing unless a visible `ICON-MISSING-*` placeholder was intentionally created and the page is therefore still marked incomplete.

## Post-Write Verification And Repair Loop

After each page write, run a mandatory verification loop.

Critical checks:

- no normal content section overlaps another normal content section unintentionally
- no child node extends beyond a non-overlay parent in a way that clips visible content
- no text node is visibly truncated because of an accidental fixed height or fixed width
- no fixed header, fixed footer, bottom tab, or floating action obscures main content without corresponding inset
- no modal, drawer, tooltip, or floating layer is hidden behind a background or normal content layer
- no key CTA, table header, form control, nav item, or state banner is clipped or obscured
- no icon node is missing, empty, or unresolved without an explicit placeholder
- no wrapper frame uses a placeholder size that causes clipping, especially `100x100`-style accidental wrappers

If any critical issue exists:

- repair the structure first, not just the leaf node
- prefer resizing or changing parent layout behavior over shrinking content arbitrarily
- do not report success until all critical issues are resolved

## Structure And Integrity Rules

These rules are mandatory for both layout components and pages:

- Create parent nodes before children.
- Use explicit parent-child nesting; do not leave normal content as loose siblings when it belongs inside a container.
- Use Auto Layout for normal layout structure.
- Reserve absolute positioning only for intentional overlays or decorative layers explicitly required by the release page or layout spec.
- Keep decorative backgrounds below interactive content in the layer stack.
- Keep fixed bars, nav, drawers, and modals above normal content in the intended order.
- Ensure no child visually overflows a non-overlay parent unless the spec explicitly requires it.
- Ensure no wrapper frame has placeholder width/height that clips a larger child.
- Ensure scroll containers, fixed regions, and overlay layers are structurally separate.
- Ensure text, cards, tables, forms, and action bars remain readable and not compressed.
- Ensure width/height sizing uses explicit fixed / fill / hug logic rather than accidental defaults.
- Ensure no page frame is left with unresolved overlap, hidden controls, clipped text, missing safe area, or broken z-order.
- Ensure no required icon is missing, invisible, or replaced by an unlabeled empty frame.

## Hard Rules

- Do not proceed without an explicit design specification created by `visual-design-spec`.
- Do not proceed without a user-provided Figma link.
- Do not create pages before layout components are created or reused.
- Do not create a page before its unique release layout family has been resolved from sitemap + all release layout files.
- Do not use `product/release/design/*.md` as the primary source.
- Do not create from draft pages.
- Do not infer a unique page name from `index.md`; use the sitemap canonical `页面级MD文件` basename.
- Do not duplicate an existing compatible layout component set in Figma.
- Do not mix multiple release layout families in one page.
- Do not reuse shell, template, navigation, or status components from a non-matching layout family.
- Do not silently choose a destination file or page when the Figma link is ambiguous.
- Do not silently choose an application form when the selected pages map to multiple unresolved forms.
- Do not create loose, flat layers when a nested container is required.
- Do not leave width, height, or overflow behavior implicit when it affects visibility or structure.
- Do not allow stacking mistakes that cause content to be hidden behind backgrounds, masks, fixed bars, or overlays.
- Do not leave unresolved clipping, truncation, or overlap issues at completion time.
- Do not silently drop icons or replace them with unlabeled empty frames.
- Do not overwrite unrelated Figma content.

## Suggested Destination Convention

Unless the user explicitly requests another convention, use:

- Figma page: `00 Layout Components`
- Figma page: `10 Release Pages`
- section under layouts: `<layout-file-name> | <Layout Key>`
- section under pages: `<应用形态>` or `<Surface>`
- top-level page frame: `<logical-pages-md-filename>-<页面ID>-<页面标题>`

## Resources

- `skills/visual-design-spec/SKILL.md`: upstream design specification contract
- `references/figma-target-resolution-checklist.md`: destination verification helper
- `references/design2figma-quality-checklist.md`: final quality verification helper

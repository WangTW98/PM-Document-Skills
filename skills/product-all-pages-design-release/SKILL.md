---
name: product-all-pages-design-release
description: "Orchestrate generation of all page-level release design Markdown documents by reading product/release/product-sitemap-release.md, parsing the Sitemap 页面生成总表, maintaining product/release/design/_generation-status.md, selecting exactly one unfinished page at a time, following product-page-design-release single-page rules with one selected design/<design-system>/ constraint set, marking completion or blockage, and continuing page-by-page until all sitemap rows are complete."
---

# Product All Pages Design Release

## Overview

Generate page-level design release documents for every page listed in `product/release/product-sitemap-release.md` -> `Sitemap 页面生成总表`, while preserving the accuracy benefits of `product-page-design-release`: only one page design document is generated at a time, then status is written before moving to the next row.

This is a runner-neutral orchestration skill. It does not replace `product-page-design-release`; it uses that skill's rules as the page-design contract for each row.

Each generated design file combines:

- One confirmed content/style source page under `product/release/mock/...`.
- One user-selected design constraint directory under `design/<design-system>/`.

The generated files under `product/release/design` are display-only design handoff documents for downstream Figma Remote MCP generation. They must not include interaction execution, analytics, tracking, API contracts, backend behavior, implementation code, or business process logic. They must also pass layout integrity review: clear hierarchy, stable spacing, explicit sizing, responsive behavior, overflow handling, and no unresolved stacking, compression, clipping, overlap, hidden-control, or layer-order risks.

## Inputs And Outputs

Required inputs:

- `product/release/product-sitemap-release.md`
- Release mock page files under `product/release/mock`
- Exactly one selected design constraint directory under `design/<design-system>/`

Required status output:

- `product/release/design/_generation-status.md`

Generated design outputs:

- `product/release/design/<same-relative-page-filename>.md`, corresponding to each row's release mock page file.

## Workflow

1. Load orchestration context.
   - Read `product/release/product-sitemap-release.md`.
   - Locate and parse `Sitemap 页面生成总表`.
   - Extract each row's `生成顺序`, `页面ID`, `父页面ID`, `层级`, `页面名称`, and `页面级MD文件`.
   - Use `页面级MD文件` only as a page filename / relative filename key from the sitemap.
   - The single-page content source must be `product/release/mock/<same-relative-page-filename>.md`.
   - The single selected design constraint source must be one `design/<design-system>/` directory.
   - Derive the design output path by preserving the same relative filename under `product/release/design`.
   - Sort rows by numeric `生成顺序`.

2. Resolve selected design system.
   - If the user already selected a design system, record its directory path in the status overview.
   - If no design system is selected, list available directories under `design/` and ask the user to choose exactly one before generating pages.
   - Do not merge multiple design systems during this orchestration.
   - If the selected design system lacks `DESIGN.md`, block generation until the design system is fixed.

3. Initialize or update status.
   - Ensure `product/release/design` exists.
   - Create `product/release/design/_generation-status.md` if missing, using `references/design-release-generation-status-template.md`.
   - If status exists, preserve completed, blocked, skipped, and source-removed rows.
   - Reconcile status with the current sitemap table: add new rows, mark missing sitemap rows as `源表已移除`, and keep existing completion records.
   - If the selected design system changes, do not silently overwrite completed design pages. Mark them as needing regeneration or ask for explicit regeneration confirmation.

4. Select one unfinished page.
   - Choose the first row by `生成顺序` whose status is not `已完成`, `已跳过`, or `已阻塞`.
   - If its design output file already exists and passes the current `product-page-design-release` checklist for the selected design system, including App Shell / Navigation Contract and layout integrity audit requirements, mark it `已完成` instead of regenerating.
   - If its design output file already exists but lacks an App Shell / Navigation Contract, lacks a layout integrity audit, omits required product-level navigation, overuses absolute positioning for normal content, or contains unresolved overlap, clipping, compression, hidden-control, ambiguous hierarchy, or layer-order risks, mark it `需重新生成` or regenerate only with explicit user permission.
   - If the `product/release/mock/...` source file does not exist, mark it `已阻塞` and record that the required release mock source is missing.
   - If the `product/release/mock/...` source file contains `MA-*`, `MQ-*`, `假设`, or `待确认`, mark it `已阻塞` and record that the release mock source must be fixed first.
   - If required sitemap fields are missing, especially `页面ID`, `页面名称`, or `页面级MD文件`, mark it `已阻塞`.

5. Generate exactly one page design release.
   - For the selected row, follow `skills/product-page-design-release/SKILL.md`.
   - Use only the selected `product/release/mock/...` file as the single page content source.
   - Use only the selected `design/<design-system>/` directory as the design constraint source.
   - Write the design release page to the derived `product/release/design` path.
   - Do not generate any other page design file in the same sub-step.
   - Do not mark generation successful unless the page design release includes a completed layout integrity audit and resolves all layout risks before saving.

6. Write completion status.
   - Update `_generation-status.md` immediately after the page design generation attempt.
   - Record status, release mock source path, design output path, selected design system path, generation timestamp, source page ID, page name, and any blockers.
   - If generation failed or source validation failed, record `已阻塞` with the concrete reason and do not mark it complete.

7. Continue until done.
   - After each status update, select the next unfinished row and repeat steps 4-6.
   - Continue only while context remains reliable. If the session is becoming long, stop after the current page and leave `_generation-status.md` accurate so the next invocation can resume safely.
   - When all rows are complete, write a final summary in `_generation-status.md`.

## Status Values

Use only these statuses:

- `待生成`: row exists in sitemap and no page design release has been generated.
- `生成中`: currently selected row.
- `已完成`: design release page exists and passes basic checklist for the selected design system.
- `已阻塞`: cannot generate because the release mock source is missing, design system is missing, source content is unresolved, or generation failed.
- `已跳过`: user explicitly skipped the page.
- `源表已移除`: page existed in status but no longer exists in current sitemap.
- `需重新生成`: page was completed with a different design system or stale design constraints and needs explicit regeneration.

## Product Page Design Release Contract

For each selected page, obey these `product-page-design-release` rules:

- Process exactly one release mock page per selected sitemap row.
- Input content source is always `product/release/mock/<same-relative-page-filename>.md`.
- The sitemap's `页面级MD文件` is only a filename key used to locate the same page under `product/release/mock`.
- Input design source is exactly one selected `design/<design-system>/` directory.
- Output path preserves the release mock filename relative to `product/release/mock`, under `product/release/design`.
- Generate a complete page content + style design release MD containing both natural language style description and AI-readable style structure.
- Include Figma Remote MCP handoff notes for frame hierarchy, auto-layout, token usage, component grouping, and responsive variants.
- Include an App Shell / Navigation Contract derived from `product/release/product-sitemap-release.md`, including top navigation, bottom tab navigation, fixed footer/bottom actions, main scroll region, safe-area behavior, and explicit exceptions.
- Include and pass layout integrity audit for clear hierarchy, parent-child structure, Auto Layout suitability, sizing constraints, padding/gap, alignment, overflow, wrapping/truncation, responsive behavior, and layer order.
- Resolve any predictable stacking, squeezing, clipping, hidden-control, unintended overlap, or layer-order issue inside the page design release before marking the row complete.
- Keep the design release display-only: no interaction execution, analytics/tracking, API definitions, request/response structures, backend behavior, business process logic, or implementation code.
- Write exactly one design release page or one blocker file for the selected page.

## Hard Rules

- Do not batch multiple pages into one design release file.
- Do not skip status updates.
- Do not overwrite completed design release files unless the user explicitly asks to regenerate.
- Do not invent missing `product/release/mock/...` source paths. Block the row instead.
- Do not invent or auto-select a design system when multiple `design/` directories exist. Ask the user to choose exactly one.
- Do not alter `product/release/product-sitemap-release.md`.
- Do not alter source files under `product/release/mock` or `design/<design-system>` while generating design releases.
- Do not mark a page complete unless the design release file exists at the expected `product/release/design` path.
- Do not mark a page complete if the design release page lacks AI-readable style structure or Figma Remote MCP handoff notes.
- Do not mark a page complete if the design release page lacks App Shell / Navigation Contract or omits required global navigation from the product overview.
- Do not mark a page complete if the design release page lacks a layout integrity audit or contains unresolved layout risks.
- Do not mark a page complete if the page design would predictably cause elements to stack, squeeze, clip, overlap unintentionally, hide controls, or render with incorrect layer order across mobile/tablet/desktop/wide layouts.
- Do not mark a page complete if the design release page contains interaction execution, analytics, API contracts, backend behavior, business process logic, implementation code, `MA-*`, `MQ-*`, `假设`, or `待确认`.

## Resources

- `references/design-release-generation-status-template.md`: status file structure.
- `references/design-release-orchestration-quality-checklist.md`: final orchestration checks.

---
name: product-all-pages-draft
description: Orchestrate generation of all page draft bundles from product/release/product-sitemap-release.md by parsing the Sitemap 页面生成总表, resolving the correct release layout file for each row, maintaining product/development/pages/_generation-status.md, selecting exactly one unfinished sitemap row at a time, following product-page-draft single-page-bundle rules for that row, marking completion or blockage, and continuing row-by-row until all sitemap rows are complete.
---

# Product All Pages Draft

## Overview

Generate every draft page bundle listed in `product/release/product-sitemap-release.md` -> `Sitemap 页面生成总表`, while preserving the accuracy benefits of `product-page-draft`: only one sitemap row is processed at a time, then status is written before moving to the next row.

This is a runner-neutral orchestration skill. It does not replace `product-page-draft`; it uses that skill's rules as the page-bundle generation contract for each row.

## Inputs And Outputs

Required input:

- `product/release/product-sitemap-release.md`

Required status output:

- `product/development/pages/_generation-status.md`

Generated bundle outputs per row:

- bundle root derived from the row's `页面级MD文件`
- main file `index.md`
- zero or more overlay sub-page docs in the same directory

## Workflow

1. Load orchestration context.
   - Read `product/release/product-sitemap-release.md`.
   - Locate and parse `Sitemap 页面生成总表`.
   - Extract each row's `生成顺序`, `页面ID`, `父页面ID`, `层级`, `Surface`, `页面名称`, and `页面级MD文件`.
   - Sort rows by numeric `生成顺序`.

2. Initialize or update status.
   - Ensure `product/development/pages` exists.
   - Create `product/development/pages/_generation-status.md` if missing, using `references/generation-status-template.md`.
   - If status exists, preserve completed, blocked, and skipped rows.
   - Reconcile status with the current sitemap table: add new rows, mark missing sitemap rows as `源表已移除`, and keep existing completion records.

3. Select one unfinished page row.
   - Choose the first row by `生成顺序` whose status is not `已完成`, `已跳过`, or `已阻塞`.
   - Derive the bundle root by stripping `.md` from `页面级MD文件`.
   - If the bundle main file `index.md` already exists and passes the current `product-page-draft` checklist, including the mandatory analytics section, `EVT-*` event IDs, final `用户补充描述` section, and any expected overlay sub-page docs, mark it `已完成` instead of regenerating.
   - If the bundle exists but lacks `## 7. 埋点事件统计设计`, lacks `埋点事件ID`, lacks any `EVT-*` event ID, lacks `用户补充描述`, or is missing expected overlay docs, treat it as stale and regenerate it unless the user explicitly forbids regeneration.
   - If required fields are missing, especially `页面ID`, `页面名称`, or `页面级MD文件`, mark it `已阻塞`.

4. Generate exactly one page bundle.
   - For the selected row, follow `skills/product-page-draft/SKILL.md`.
   - Resolve the row's matching release layout file before generation. If no unique match exists, mark the row `已阻塞`.
   - Use the selected row as the target page.
   - Write the page bundle to the directory derived from the row's `页面级MD文件`.
   - Do not generate any other sitemap row in the same sub-step.

5. Write completion status.
   - Update `_generation-status.md` immediately after the page bundle is generated.
   - Record status, bundle root, main file path, generated overlay doc list, generation timestamp, source page ID, page name, assumptions count, confirmation questions count, and any blockers.
   - If generation failed, record `已阻塞` with the concrete reason and do not mark it complete.

6. Continue until done.
   - After each status update, select the next unfinished row and repeat steps 3-5.
   - Continue only while context remains reliable. If the session is becoming long, stop after the current row and leave `_generation-status.md` accurate so the next invocation can resume safely.
   - When all rows are complete, write a final summary in `_generation-status.md`.

## Status Values

Use only these statuses:

- `待生成`: row exists in sitemap and no draft page bundle has been generated
- `生成中`: currently selected row
- `已完成`: page bundle exists and passes basic checklist
- `已阻塞`: cannot generate because required source data is missing or generation failed
- `已跳过`: user explicitly skipped the row
- `源表已移除`: page existed in status but no longer exists in current sitemap

## Product Page Draft Contract

For each selected row, obey these `product-page-draft` rules:

- Generate exactly one sitemap-row page bundle.
- Output bundle root is derived from the row's `页面级MD文件`.
- Main output file is `index.md`, not the raw `页面级MD文件` itself.
- Include Mermaid plus detailed tables.
- Include page elements, states, triggers, style definitions, interactions, actions, edge cases, APIs, request/response data structures, media/resources, and page-level `PA-*` / `PQ-*` assumptions when needed.
- Include the mandatory analytics event contract from `product-page-draft`: Action table columns `埋点事件ID`, `埋点事件名称`, `不埋点原因`, plus `## 7. 埋点事件统计设计` with at least one `EVT-*` page exposure event.
- Resolve the correct release layout file for the selected row; do not reuse a sibling page's layout file blindly.
- End every generated main draft with `## 12. 用户补充描述`.
- Keep all page-level uncertainty inside that page bundle's main `index.md`.
- Generate auxiliary overlay docs only when the current row genuinely contains overlay sub-pages that do not already exist as separate sitemap rows.

## Hard Rules

- Do not batch multiple sitemap rows into one MD file.
- Do not skip status updates.
- Do not overwrite completed page bundles unless the user explicitly asks to regenerate.
- Do not invent missing `页面级MD文件` values. Block the row instead.
- Do not alter `product/release/product-sitemap-release.md`.
- Do not mark a row complete unless the main file exists at the derived bundle path.
- Do not mark a row complete if it lacks the mandatory analytics section, `EVT-*` event IDs, `用户补充描述`, or required overlay docs.
- Do not mark a row complete if its layout-file match is missing or ambiguous.

## Resources

- `references/generation-status-template.md`: status file structure.
- `references/orchestration-quality-checklist.md`: final orchestration checks.

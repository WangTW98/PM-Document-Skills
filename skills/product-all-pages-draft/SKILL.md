---
name: product-all-pages-draft
description: Orchestrate generation of all page-level draft Markdown documents from product/release/product-overview-release.md by parsing the Sitemap 页面生成总表, maintaining product/development/pages/_generation-status.md, selecting exactly one unfinished page at a time, following product-page-draft single-page rules for that page, marking completion or blockage, and continuing page-by-page until all sitemap rows are complete.
---

# Product All Pages Draft

## Overview

Generate every page draft listed in `product/release/product-overview-release.md` -> `Sitemap 页面生成总表`, while preserving the accuracy benefits of `product-page-draft`: only one sitemap row is processed at a time, then status is written before moving to the next row.

This is a runner-neutral orchestration skill. It does not replace `product-page-draft`; it uses that skill's rules as the page-generation contract for each row.

## Inputs And Outputs

Required input:

- `product/release/product-overview-release.md`

Required status output:

- `product/development/pages/_generation-status.md`

Generated page outputs:

- The exact `页面级MD文件` path from each row in `Sitemap 页面生成总表`.

## Workflow

1. Load orchestration context.
   - Read `product/release/product-overview-release.md`.
   - Locate and parse `Sitemap 页面生成总表`.
   - Extract each row's `生成顺序`, `页面ID`, `父页面ID`, `层级`, `页面名称`, and `页面级MD文件`.
   - Sort rows by numeric `生成顺序`.

2. Initialize or update status.
   - Ensure `product/development/pages` exists.
   - Create `product/development/pages/_generation-status.md` if missing, using `references/generation-status-template.md`.
   - If status exists, preserve completed, blocked, and skipped rows.
   - Reconcile status with the current sitemap table: add new rows, mark missing sitemap rows as `源表已移除`, and keep existing completion records.

3. Select one unfinished page.
   - Choose the first row by `生成顺序` whose status is not `已完成`, `已跳过`, or `已阻塞`.
   - If its output file already exists and passes the current `product-page-draft` checklist, including the mandatory analytics section and `EVT-*` event IDs, mark it `已完成` instead of regenerating.
   - If its output file exists but lacks `## 7. 埋点事件统计设计`, lacks `埋点事件ID`, or lacks any `EVT-*` event ID, treat it as stale and regenerate it unless the user explicitly forbids regeneration.
   - If required fields are missing, especially `页面ID`, `页面名称`, or `页面级MD文件`, mark it `已阻塞`.

4. Generate exactly one page.
   - For the selected row, follow `skills/product-page-draft/SKILL.md`.
   - Use the selected row as the target page.
   - Write the page MD to the exact `页面级MD文件` path from the row.
   - Do not generate any other page in the same sub-step.

5. Write completion status.
   - Update `_generation-status.md` immediately after the page is generated.
   - Record status, output path, generation timestamp, source page ID, page name, assumptions count, confirmation questions count, and any blockers.
   - If generation failed, record `已阻塞` with the concrete reason and do not mark it complete.

6. Continue until done.
   - After each status update, select the next unfinished row and repeat steps 3-5.
   - Continue only while context remains reliable. If the session is becoming long, stop after the current page and leave `_generation-status.md` accurate so the next invocation can resume safely.
   - When all rows are complete, write a final summary in `_generation-status.md`.

## Status Values

Use only these statuses:

- `待生成`: row exists in sitemap and no page draft has been generated.
- `生成中`: currently selected row.
- `已完成`: page draft exists and passes basic checklist.
- `已阻塞`: cannot generate because required source data is missing or generation failed.
- `已跳过`: user explicitly skipped the page.
- `源表已移除`: page existed in status but no longer exists in current sitemap.

## Product Page Draft Contract

For each selected page, obey these `product-page-draft` rules:

- Generate exactly one page per selected sitemap row.
- Output path must exactly match the row's `页面级MD文件`.
- Include Mermaid plus detailed tables.
- Include page elements, states, triggers, style definitions, interactions, actions, edge cases, APIs, request/response data structures, media/resources, and page-level `PA-*` / `PQ-*` assumptions when needed.
- Include the mandatory analytics event contract from `product-page-draft`: Action table columns `埋点事件ID`, `埋点事件名称`, `不埋点原因`, plus `## 7. 埋点事件统计设计` with at least one `EVT-*` page exposure event.
- Keep all page-level uncertainty inside that page file's final `页面假设与待确认统一清单`.

## Hard Rules

- Do not batch multiple pages into one MD file.
- Do not skip status updates.
- Do not overwrite completed page MD files unless the user explicitly asks to regenerate.
- Do not invent missing `页面级MD文件` paths. Block the row instead.
- Do not alter `product/release/product-overview-release.md`.
- Do not mark a page complete unless the page file exists at the exact sitemap path.
- Do not mark a page complete if it lacks the mandatory analytics section or `EVT-*` event IDs.

## Resources

- `references/generation-status-template.md`: status file structure.
- `references/orchestration-quality-checklist.md`: final orchestration checks.

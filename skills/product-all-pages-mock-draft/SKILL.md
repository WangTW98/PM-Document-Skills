---
name: product-all-pages-mock-draft
description: Orchestrate generation of all content-only mock draft Markdown documents from release pages by reading product/release/product-overview-release.md, parsing the Sitemap 页面生成总表, maintaining product/development/mock/_generation-status.md, selecting exactly one unfinished page at a time, following product-page-mock-draft single-page rules for that page, marking completion or blockage, and continuing page-by-page until all sitemap rows are complete.
---

# Product All Pages Mock Draft

## Overview

Generate mock content drafts for every page listed in `product/release/product-overview-release.md` -> `Sitemap 页面生成总表`, while preserving the accuracy benefits of `product-page-mock-draft`: only one release page is converted to mock content at a time, then status is written before moving to the next row.

This is a runner-neutral orchestration skill. It does not replace `product-page-mock-draft`; it uses that skill's rules as the mock-generation contract for each page.

## Inputs And Outputs

Required inputs:

- `product/release/product-overview-release.md`
- Release page files under `product/release/pages`

Required status output:

- `product/development/mock/_generation-status.md`

Generated mock outputs:

- `product/development/mock/<same-relative-page-filename>.md`, corresponding to each row's release page file.

## Workflow

1. Load orchestration context.
   - Read `product/release/product-overview-release.md`.
   - Locate and parse `Sitemap 页面生成总表`.
   - Extract each row's `生成顺序`, `页面ID`, `父页面ID`, `层级`, `页面名称`, and `页面级MD文件`.
   - Treat `页面级MD文件` as the original draft page path. Derive the release page source path by replacing leading `product/development/pages` with `product/release/pages`.
   - Derive the mock output path by replacing leading `product/release/pages` with `product/development/mock`.
   - Sort rows by numeric `生成顺序`.

2. Initialize or update status.
   - Ensure `product/development/mock` exists.
   - Create `product/development/mock/_generation-status.md` if missing, using `references/mock-generation-status-template.md`.
   - If status exists, preserve completed, blocked, skipped, and source-removed rows.
   - Reconcile status with the current sitemap table: add new rows, mark missing sitemap rows as `源表已移除`, and keep existing completion records.

3. Select one unfinished page.
   - Choose the first row by `生成顺序` whose status is not `已完成`, `已跳过`, or `已阻塞`.
   - If its mock output file already exists and passes the current `product-page-mock-draft` checklist, mark it `已完成` instead of regenerating.
   - If its mock output file exists but lacks `来源对照矩阵`, lacks `内容来源类型`, or does not map the release page's layout/element/state/action tables, treat it as stale and regenerate it unless the user explicitly forbids regeneration.
   - If the release page source file does not exist, mark it `已阻塞` and record that `product-page-release` must generate the release page first.
   - If required fields are missing, especially `页面ID`, `页面名称`, or `页面级MD文件`, mark it `已阻塞`.

4. Generate exactly one mock page.
   - For the selected row, follow `skills/product-page-mock-draft/SKILL.md`.
   - Use the derived release page source file as the single input page.
   - Write the mock draft to the derived `product/development/mock` path.
   - Do not generate any other mock page in the same sub-step.

5. Write completion status.
   - Update `_generation-status.md` immediately after the mock generation attempt.
   - Record status, release source path, mock output path, generation timestamp, source page ID, page name, assumptions count, confirmation questions count, and any blockers.
   - If generation failed, record `已阻塞` with the concrete reason and do not mark it complete.

6. Continue until done.
   - After each status update, select the next unfinished row and repeat steps 3-5.
   - Continue only while context remains reliable. If the session is becoming long, stop after the current page and leave `_generation-status.md` accurate so the next invocation can resume safely.
   - When all rows are complete, write a final summary in `_generation-status.md`.

## Status Values

Use only these statuses:

- `待生成`: row exists in sitemap and no mock draft has been generated.
- `生成中`: currently selected row.
- `已完成`: mock draft exists and passes basic checklist.
- `已阻塞`: cannot generate because the release page is missing, required source tables are missing, or generation failed.
- `已跳过`: user explicitly skipped the page.
- `源表已移除`: page existed in status but no longer exists in current sitemap.

## Product Page Mock Draft Contract

For each selected page, obey these `product-page-mock-draft` rules:

- Process exactly one release page per selected sitemap row.
- Input source is the release page derived from the row's `页面级MD文件`.
- Output path preserves the release page filename relative to `product/release/pages`, under `product/development/mock`.
- Generate content-only mock data: text, labels, options, media descriptions, sample records, and state-specific display copy.
- Exclude interaction execution logic, analytics/tracking, API definitions, request/response structures, backend behavior, and implementation code.
- Strictly map source rows from `页面布局与内容区块`, `页面元素清单`, `元素状态矩阵`, and `交互 Action 与执行效果` into mock content or explicit unmapped reasons.
- Every content row must label `内容来源类型` as `静态` or `动态`, with `动态来源说明` for dynamic content.
- Keep all mock-level uncertainty inside that mock file's final `Mock 假设与待确认统一清单`.

## Hard Rules

- Do not batch multiple pages into one mock file.
- Do not skip status updates.
- Do not overwrite completed mock files unless the user explicitly asks to regenerate.
- Do not invent missing release page source paths. Block the row instead.
- Do not alter `product/release/product-overview-release.md` or files under `product/release/pages`.
- Do not mark a page complete unless the mock file exists at the expected `product/development/mock` path.
- Do not mark a page complete if it lacks `来源对照矩阵`, `内容来源类型`, or source table mappings.

## Resources

- `references/mock-generation-status-template.md`: status file structure.
- `references/mock-orchestration-quality-checklist.md`: final orchestration checks.

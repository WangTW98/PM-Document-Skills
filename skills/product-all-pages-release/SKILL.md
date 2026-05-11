---
name: product-all-pages-release
description: Orchestrate generation of all release page Markdown documents from draft pages by reading product/release/product-overview-release.md, parsing the Sitemap 页面生成总表, maintaining product/release/pages/_generation-status.md, selecting exactly one unfinished page at a time, following product-page-release single-page rules for that page, marking completion or blockage, and continuing page-by-page until all sitemap rows are complete.
---

# Product All Pages Release

## Overview

Generate release versions for every page listed in `product/release/product-overview-release.md` -> `Sitemap 页面生成总表`, while preserving the accuracy benefits of `product-page-release`: only one page is released at a time, then status is written before moving to the next row.

This is a runner-neutral orchestration skill. It does not replace `product-page-release`; it uses that skill's rules as the page-release contract for each row.

## Inputs And Outputs

Required inputs:

- `product/release/product-overview-release.md`
- Draft page files under `product/development/pages`

Required status output:

- `product/release/pages/_generation-status.md`

Generated release page outputs:

- `product/release/pages/<same-relative-page-filename>.md`, corresponding to each row's `页面级MD文件` draft path.

## Workflow

1. Load orchestration context.
   - Read `product/release/product-overview-release.md`.
   - Locate and parse `Sitemap 页面生成总表`.
   - Extract each row's `生成顺序`, `页面ID`, `父页面ID`, `层级`, `页面名称`, and `页面级MD文件`.
   - Treat `页面级MD文件` as the draft source path. Derive the release output path by replacing the leading `product/development/pages` with `product/release/pages`.
   - Sort rows by numeric `生成顺序`.

2. Initialize or update status.
   - Ensure `product/release/pages` exists.
   - Create `product/release/pages/_generation-status.md` if missing, using `references/release-generation-status-template.md`.
   - If status exists, preserve completed, blocked, skipped, and source-removed rows.
   - Reconcile status with the current sitemap table: add new rows, mark missing sitemap rows as `源表已移除`, and keep existing completion records.

3. Select one unfinished page.
   - Choose the first row by `生成顺序` whose status is not `已完成`, `已跳过`, or `已阻塞`.
   - If its release output file already exists and passes the page release checklist, mark it `已完成` instead of regenerating.
   - If the draft source file does not exist, mark it `已阻塞` and record that `product-page-draft` must generate the draft page first.
   - If the draft source file exists but lacks `## 7. 埋点事件统计设计`, lacks `埋点事件ID`, or lacks any `EVT-*` event ID, mark it `已阻塞` and record that `product-page-draft` must regenerate the draft with analytics before release.
   - If required fields are missing, especially `页面ID`, `页面名称`, or `页面级MD文件`, mark it `已阻塞`.

4. Release exactly one page.
   - For the selected row, follow `skills/product-page-release/SKILL.md`.
   - Use the row's `页面级MD文件` as the single draft source page.
   - Write the release page to the derived `product/release/pages` path.
   - Do not release any other page in the same sub-step.

5. Write completion status.
   - Update `_generation-status.md` immediately after the page release attempt.
   - Record status, draft source path, release output path, generation timestamp, source page ID, page name, and any blockers.
   - If release failed or has unresolved `PA-*` / `PQ-*` decisions, record `已阻塞` with the concrete reason and do not mark it complete.

6. Continue until done.
   - After each status update, select the next unfinished row and repeat steps 3-5.
   - Continue only while context remains reliable. If the session is becoming long, stop after the current page and leave `_generation-status.md` accurate so the next invocation can resume safely.
   - When all rows are complete, write a final summary in `_generation-status.md`.

## Status Values

Use only these statuses:

- `待生成`: row exists in sitemap and no release page has been generated.
- `生成中`: currently selected row.
- `已完成`: release page exists and passes basic checklist.
- `已阻塞`: cannot release because draft page is missing, draft confirmation items are unresolved, or generation failed.
- `已跳过`: user explicitly skipped the page.
- `源表已移除`: page existed in status but no longer exists in current sitemap.

## Product Page Release Contract

For each selected page, obey these `product-page-release` rules:

- Process exactly one draft page per selected sitemap row.
- Input source is the row's `页面级MD文件`.
- Source draft page must already contain the mandatory analytics section and `EVT-*` event IDs from `product-page-draft`.
- Output path preserves the draft page filename relative to `product/development/pages`, under `product/release/pages`.
- Apply every `PA-*` / `PQ-*` Release handling decision from the draft page's final `页面假设与待确认统一清单`.
- Remove all `PA-*`, `PQ-*`, assumptions, open questions, uncertainty markers, and confirmation workflow sections.
- Write either one release page or one blocker file for the selected page.

## Hard Rules

- Do not batch multiple pages into one release file.
- Do not skip status updates.
- Do not overwrite completed release page files unless the user explicitly asks to regenerate.
- Do not invent missing draft source page paths. Block the row instead.
- Do not alter `product/release/product-overview-release.md`.
- Do not mark a page complete unless the release file exists at the expected `product/release/pages` path.
- Do not mark a page complete if the release page still contains `PA-*`, `PQ-*`, `假设`, or `待确认`.
- Do not mark a page complete if the release page lacks `埋点事件ID` or any `EVT-*` event ID.
- Do not attempt release from a stale draft page that lacks analytics event statistics; block it and request draft regeneration.

## Resources

- `references/release-generation-status-template.md`: status file structure.
- `references/release-orchestration-quality-checklist.md`: final orchestration checks.

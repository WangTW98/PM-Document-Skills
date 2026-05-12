---
name: product-all-pages-mock-release
description: Orchestrate generation of all confirmed release mock Markdown documents from mock draft pages by reading product/release/product-overview-release.md, parsing the Sitemap 页面生成总表, maintaining product/release/mock/_generation-status.md, selecting exactly one unfinished page at a time, following product-page-mock-release single-page rules for that page, marking completion or blockage, and continuing page-by-page until all sitemap rows are complete.
---

# Product All Pages Mock Release

## Overview

Generate release mock versions for every page listed in `product/release/product-overview-release.md` -> `Sitemap 页面生成总表`, while preserving the accuracy benefits of `product-page-mock-release`: only one mock draft page is released at a time, then status is written before moving to the next row.

This is a runner-neutral orchestration skill. It does not replace `product-page-mock-release`; it uses that skill's rules as the mock-release contract for each row.

The generated release mock files are confirmed content-only specifications. They must contain final display content and mock data, without unresolved assumptions, `MA-*` / `MQ-*` IDs, interaction execution, analytics, API contracts, backend behavior, or implementation logic.

## Inputs And Outputs

Required inputs:

- `product/release/product-overview-release.md`
- Mock draft page files under `product/development/mock`

Required status output:

- `product/release/mock/_generation-status.md`

Generated release mock outputs:

- `product/release/mock/<same-relative-page-filename>.md`, corresponding to each row's mock draft file.

## Workflow

1. Load orchestration context.
   - Read `product/release/product-overview-release.md`.
   - Locate and parse `Sitemap 页面生成总表`.
   - Extract each row's `生成顺序`, `页面ID`, `父页面ID`, `层级`, `页面名称`, and `页面级MD文件`.
   - Use `页面级MD文件` only as a page filename / relative filename key from the sitemap.
   - The single-page input source must be `product/development/mock/<same-relative-page-filename>.md`.
   - Do not treat any other directory as a valid mock release input source.
   - Derive the release mock output path by preserving the same relative filename under `product/release/mock`.
   - Sort rows by numeric `生成顺序`.

2. Initialize or update status.
   - Ensure `product/release/mock` exists.
   - Create `product/release/mock/_generation-status.md` if missing, using `references/mock-release-generation-status-template.md`.
   - If status exists, preserve completed, blocked, skipped, and source-removed rows.
   - Reconcile status with the current sitemap table: add new rows, mark missing sitemap rows as `源表已移除`, and keep existing completion records.

3. Select one unfinished page.
   - Choose the first row by `生成顺序` whose status is not `已完成`, `已跳过`, or `已阻塞`.
   - If its release mock output file already exists and passes the current `product-page-mock-release` checklist, mark it `已完成` instead of regenerating.
   - If the `product/development/mock/...` source file does not exist, mark it `已阻塞` and record that the required mock draft source is missing.
   - If the `product/development/mock/...` source file exists but lacks `Mock 假设与待确认统一清单`, lacks `内容来源类型`, lacks `用户补充描述`, or lacks the core mock content sections, mark it `已阻塞` and record that the mock draft source must be corrected.
   - If required fields are missing, especially `页面ID`, `页面名称`, or `页面级MD文件`, mark it `已阻塞`.

4. Release exactly one mock page.
   - For the selected row, follow `skills/product-page-mock-release/SKILL.md`.
   - Use only the selected `product/development/mock/...` file as the single input page.
   - Write the release mock page to the derived `product/release/mock` path.
   - Do not release any other mock page in the same sub-step.

5. Write completion status.
   - Update `_generation-status.md` immediately after the mock release attempt.
   - Record status, mock draft source path, release mock output path, generation timestamp, source page ID, page name, and any blockers.
   - If release failed or has unresolved `MA-*` / `MQ-*` decisions, record `已阻塞` with the concrete reason and do not mark it complete.

6. Continue until done.
   - After each status update, select the next unfinished row and repeat steps 3-5.
   - Continue only while context remains reliable. If the session is becoming long, stop after the current page and leave `_generation-status.md` accurate so the next invocation can resume safely.
   - When all rows are complete, write a final summary in `_generation-status.md`.

## Status Values

Use only these statuses:

- `待生成`: row exists in sitemap and no release mock page has been generated.
- `生成中`: currently selected row.
- `已完成`: release mock page exists and passes basic checklist.
- `已阻塞`: cannot release because the mock draft is missing, mock confirmation items are unresolved, required mock sections are missing, or generation failed.
- `已跳过`: user explicitly skipped the page.
- `源表已移除`: page existed in status but no longer exists in current sitemap.

## Product Page Mock Release Contract

For each selected page, obey these `product-page-mock-release` rules:

- Process exactly one mock draft page per selected sitemap row.
- Input source is always `product/development/mock/<same-relative-page-filename>.md`.
- The sitemap's `页面级MD文件` is only a filename key used to locate the same page under `product/development/mock`.
- Output path preserves the mock draft filename relative to `product/development/mock`, under `product/release/mock`.
- Apply every `MA-*` / `MQ-*` Release handling decision from the mock draft page's final `Mock 假设与待确认统一清单`.
- Apply every non-empty `用户补充描述` instruction from the mock draft page.
- Remove all `MA-*`, `MQ-*`, assumptions, open questions, uncertainty markers, and confirmation workflow sections.
- Preserve confirmed display content, mock data, media descriptions, state copy, action visible-content mapping, and static/dynamic content source labels.
- Keep the release mock content-only: no interaction execution, analytics/tracking, API definitions, request/response structures, backend behavior, or implementation code.
- Write either one release mock page or one blocker file for the selected page.

## Hard Rules

- Do not batch multiple mock pages into one release mock file.
- Do not skip status updates.
- Do not overwrite completed release mock files unless the user explicitly asks to regenerate.
- Do not invent missing `product/development/mock/...` source paths. Block the row instead.
- Do not alter `product/release/product-overview-release.md`.
- Do not alter source files under `product/development/mock` while releasing.
- Do not mark a page complete unless the release mock file exists at the expected `product/release/mock` path.
- Do not mark a page complete if the release mock page still contains `MA-*`, `MQ-*`, `假设`, or `待确认`.
- Do not mark a page complete if the release mock page contains interaction execution, analytics, API contracts, backend behavior, or implementation code.

## Resources

- `references/mock-release-generation-status-template.md`: status file structure.
- `references/mock-release-orchestration-quality-checklist.md`: final orchestration checks.

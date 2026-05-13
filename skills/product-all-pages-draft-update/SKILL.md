---
name: product-all-pages-draft-update
description: Orchestrate versioned updates for all page-level draft Markdown documents by reading product/release/product-sitemap-release.md, parsing the Sitemap 页面生成总表, resolving the correct layout-family file for each row, maintaining product/development/pages/_revision-status.md, selecting one unfinished page at a time, following product-page-draft-update single-page rules, and saving updated page-vN drafts with user confirmations and 用户补充描述 merged into each page body.
---

# Product All Pages Draft Update

## Overview

Update every page draft listed in `product/release/product-sitemap-release.md` -> `Sitemap 页面生成总表`, while preserving the accuracy benefits of `product-page-draft-update`: only one page draft is updated at a time, then status is written before moving to the next row.

This skill never writes release page files. It is the batch companion to `product-page-draft-update`.

## Inputs And Outputs

Required inputs:

- `product/release/product-sitemap-release.md`
- Draft page files under `product/development/pages`

Required status output:

- `product/development/pages/_revision-status.md`

Generated outputs:

- Versioned draft files beside each page source, for example `product/development/pages/010-login-v2.md`.

## Workflow

1. Load orchestration context.
   - Read `product/release/product-sitemap-release.md`.
   - Locate and parse `Sitemap 页面生成总表`.
   - Extract each row's `生成顺序`, `页面ID`, `父页面ID`, `层级`, `页面名称`, and `页面级MD文件`.
   - Treat `页面级MD文件` as the canonical draft source path.
   - For processing, use the highest available versioned sibling of that path, such as `010-login-v3.md`; if none exists, use the canonical draft source path.
   - Sort rows by numeric `生成顺序`.

2. Initialize or update status.
   - Ensure `product/development/pages` exists.
   - Create `product/development/pages/_revision-status.md` if missing.
   - If status exists, preserve completed, blocked, skipped, and source-removed rows.
   - Reconcile status with the current sitemap table: add new rows, mark missing sitemap rows as `源表已移除`, and keep existing completion records.

3. Select one unfinished page.
   - Choose the first row by `生成顺序` whose status is not `已完成`, `已跳过`, or `已阻塞`.
   - If a newer versioned draft already exists and passes the `product-page-draft-update` checks, mark it `已完成` instead of regenerating unless the user asks to continue from it.
   - If the draft source file does not exist, mark it `已阻塞` and record that `product-page-draft` must generate the draft page first.
   - If the draft source file exists but lacks `## 7. 埋点事件统计设计`, lacks `埋点事件ID`, lacks any `EVT-*` event ID, or lacks `用户补充描述`, mark it `已阻塞` and record that `product-page-draft` must regenerate the draft with analytics and user supplement support.
   - If required fields are missing, especially `页面ID`, `页面名称`, or `页面级MD文件`, mark it `已阻塞`.

4. Update exactly one page draft.
   - For the selected row, follow `skills/product-page-draft-update/SKILL.md`.
   - Resolve the row's matching `product-layout-release*.md` file before update. If no unique match exists, mark the row `已阻塞`.
   - Use the row's canonical `页面级MD文件` and latest versioned sibling as the selected single draft source page.
   - Write the next versioned draft beside the selected source draft.
   - Do not process any other page in the same sub-step.

5. Write completion status.
   - Update `_revision-status.md` immediately after the page update attempt.
   - Record status, canonical draft source path, actual source path, generated versioned draft path, generation timestamp, source page ID, page name, and any blockers.
   - Do not mark a row complete if the generated draft only changes the version, timestamp, or filename without applying the user's confirmations or `用户补充描述` into the page body.
   - Unresolved `PA-*` / `PQ-*` decisions are expected and should not by themselves block the row if they are preserved in the revised draft.

6. Continue until done.
   - After each status update, select the next unfinished row and repeat steps 3-5.
   - Continue only while context remains reliable. If the session is becoming long, stop after the current page and leave `_revision-status.md` accurate so the next invocation can resume safely.
   - When all rows are complete, write a final summary in `_revision-status.md`.

## Status Values

Use only these statuses:

- `待更新`: row exists in sitemap and no revised draft has been generated.
- `更新中`: currently selected row.
- `已完成`: revised draft exists and passes basic checks.
- `已阻塞`: cannot update because source draft is missing, stale, malformed, or update failed.
- `已跳过`: user explicitly skipped the page.
- `源表已移除`: page existed in status but no longer exists in current sitemap.

## Product Page Draft Update Contract

For each selected page, obey these `product-page-draft-update` rules:

- Process exactly one draft page per selected sitemap row.
- Canonical input source is the row's `页面级MD文件`; actual processing source is the latest versioned sibling when one exists.
- Source draft page must include mandatory analytics and `用户补充描述`.
- Source draft page must also be traceable to exactly one matched layout-family file.
- Merge every filled `PA-*` / `PQ-*` user decision and every non-empty `用户补充描述` instruction into the page body.
- Output must be a substantively regenerated page draft. Version-only copies are invalid.
- Output must follow `skills/product-page-draft/references/page-draft-template.md` exactly in required chapter structure and must not omit sections.
- Output must keep remaining/new `PA-*` / `PQ-*` workflow rows, mandatory analytics, and an empty final `用户补充描述`.
- Write exactly one versioned draft or record one blocked row.

## Hard Rules

- Do not create release page files.
- Do not mark a page complete if the revised draft no longer follows the page draft template or is missing required sections.
- Do not batch multiple pages into one draft file.
- Do not skip status updates.
- Do not overwrite completed versioned drafts unless the user explicitly asks to regenerate.
- Do not invent missing draft source page paths. Block the row instead.
- Do not alter `product/release/product-sitemap-release.md`.
- Do not mark a page complete unless the versioned draft file exists.
- Do not complete a versioned draft whose only material difference is document version, timestamp, or filename.
- Do not mark a page complete if its layout-family match is missing or ambiguous.

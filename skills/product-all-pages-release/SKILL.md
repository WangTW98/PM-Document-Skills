---
name: product-all-pages-release
description: Orchestrate generation of all release page bundles from the latest draft page bundles by reading product/release/product-sitemap-release.md, parsing the Sitemap 页面生成总表, resolving the correct release layout file for each row, maintaining product/release/pages/_generation-status.md, selecting exactly one unfinished sitemap row at a time, following product-page-release single-page-bundle rules for that row, marking completion or blockage, and continuing row-by-row until all sitemap rows are complete with a release directory structure that mirrors draft.
---

# Product All Pages Release

## Overview

Generate release bundles for every page listed in `product/release/product-sitemap-release.md` -> `Sitemap 页面生成总表`, while preserving the accuracy benefits of `product-page-release`: only one page bundle is released at a time, then status is written before moving to the next row.

This skill only creates final release output. For iterative draft updates across pages, use `product-all-pages-draft-update`.

This is a runner-neutral orchestration skill. It does not replace `product-page-release`; it uses that skill's rules as the page-release contract for each row.

## Inputs And Outputs

Required inputs:

- `product/release/product-sitemap-release.md`
- draft page bundles under `product/development/pages`

Required status output:

- `product/release/pages/_generation-status.md`

Generated release bundle outputs:

- `product/release/pages/<page-key>/index.md`
- optional release overlay docs in the same page directory
- the release directory layout must mirror the draft page directory layout under `product/development/pages/<page-key>/`

## Workflow

1. Load orchestration context.
   - Read `product/release/product-sitemap-release.md`.
   - Locate and parse `Sitemap 页面生成总表`.
   - Extract each row's `生成顺序`, `页面ID`, `父页面ID`, `层级`, `页面名称`, and `页面级MD文件`.
   - Treat `页面级MD文件` as the canonical draft page key.
   - Derive the draft bundle root by removing the trailing `.md`.
   - For processing, always use the highest available versioned main draft in that bundle, such as `index-v3.md`; if none exists, use `index.md`.
   - Derive the final release bundle root by replacing the leading `product/development/pages` with `product/release/pages` on the canonical page key, then removing `.md`.
   - Sort rows by numeric `生成顺序`.

2. Initialize or update status.
   - Ensure `product/release/pages` exists.
   - Create `product/release/pages/_generation-status.md` if missing, using `references/release-generation-status-template.md`.
   - If status exists, preserve completed, blocked, skipped, and source-removed rows.
   - Reconcile status with the current sitemap table: add new rows, mark missing sitemap rows as `源表已移除`, and keep existing completion records.

3. Select one unfinished page bundle.
   - Choose the first row by `生成顺序` whose status is not `已完成`, `已跳过`, or `已阻塞`.
   - If its release main file already exists and passes the page release checklist, mark it `已完成` instead of regenerating.
   - If the draft bundle root does not exist, mark it `已阻塞` and record that `product-page-draft` must generate the draft bundle first.
   - If the effective latest draft main file exists but lacks `## 7. 埋点事件统计设计`, lacks `埋点事件ID`, lacks any `EVT-*` event ID, or lacks `用户补充描述`, mark it `已阻塞` and record that `product-page-draft` must regenerate the draft bundle with analytics and user supplement support before release.
   - If required fields are missing, especially `页面ID`, `页面名称`, or `页面级MD文件`, mark it `已阻塞`.

4. Release exactly one page bundle.
   - For the selected row, follow `skills/product-page-release/SKILL.md`.
   - Resolve the row's matching release layout file before release. If no unique match exists, mark the row `已阻塞`.
   - Use the row's canonical draft bundle root and latest versioned main draft as the selected single source bundle.
   - Resolve source overlay files using the same latest-version rules as `product-page-release`: prefer same-version overlay files, then highest available overlay version, then canonical non-versioned overlay file.
   - Write the release bundle to the derived `product/release/pages/<page-key>/` directory.
   - Keep the release directory structure aligned with the draft bundle structure.
   - Do not release any other sitemap row in the same sub-step.

5. Write completion status.
   - Update `_generation-status.md` immediately after the page release attempt.
   - Record status, draft bundle root, actual draft main path, actual draft overlay source list, release bundle root, generated overlay doc list, generation timestamp, source page ID, page name, and any blockers.
   - If release failed or has unresolved `PA-*` / `PQ-*` decisions, record `已阻塞` with the concrete reason and do not mark it complete.

6. Continue until done.
   - After each status update, select the next unfinished row and repeat steps 3-5.
   - Continue only while context remains reliable. If the session is becoming long, stop after the current row and leave `_generation-status.md` accurate so the next invocation can resume safely.
   - When all rows are complete, write a final summary in `_generation-status.md`.

## Status Values

Use only these statuses:

- `待生成`: row exists in sitemap and no release bundle has been generated
- `生成中`: currently selected row
- `已完成`: release bundle exists and passes basic checklist
- `已阻塞`: cannot release because draft bundle is missing, draft confirmation items are unresolved, or generation failed
- `已跳过`: user explicitly skipped the row
- `源表已移除`: page existed in status but no longer exists in current sitemap

## Product Page Release Contract

For each selected row, obey these `product-page-release` rules:

- Process exactly one draft page bundle per selected sitemap row.
- Canonical input source is the row's `页面级MD文件`; actual processing source is the latest versioned main draft inside the derived bundle root when one exists.
- Effective overlay sources must be resolved from the same draft directory by preferring same-version overlay files, then highest available overlay versions, then canonical non-versioned overlay files.
- Source draft main file must already contain the mandatory analytics section and `EVT-*` event IDs from `product-page-draft`.
- The row must be traceable to exactly one matched release layout file before release.
- Source draft main file must include `用户补充描述`; `product-page-release` must analyze and apply non-empty supplement instructions before marking the page complete.
- Output path is the derived release bundle root under `product/release/pages`; the release main file is always `index.md`.
- Release output structure must mirror the draft page directory structure, while removing any `-vN` suffixes from release filenames.
- Apply every `PA-*` / `PQ-*` Release handling decision from the draft main file's final `页面假设与待确认统一清单`.
- Apply every non-empty `用户补充描述` instruction from the draft main file.
- Remove all `PA-*`, `PQ-*`, assumptions, open questions, uncertainty markers, and confirmation workflow sections.
- Write either one release bundle or one blocker file for the selected row.

## Hard Rules

- Do not batch multiple sitemap rows into one release bundle.
- Do not skip status updates.
- Do not overwrite completed release bundles unless the user explicitly asks to regenerate.
- Do not invent missing draft bundle roots. Block the row instead.
- Do not alter `product/release/product-sitemap-release.md`.
- Do not mark a row complete unless the release main file exists at the expected bundle path.
- Do not mark a row complete if the release bundle still contains `PA-*`, `PQ-*`, `假设`, or `待确认`.
- Do not mark a row complete if the release main file lacks `埋点事件ID` or any `EVT-*` event ID.
- Do not attempt release from a stale draft bundle that lacks analytics event statistics; block it and request draft regeneration.
- Do not mark a row complete if its layout-file match is missing or ambiguous.
- Do not release from an older draft main file when a newer `index-vN.md` exists, unless the user explicitly requests that older file.
- Do not generate a release directory structure that differs from the draft directory structure.

## Resources

- `references/release-generation-status-template.md`: status file structure.
- `references/release-orchestration-quality-checklist.md`: final orchestration checks.

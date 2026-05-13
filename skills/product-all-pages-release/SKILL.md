---
name: product-all-pages-release
description: Orchestrate page-by-page processing for all page drafts. By default, when the user has not explicitly asked for final/release/正式版 pages, select one page draft at a time and use product-page-release in draft revision mode to apply completed PA-/PQ- decisions and 用户补充描述 edits into versioned draft files while preserving the review loop. Only when the user explicitly asks for final/release output, generate all release page Markdown documents under product/release/pages using the existing product-page-release final mode.
---

# Product All Pages Release

## Overview

Process every page listed in `product/release/product-sitemap-release.md` -> `Sitemap 页面生成总表`, while preserving the accuracy benefits of `product-page-release`: only one page is processed at a time, then status is written before moving to the next row.

This skill has two modes:

- Draft revision mode, which is the default unless the user explicitly asks for final/release/正式版 pages.
- Final release mode, which keeps the existing orchestration behavior and writes confirmed page specs under `product/release/pages`.

This is a runner-neutral orchestration skill. It does not replace `product-page-release`; it uses that skill's rules as the page-release contract for each row.

## Inputs And Outputs

Required inputs:

- `product/release/product-sitemap-release.md`
- Draft page files under `product/development/pages`

Required status output:

- Draft revision mode: `product/development/pages/_revision-status.md`
- Final release mode: `product/release/pages/_generation-status.md`

Generated outputs:

- Draft revision mode: versioned draft files beside each source draft, for example `product/development/pages/010-login-v2.md`.
- Final release mode: `product/release/pages/<same-relative-page-filename>.md`, corresponding to each row's `页面级MD文件` draft path.

## Workflow

0. Determine mode before writing any file.
   - Use final release mode only when the user explicitly asks to generate final/release/正式版 pages, publish confirmed page releases, write under `product/release/pages`, or remove all draft confirmation structures.
   - Use draft revision mode for requests such as "处理所有页面修改", "根据已填写确认项更新页面 draft", "继续完善所有页面", or any request that does not clearly ask for final/release output.
   - If the request is ambiguous, choose draft revision mode. Do not create or overwrite `product/release/pages` outputs without explicit final-release intent.

1. Load orchestration context.
   - Read `product/release/product-sitemap-release.md`.
   - Locate and parse `Sitemap 页面生成总表`.
   - Extract each row's `生成顺序`, `页面ID`, `父页面ID`, `层级`, `页面名称`, and `页面级MD文件`.
   - Treat `页面级MD文件` as the canonical draft source path.
   - For processing, use the highest available versioned sibling of that path, such as `010-login-v3.md`; if none exists, use the canonical draft source path.
   - Derive the final release output path from the canonical draft source path by replacing the leading `product/development/pages` with `product/release/pages`; do not carry draft revision suffixes into final release filenames.
   - Sort rows by numeric `生成顺序`.

2. Initialize or update status.
   - In draft revision mode, ensure `product/development/pages` exists and use `product/development/pages/_revision-status.md`.
   - In final release mode, ensure `product/release/pages` exists and use `product/release/pages/_generation-status.md`.
   - Create the relevant status file if missing, using `references/release-generation-status-template.md` as the structure reference.
   - If status exists, preserve completed, blocked, skipped, and source-removed rows.
   - Reconcile status with the current sitemap table: add new rows, mark missing sitemap rows as `源表已移除`, and keep existing completion records.

3. Select one unfinished page.
   - Choose the first row by `生成顺序` whose status is not `已完成`, `已跳过`, or `已阻塞`.
   - In draft revision mode, if a newer versioned draft already exists and passes the product-page-release draft revision checks, mark it `已完成` instead of regenerating unless the user asks to continue from it.
   - In final release mode, if its release output file already exists and passes the page release checklist, mark it `已完成` instead of regenerating.
   - If the draft source file does not exist, mark it `已阻塞` and record that `product-page-draft` must generate the draft page first.
   - If the draft source file exists but lacks `## 7. 埋点事件统计设计`, lacks `埋点事件ID`, lacks any `EVT-*` event ID, or lacks `用户补充描述`, mark it `已阻塞` and record that `product-page-draft` must regenerate the draft with analytics and user supplement support before release.
   - If required fields are missing, especially `页面ID`, `页面名称`, or `页面级MD文件`, mark it `已阻塞`.

4. Process exactly one page.
   - For the selected row, follow `skills/product-page-release/SKILL.md`.
   - Use the row's `页面级MD文件` as the canonical draft source page and the latest versioned sibling as the actual processing source when one exists.
   - In draft revision mode, run `product-page-release` in draft revision mode and write the next versioned draft beside the source draft.
   - In final release mode, run `product-page-release` in final release mode and write the release page to the derived `product/release/pages` path.
   - Do not process any other page in the same sub-step.

5. Write completion status.
   - Update the selected mode's status file immediately after the page processing attempt.
   - Record status, draft source path, generated draft/release output path, generation timestamp, source page ID, page name, mode, and any blockers.
   - In draft revision mode, unresolved `PA-*` / `PQ-*` decisions are expected and should not by themselves block the row if they are preserved in the revised draft.
   - In final release mode, if release failed or has unresolved `PA-*` / `PQ-*` decisions, record `已阻塞` with the concrete reason and do not mark it complete.

6. Continue until done.
   - After each status update, select the next unfinished row and repeat steps 3-5.
   - Continue only while context remains reliable. If the session is becoming long, stop after the current page and leave the selected mode's status file accurate so the next invocation can resume safely.
   - When all rows are complete, write a final summary in the selected mode's status file.

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
- Canonical input source is the row's `页面级MD文件`; actual processing source is the latest versioned sibling when one exists.
- Source draft page must already contain the mandatory analytics section and `EVT-*` event IDs from `product-page-draft`.
- Source draft page must include `用户补充描述`; `product-page-release` must analyze and apply non-empty supplement instructions before marking the page complete.
- In draft revision mode, output is the next versioned draft beside the source draft and must keep the page assumption/question list plus an empty final `用户补充描述`.
- In final release mode, output path is derived from the canonical `页面级MD文件` under `product/release/pages`; draft revision suffixes such as `-v2` or `-v3` are not carried into release filenames.
- Apply every `PA-*` / `PQ-*` Release handling decision from the draft page's final `页面假设与待确认统一清单`.
- Apply every non-empty `用户补充描述` instruction from the draft page.
- In draft revision mode, preserve remaining/new `PA-*`, `PQ-*`, assumptions, open questions, uncertainty markers, and confirmation workflow sections for the next review loop.
- In final release mode, remove all `PA-*`, `PQ-*`, assumptions, open questions, uncertainty markers, and confirmation workflow sections.
- Write either one release page or one blocker file for the selected page.

## Mode Rules

- Default to draft revision mode unless final-release intent is explicit.
- Draft revision mode is a loop: apply each page's completed decisions and supplement, then produce the next reviewable page draft with remaining/new assumptions and questions.
- Final release mode is terminal: all material page assumptions/questions must be resolved before writing `product/release/pages`.
- A request to "处理已修改内容", "继续修改", "生成新版 draft", or "根据补充描述更新" is not final-release intent.
- Phrases such as "生成最终版", "生成正式版", "生成 release", "输出到 product/release/pages", or "去掉所有待确认" are final-release intent.

## Hard Rules

- Do not batch multiple pages into one release file.
- Do not skip status updates.
- Do not overwrite completed release page files unless the user explicitly asks to regenerate.
- Do not invent missing draft source page paths. Block the row instead.
- Do not alter `product/release/product-sitemap-release.md`.
- Do not mark a page complete unless the release file exists at the expected `product/release/pages` path.
- In final release mode, do not mark a page complete if the release page still contains `PA-*`, `PQ-*`, `假设`, or `待确认`.
- In final release mode, do not mark a page complete if the release page lacks `埋点事件ID` or any `EVT-*` event ID.
- Do not attempt release from a stale draft page that lacks analytics event statistics; block it and request draft regeneration.
- Do not use final release behavior for ordinary draft update requests.

## Resources

- `references/release-generation-status-template.md`: status file structure.
- `references/release-orchestration-quality-checklist.md`: final orchestration checks.

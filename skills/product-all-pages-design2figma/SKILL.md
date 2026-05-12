---
name: product-all-pages-design2figma
description: "Orchestrate creation of all page designs into Figma by reading product/release/product-overview-release.md, parsing the Sitemap 页面生成总表, maintaining product/release/design/_figma-remote-mcp-status.md, selecting exactly one unfinished page at a time, following the product-pages-design2figma single-page Figma Remote MCP rules, writing completion status, and continuing page-by-page until all sitemap rows are complete."
---

# Product All Pages Design2Figma

## Overview

Create all page designs in Figma, one page at a time, from release design MD files under `product/release/design`.

This is a runner-neutral orchestration skill for Figma Remote MCP execution. It does not replace the single-page Figma creation skill; it uses `product-pages-design2figma` as the single-page contract for each selected sitemap row.

This skill must maintain a resumable status file:

- `product/release/design/_figma-remote-mcp-status.md`

The orchestration reads the product sitemap, finds the next unfinished page, calls/follows the single-page Design2Figma rule for that page, records the created Figma destination or blocker, then moves to the next unfinished page.

## Inputs And Outputs

Required inputs:

- `product/release/product-overview-release.md`
- Release design page files under `product/release/design`
- One Figma link provided by the user.
- One target Figma page, provided as a page name, page node, selected node, or explicit target node in the Figma link.

Required status output:

- `product/release/design/_figma-remote-mcp-status.md`

External output:

- One created Figma page/frame per completed sitemap row, inside the user-provided Figma file and target Figma page.

## Workflow

1. Load orchestration context.
   - Read `product/release/product-overview-release.md`.
   - Locate and parse `Sitemap 页面生成总表`.
   - Extract each row's `生成顺序`, `页面ID`, `父页面ID`, `层级`, `页面名称`, and `页面级MD文件`.
   - Use `页面级MD文件` only as a page filename / relative filename key from the sitemap.
   - The single-page design MD source must be `product/release/design/<same-relative-page-filename>.md`.
   - Sort rows by numeric `生成顺序`.

2. Resolve shared Figma destination context.
   - Require one Figma link from the user.
   - Require one target Figma page or target node from the user.
   - Parse and record the Figma URL, `fileKey`, `node-id` when present, target page name, insert mode, and replacement/append policy.
   - If the Figma destination is ambiguous, stop before processing pages.
   - Do not write to a default Figma page.

3. Initialize or update status.
   - Ensure `product/release/design` exists.
   - Create `product/release/design/_figma-remote-mcp-status.md` if missing, using `references/figma-remote-mcp-status-template.md`.
   - If status exists, preserve completed, blocked, skipped, source-removed, and needs-regeneration rows.
   - Reconcile status with the current sitemap table: add new rows, mark missing sitemap rows as `源表已移除`, and keep existing completion records.
   - If the Figma link or target page changes from the status file's recorded destination, do not silently overwrite or duplicate existing completed Figma frames. Mark completed rows as `需重新生成` or ask for explicit confirmation.

4. Select one unfinished page.
   - Choose the first row by `生成顺序` whose status is not `已完成`, `已跳过`, or `已阻塞`.
   - If the row is `需重新生成`, process it only when the user explicitly requested regeneration.
   - If the `product/release/design/...` source file does not exist, mark it `已阻塞` and record that the design release source is missing.
   - If the source file lacks `AI 可读样式结构`, `Figma Remote MCP 生成提示`, or required layout integrity checks, mark it `已阻塞`.
   - If the source file contains `MA-*`, `MQ-*`, `假设`, or `待确认`, mark it `已阻塞`.
   - If required sitemap fields are missing, especially `页面ID`, `页面名称`, or `页面级MD文件`, mark it `已阻塞`.

5. Create exactly one page in Figma.
   - For the selected row, follow `skills/product-pages-design2figma/SKILL.md`.
   - Use only the selected `product/release/design/...` file as the single page MD source.
   - Use the resolved Figma link and target Figma page from this orchestration.
   - Create exactly one Figma page/frame for the selected row.
   - Do not create any other page in the same sub-step.

6. Write completion status.
   - Update `_figma-remote-mcp-status.md` immediately after the Figma creation attempt.
   - Record status, page source path, Figma file URL, fileKey, target page, target node, created top-level frame name, created node ID when available, generation timestamp, and any blockers.
   - If creation failed, target resolution failed, source validation failed, or post-write verification failed, record `已阻塞` with the concrete reason and do not mark it complete.

7. Continue until done.
   - After each status update, select the next unfinished row and repeat steps 4-6.
   - Continue only while context remains reliable. If the session is becoming long, stop after the current page and leave `_figma-remote-mcp-status.md` accurate so the next invocation can resume safely.
   - When all rows are complete, write a final summary in `_figma-remote-mcp-status.md`.

## Status Values

Use only these statuses:

- `待生成`: row exists in sitemap and has not been created in Figma.
- `生成中`: currently selected row.
- `已完成`: Figma page/frame was created in the intended Figma file and target page and passed verification.
- `已阻塞`: cannot create because source MD is missing/invalid, Figma destination is ambiguous, Figma write failed, or verification failed.
- `已跳过`: user explicitly skipped the page.
- `源表已移除`: page existed in status but no longer exists in current sitemap.
- `需重新生成`: destination or source changed after completion; regenerate only with explicit user permission.

## Single-Page Design2Figma Contract

For each selected page, obey these `product-pages-design2figma` rules:

- Process exactly one `product/release/design/...` page MD per selected sitemap row.
- Input source is always `product/release/design/<same-relative-page-filename>.md`.
- The sitemap's `页面级MD文件` is only a filename key used to locate the same page under `product/release/design`.
- Parse the Figma link and inspect the target Figma file/page before any write operation.
- Do not write to Figma if the target page is ambiguous.
- Create exactly one top-level page frame or target insertion frame.
- Use the required top-level Figma layer/frame naming from the single-page skill.
- Verify the created Figma node after writing when tooling allows.
- Do not create interaction prototypes, analytics layers, API annotations, backend diagrams, business workflow nodes, or implementation-code artifacts.

## Hard Rules

- Do not batch multiple page MD files into one Figma frame.
- Do not skip status updates.
- Do not write to Figma before resolving the shared Figma link and target page.
- Do not invent missing `product/release/design/...` source paths. Block the row instead.
- Do not auto-select a target Figma page when the destination is ambiguous.
- Do not overwrite existing Figma content unless the user explicitly requested replacement.
- Do not alter `product/release/product-overview-release.md`.
- Do not alter source files under `product/release/design` while creating Figma pages.
- Do not mark a row complete unless the Figma output was created in the intended Figma file and target page.
- Do not mark a row complete if post-write verification finds wrong target page, missing content, unresolved overlap/clipping, incorrect layer order, unsupported application form, or prohibited business/analytics/API artifacts.

## Resources

- `references/figma-remote-mcp-status-template.md`: status file structure.
- `references/figma-remote-mcp-orchestration-quality-checklist.md`: final orchestration checks.

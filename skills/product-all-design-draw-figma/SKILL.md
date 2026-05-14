---
name: product-all-design-draw-figma
description: Orchestrate Figma restoration from product/design one page at a time by maintaining a draw status file, selecting exactly one `*-figma.json` artifact each round, and delegating execution to product-design-draw-figma with MCP-stability and token-efficiency rules. Use when an AI agent needs to work through many Figma JSON pages safely without batch rendering.
---

# Product All Design Draw Figma

## Overview

This is an orchestration skill.

It does not replace `product-design-draw-figma`.
It schedules repeated single-page Figma restoration rounds until every eligible `*-figma.json` artifact under `product/design/` has been processed.

The orchestration loop is:

1. discover candidate Figma JSON files
2. initialize or refresh a status file
3. select exactly one unfinished page
4. delegate the actual draw to `product-design-draw-figma`
5. persist the result
6. stop this round
7. continue on the next invocation

The skill is intentionally single-page-per-round to reduce MCP instability, remote write risk, and token waste.

## Inputs And Outputs

Required inputs:

- `product/design/`
- the delegated single-page skill at `../product-design-draw-figma/SKILL.md`
- the delegated contract reference at `../product-md-design/references/figma-json-contract.md`

Optional inputs:

- an explicit target Figma file URL or `fileKey`
- an explicit request to retry blocked pages
- an explicit request to rebuild the status queue from disk

Output root:

- `product/design/`

Status file:

- `product/design/_design-draw-figma-status.md`

Per invocation output rule:

- create or update the status file
- process exactly one eligible `*-figma.json`
- stop after that one page

## Status File Format

The status file must be short, deterministic, and cheap to reload.

Recommended structure:

```md
# Design Draw Figma Status

## Meta

- source_root: product/design
- target_figma_file_key: AbCdEf123456
- total_pages: 18
- completed_pages: 5
- pending_pages: 11
- blocked_pages: 2
- last_updated: 2026-05-14

## Queue

| 页面ID | 页面名称 | JSON文件 | 状态 | 尝试次数 | 目标 | 错误摘要 |
|---|---|---|---|---:|---|---|
| U-100 | 登录 | product/design/010-login/index-figma.json | done | 1 | AbCdEf123456 / U-100 登录 | |
| U-210 | 待办列表 | product/design/030-todo/index-figma.json | pending | 0 | AbCdEf123456 / U-210 待办列表 | |
| U-330 | 退款申请 | product/design/110-order/弹窗-退款申请-figma.json | blocked | 2 | AbCdEf123456 / U-330 退款申请 | unresolved instance |
```

Allowed status values:

- `pending`
- `in_progress`
- `done`
- `blocked`

## Workflow

1. Resolve the source queue.
   - Scan `product/design/` for files ending in `-figma.json`.
   - Ignore `-pencil.json`, blocker files, status files, and unrelated artifacts.
   - Sort candidates deterministically by path.

2. Initialize or refresh the status file.
   - If `product/design/_design-draw-figma-status.md` does not exist, create it from the discovered queue.
   - If it exists, prefer the status file as the primary queue source.
   - Refresh only missing rows or newly discovered files unless the user explicitly asks to rebuild the whole queue.
   - Never silently reset `done` rows to `pending`.

3. Minimize token load before delegation.
   - Read the status file first.
   - Do not read every JSON file into context.
   - Read only the single selected `*-figma.json` file for this round.
   - Do not duplicate large contract text in the orchestration output when the delegated skill already owns that logic.

4. Select exactly one target page.
   - Default selectable rows are `pending`.
   - `blocked` rows are not selectable unless the user explicitly asks to retry blocked pages.
   - `done` rows are not selectable unless the user explicitly asks to regenerate a specific page.
   - If multiple selectable rows exist, choose the first by stable queue order.
   - Before delegation, mark the row `in_progress`.

5. Delegate the draw.
   - Read `../product-design-draw-figma/SKILL.md`.
   - Follow all of its single-page validation and restore rules.
   - Pass exactly one explicit `-figma.json` source.
   - Reuse the same target `fileKey` when one has already been established, unless the user explicitly overrides it.
   - Let the delegated skill load `figma:figma-use` before any write call.

6. Classify failures conservatively.
   - Treat these as retryable transient failures:
     - MCP timeout
     - temporary Figma tool unavailability
     - target file access failure
     - context or token pressure during the draw round
   - Treat these as non-retryable blocking failures:
     - malformed JSON
     - missing contract-required fields
     - unresolved `instance` semantics that would change visible meaning
     - impossible icon or layout restoration without visible regression

7. Persist completion.
   - On success:
     - mark the row `done`
     - increment attempt count
     - record the target summary
     - clear the error digest
   - On retryable failure:
     - increment attempt count
     - if attempt count is still below the retry cap, move back to `pending`
     - otherwise mark `blocked`
     - write a short error digest only
   - On non-retryable failure:
     - increment attempt count
     - mark `blocked`
     - write a short error digest only

8. Stop after one page.
   - Never continue to a second page in the same invocation.
   - The next invocation resumes from the next eligible row.

## Retry And Stability Rules

- Default retry cap: `2`
- Do not retry indefinitely.
- Keep error digests short, such as:
  - `missing page.frame_name`
  - `unresolved instance`
  - `figma file unavailable`
  - `mcp timeout`
- Do not paste raw stack traces or large tool output into the status file.
- Prefer one shared Figma file across many pages to reduce file creation churn and remote context switching.
- Do not rescan and reparsed every JSON payload once a valid status file already exists, unless the user explicitly asks to refresh.

## Hard Constraints

- This skill is an orchestrator, not a renderer.
- Never draw more than one page per invocation.
- Never ask the delegated skill to process multiple files.
- Never read all page JSON contents into context in the same round.
- Never let long run logs accumulate in the status file.
- Never silently reclassify a contract error as retryable just to keep the queue moving.
- Never create a new Figma file every round unless the user asks for that behavior.
- Never bypass the delegated skill's requirement to load `figma:figma-use`.

## Compatibility

This skill must remain runner-neutral at the queue and workflow level.

Any AI runner must be able to:

- discover the same set of Figma JSON files
- derive the same next page from the status file
- delegate exactly one page to `product-design-draw-figma`
- continue from the same persisted queue state on the next round

---
name: product-all-md-design
description: Orchestrate page-by-page design generation from the release overview or sitemap source by maintaining `product/design/_md-design-status.md`, selecting exactly one unfinished page each round, and applying `product-md-design` single-page rules to generate the next page's design JSON for Figma, Pencil, or both. Use when an AI agent such as Codex or Gemini CLI needs to work through the whole release sitemap safely without batch collisions.
---

# Product All MD Design

## Overview

This is an orchestration skill.

It does not replace `product-md-design`.
It schedules and tracks repeated single-page execution of `product-md-design` rules until every page in the release sitemap has been completed.

The orchestration loop is:

1. read the release overview source
2. parse the sitemap page generation table
3. initialize or update `product/design/_md-design-status.md`
4. select exactly one unfinished page
5. invoke or strictly follow `product-md-design` for that one page
6. write completion state back to the status file
7. stop this round
8. on the next invocation, continue with the next unfinished page

## Source Resolution

Preferred upstream source:

- `product/release/product-overview-release.md`

Fallback upstream source:

- `product/release/product-sitemap-release.md`

Use the fallback when the preferred overview file is absent in the project but the sitemap generation table is stored in the sitemap release file instead.

The required section is the release sitemap generation table, typically named or equivalent to:

- `Sitemap 页面生成总表`

If neither file exists, or the page generation table cannot be found reliably, do not start generation. Write or update the status file with a blocker note instead of guessing.

## Inputs And Outputs

Required inputs:

- the overview source file resolved by the rules above
- the page generation table inside that file
- the delegated single-page skill contract at `../product-md-design/SKILL.md`

Optional input:

- one explicit design spec under `design/<slug>/` or `design/<slug>/DESIGN.md`
- one explicit output target request in user language:
  - `figma`
  - `pencil`
  - both

Output root:

- `product/design/`

Status file:

- `product/design/_md-design-status.md`

Per invocation output rule:

- create or update the status file
- process exactly one unfinished page
- generate only that page's target JSON outputs through `product-md-design` rules

## Status File Format

The status file must be Markdown so human reviewers and multiple agents can both use it.

Keep the file deterministic and easy to parse.

Recommended structure:

```md
# MD Design Status

## Meta

- source_file: product/release/product-overview-release.md
- fallback_used: false
- output_target: figma
- total_pages: 24
- completed_pages: 3
- pending_pages: 21
- last_updated: 2026-05-14

## Queue

| 页面ID | 页面名称 | 页面级MD文件 | 状态 | 产物 |
|---|---|---|---|---|
| U-100 | 登录注册 | product/release/pages/010-user-login/index.md | done | product/design/010-user-login/index-figma.json |
| U-210 | 待办事项列表 | product/release/pages/030-user-todo-list/index.md | pending | |
```

Allowed status values:

- `pending`
- `in_progress`
- `done`
- `blocked`

If the current round fails, mark the row `blocked` and link the blocker artifact when one exists.

## Workflow

1. Resolve the overview source.
   - Try `product/release/product-overview-release.md` first.
   - If it does not exist, use `product/release/product-sitemap-release.md`.
   - Record in the status meta whether fallback was used.

2. Parse the sitemap page generation table.
   - Locate the machine-readable table that enumerates pages to generate.
   - Extract at minimum:
     - page ID
     - page name
     - page level or order when available
     - page Markdown path
     - generation order when available
   - Ignore rows that do not point to a page-level Markdown file under `product/release/pages/`.

3. Initialize or refresh the status file.
   - If `product/design/_md-design-status.md` does not exist, create it from the parsed queue.
   - If it exists, preserve completed rows and refresh only missing metadata or newly discovered pages.
   - Do not silently reset `done` rows back to `pending`.

4. Resolve the target output mode for this orchestration round.
   - If the user explicitly says `figma`, the delegated single-page run must request only Figma JSON.
   - If the user explicitly says `pencil`, the delegated single-page run must request only Pencil JSON.
   - If the user explicitly says both, request both.
   - If the user does not specify a target, default to both.

5. Select exactly one page.
   - Choose the first unfinished row by stable generation order.
   - Treat `pending` as selectable.
   - Treat `blocked` as non-selectable unless the user explicitly asks to retry blocked pages.
   - Treat `done` as complete and skip it.
   - Before generation, mark the selected row `in_progress`.

6. Delegate to `product-md-design`.
   - Read `../product-md-design/skill.yaml` and `../product-md-design/SKILL.md`.
   - Follow all of its single-page filtering, layout resolution, design-spec, spacing, icon, list-alignment, and state-example rules.
   - Pass only the selected page file for this round.
   - Never pass multiple page files.
   - Never bypass its layout validation or visual-only filtering.

7. Persist completion.
   - If generation succeeds, mark the row `done`.
   - Write the produced artifact paths into the `产物` column.
   - Update the status counters and `last_updated`.
   - If generation fails because of ambiguity or blockers, mark the row `blocked` and record the blocker path or short reason.

8. Stop after one page.
   - Do not continue to a second page in the same invocation.
   - The next invocation resumes from the next unfinished row.

## Regeneration Rules

- If the user explicitly asks to regenerate one page, allow selecting that page even if it is already `done`.
- If the user explicitly asks to regenerate all pages, do not do it in one run.
- Instead, reset or annotate the queue as requested and still process only one page in the current invocation.

## Hard Constraints

- This skill is an orchestrator, not a bulk generator.
- Never generate more than one page per invocation.
- Never ask `product-md-design` to process multiple source files.
- Never skip status-file creation or update.
- Never assume `product/release/product-overview-release.md` exists without checking.
- Never let Codex-specific or Gemini-specific behavior leak into the status schema or generated artifact schema.
- Never lose completed progress when the status file already exists.

## Compatibility

This skill must remain runner-neutral.

Codex, Gemini CLI, or any other agent must be able to:

- discover the same next page
- understand the same status file
- apply the same single-page delegation boundary
- continue from the same completion state on the next round

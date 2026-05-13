---
name: product-page-draft
description: Generate one page-level draft Markdown document at a time from product/release/product-sitemap-release.md, using exactly one row from the Sitemap 页面生成总表 and matching that row to the correct release layout-family file under product/release/layout. Use when an AI agent needs to create detailed page requirements under product/development/pages for a selected page while inheriting the right shell/navigation/responsive contract for that page's Surface and terminal shape.
---

# Product Page Draft

## Overview

Create a detailed draft MD for exactly one page listed in `product/release/product-sitemap-release.md` -> `Sitemap 页面生成总表`. This skill must also resolve and use the correct layout-family dependency under `product/release/layout/`, so page requirements inherit the confirmed product-level shell, navigation, page container, responsive, and global state rules for the target page's actual Surface and terminal shape. This skill is deliberately single-page-per-run to reduce context drift and keep page requirements accurate.

This skill is runner-neutral. Any AI system can use it by reading this file and the references under `references/`; platform-specific metadata belongs under `adapters/`.

## Required Input

- `product/release/product-sitemap-release.md`
- The matching layout release file under `product/release/layout`, such as `product-layout-release.md`, `product-layout-release-user-web.md`, or `product-layout-release-admin-web.md`
- A target page specified by the user as `页面ID`, `页面名称`, or table row.

If no matching `product-layout-release*.md` file exists, block generation and ask the user to run `product-layout-release` first. Do not fall back to draft layout files for normal generation, because page drafts must be based on confirmed layout rules.

If the user does not specify a target page:

1. Read `Sitemap 页面生成总表` and `页面生成队列`.
2. Check `product/development/pages`.
3. Select the first row by `生成顺序` whose page MD does not exist.
4. Generate only that one page.

## Output Path

Write one file under:

- the exact path from the target row's `页面级MD文件` column

Rules:

- The output file path must exactly match the `页面级MD文件` value in `Sitemap 页面生成总表`.
- If the `页面级MD文件` value is relative, resolve it from the project root.
- If the `页面级MD文件` value is missing, block generation and ask the user to complete that sitemap row instead of inventing a filename.
- The document H1 must exactly equal the page name.
- Never create page MD files for multiple sitemap rows in one invocation.

## Workflow

1. Load source context.
   - Read `product/release/product-sitemap-release.md`.
   - Locate `Sitemap 页面生成总表`.
   - Parse the target row and nearby hierarchy context: parent page, child pages, dependencies, role, function, key operations, key data/content, states, rules, and upstream/downstream pages.
   - Resolve the correct layout release file before reading layout rules:
     - Read all candidate files matching `product/release/layout/product-layout-release*.md`.
     - Prefer the file whose section `0. 文档状态` metadata exactly matches the target row's `Surface` and terminal shape via `Layout Key`, `适用 Surface`, `适用端形态`, and `覆盖页面ID / 页面级MD文件范围`.
     - Use `Surface 划分`, `分 Surface 层级清单`, `Surface` 列、产品形态描述、以及页面级MD文件路径样例来判断 terminal shape.
     - If exactly one file matches, use it.
     - If only `product-layout-release.md` exists, use it as the single-family layout contract.
     - If multiple candidate files exist but none or more than one match, block generation and report the ambiguous files rather than guessing.
   - Read the matched layout file.
   - Parse the target page's layout mapping from the matched `product-layout-release*.md`: Surface ID, Shell type, page template, navigation position, child presentation, global region inheritance, responsive rules, global state containers, route behavior, and role/permission layout effects.
   - Read only the source context needed for the selected page. Do not expand into unrelated pages.

2. Build the page model.
   - Identify page purpose, user goal, entry points, exit points, role/permission requirements, global page attributes, and data dependencies.
   - Apply the inherited layout contract before defining elements: app/web shell, top nav, side nav, bottom tabs, breadcrumb, main scroll region, fixed action area, modal/drawer layer, safe-area behavior, and responsive container rules.
   - If the release layout lacks a material page-level detail, infer the detail only when practical and mark it with a page-level `PA-*` or `PQ-*` ID. Do not introduce or reuse layout-level `LA-*` / `LQ-*` IDs in page drafts.
   - Enumerate all visible and functional elements: buttons, links, icons, tabs, segmented controls, dropdowns, selects, search boxes, filters, forms, inputs, textareas, radio groups, checkboxes, toggles, sliders, steppers, uploaders, date/time pickers, tables, lists, cards, banners, alerts, modals, drawers, toasts, tooltips, charts, images, audio/video, file previews, loading indicators, empty states, navigation bars, tab bars, and any product-specific widgets.
   - Include inferred elements when the release overview is incomplete, but mark them with page-level assumption or confirmation IDs.

3. Define states and behavior.
   - For the page and every important element, define default, loading, disabled, enabled, focused, selected, hover/pressed when applicable, success, warning, error, empty, permission-denied, offline/network-error, quota-limited, payment-required, and backend-returned states.
   - For each state, list trigger conditions, visual/style definition, available actions, blocked actions, copy/messaging, and recovery path.
   - State triggers may come from global page properties, local form values, validation, permission, membership, quota, device capability, route parameters, or API response fields.

4. Define interactions and APIs.
   - For each element action, describe trigger, precondition, validation, API call or local action, request JSON, response JSON, success effect, failure effect, analytics event ID, analytics event name, tracking requirement, and navigation result.
   - Create an analytics event section for every page. This section is mandatory, even for simple pages.
   - Define event IDs, event names, trigger timing, counted occurrences, event properties, deduplication rules, upload timing, success/failure attribution, and funnel relationship.
   - Include analytics events for page exposure, key clicks, form submit attempts, validation failures, API success/failure, media interactions, payment/quota prompts, navigation exits, and other page-specific business actions when relevant.
   - At minimum, every page must include one page exposure event (`EVT-001` or equivalent). Every primary action and every API-triggering action must have an `EVT-*` event ID. Secondary actions that are intentionally not tracked must state `不埋点` and provide a reason.
   - If an API is not described in the overview, infer a practical contract and mark it with `PA-*` or `PQ-*`.
   - Include shared data structures and per-element data binding.

5. Draft the MD.
   - Use `references/page-draft-template.md`.
   - Include a Mermaid `mindmap` or `flowchart` that mirrors the page structure and interactions.
   - Use tables for element inventory, state matrix, action matrix, analytics events, API contracts, data structures, and edge cases.
   - Put all page-level assumptions/open questions in a unified list near the end of the document.
   - Append `## 12. 用户补充描述` as the final section of the page draft. This section is intentionally blank except for concise instructions and a fenced placeholder area where the user can write natural-language modifications to this specific page before release.
   - State that `product-page-release` must analyze and apply the user's supplement when generating the release page.

6. Verify and save.
   - Ensure the parent directory of the row's `页面级MD文件` exists.
   - Write exactly one page MD file.
   - Run the checklist in `references/page-draft-quality-checklist.md` before finishing.

## Page-Level Assumption IDs

Use page-scoped IDs so the page draft can later be converted into a release page document:

- Page assumptions: `PA-001`, `PA-002`, ...
- Page confirmation questions: `PQ-001`, `PQ-002`, ...
- Mark inline content as `（页面假设 PA-001：...）` or `（页面待确认 PQ-001：...）`.
- Every `PA-*` and `PQ-*` used anywhere in the page MD must appear in `页面假设与待确认统一清单`.
- Reuse the same ID when the same uncertainty appears in multiple tables.

## Hard Rules

- Generate only one page per invocation.
- Every generated page must end with `## 12. 用户补充描述`.
- The `用户补充描述` section must be page-specific, editable by the user, and must not contain generated product requirements unless they are instructions telling the user where to write their supplement.
- Every generated page must include `## 7. 埋点事件统计设计`.
- Every generated page must include at least one `EVT-*` analytics event ID.
- The `交互 Action 与执行效果` table must include `埋点事件ID`, `埋点事件名称`, and `不埋点原因` columns.
- Do not mark analytics as optional. If an action is not tracked, explain why in `不埋点原因`.
- Do not skip element details because the overview is high level; infer practical details and mark uncertainty.
- Do not produce only prose. The output must contain Mermaid plus detailed tables.
- Do not generate UI implementation code.
- Do not alter `product/release/product-sitemap-release.md`.
- Do not alter any `product/release/layout/product-layout-release*.md` file.
- Do not ignore layout-family matching; page shell and layout inheritance must come from the matched release layout file, not from an arbitrary sibling layout file.
- Do not create release-level page MD files; this skill only creates draft page MD files.
- Do not use vague element names such as `按钮1` or `表单项`. Name elements by purpose, for example `发送验证码按钮`, `岗位方向下拉选择器`, `PDF 导出主按钮`.

## Resources

- `references/page-draft-template.md`: required output structure.
- `references/page-draft-quality-checklist.md`: verification checklist.

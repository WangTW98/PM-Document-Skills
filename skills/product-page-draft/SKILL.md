---
name: product-page-draft
description: Generate exactly one page bundle at a time from product/release/product-sitemap-release.md by using exactly one row from the Sitemap 页面生成总表, resolving the correct release layout file for that row, and writing the page under a canonical page directory with index.md plus optional overlay sub-page docs. Use when an AI agent needs to create detailed page requirements under product/development/pages for one selected sitemap page while inheriting the correct confirmed release layout contract.
---

# Product Page Draft

## Overview

Create a detailed draft page bundle for exactly one page listed in `product/release/product-sitemap-release.md` -> `Sitemap 页面生成总表`.

One sitemap row still means one page. But one page may produce multiple Markdown files inside one page directory:

- the main page file: `index.md`
- zero or more overlay sub-page files such as `弹窗（确认）-申请退款.md` or `模态窗（抽屉）-材料审核.md`

The sitemap row's `页面级MD文件` is no longer treated as the final single-file output path. It is the canonical page key used to derive the output directory, locate the matching release layout family, and keep all page-bundle files grouped together.

This skill is runner-neutral. Any AI system can use it by reading this file and the references under `references/`; platform-specific metadata belongs under `adapters/`.

## Required Input

- `product/release/product-sitemap-release.md`
- The matching release layout file under `product/release/layout/`
- A target page specified by the user as `页面ID`, `页面名称`, or table row

Allowed release layout candidates:

- `product/release/layout/product-layout-release*.md`

Excluded from layout candidates:

- any `*.blockers.md`
- any non-Release draft or intermediate file

If no matching release layout file exists, block generation and ask the user to finish the corresponding `product-layout-release` output first. Do not fall back to draft layout files for normal generation.

If the user does not specify a target page:

1. Read `Sitemap 页面生成总表` and `页面生成队列`.
2. Check `product/development/pages`.
3. Select the first row by `生成顺序` whose page bundle main file does not exist.
4. Generate only that one page bundle.

## Output Path

For a target row whose `页面级MD文件` is:

- `product/development/pages/110-order-detail.md`

derive the page bundle root as:

- `product/development/pages/110-order-detail/`

and write:

- main page: `product/development/pages/110-order-detail/index.md`
- optional overlay docs: `product/development/pages/110-order-detail/<overlay-file>.md`

Rules:

- The sitemap row's `页面级MD文件` is the canonical page key and directory anchor, not the final page-bundle main file.
- Derive the bundle root by removing the trailing `.md` from `页面级MD文件`.
- If the `页面级MD文件` value is relative, resolve it from the project root before deriving the directory.
- If the `页面级MD文件` value is missing, block generation and ask the user to complete that sitemap row instead of inventing a path.
- The document H1 in `index.md` must exactly equal the page name.
- Never create bundles for multiple sitemap rows in one invocation.

## Overlay Sub-Page Rules

Inside one selected sitemap row, determine whether the page contains overlay-like sub-pages that should be split into separate Markdown files.

Common overlay examples:

- 确认弹窗
- 表单弹窗
- 结果弹窗
- 全屏模态窗
- 抽屉
- 侧滑面板
- 底部弹层
- 多步骤模态流程

Split an overlay into its own file only when at least one condition is true:

- it has its own user goal or task completion path
- it contains its own form, validation, upload, audit, payment, selection, or preview flow
- it triggers its own API or submission action
- it has meaningful independent states such as loading, invalid, submitting, success, reject, timeout, or permission-restricted
- its content is too large or too stateful to remain a single row inside the parent page tables

Do not split these into standalone overlay files unless they are unusually business-heavy:

- tooltip
- popover
- dropdown option panel
- date picker panel
- plain image lightbox
- toast
- snackbar
- one-line confirm prompt with no business state

If an overlay-like child is already represented by another explicit sitemap row in `Sitemap 页面生成总表`, do not duplicate it inside the parent page directory as another standalone file. The sitemap row remains the authoritative independent page. In the parent page draft, reference that child page row instead.

Naming rules for standalone overlay files:

- `弹窗（确认）-删除草稿.md`
- `弹窗（表单）-新增服务分类.md`
- `模态窗（抽屉）-退款审核.md`
- `模态窗（全屏）-批量导入预览.md`

Choose `弹窗` or `模态窗` according to the actual interaction style. The parenthesized type must be concrete and stable.

## Workflow

1. Load source context.
   - Read `product/release/product-sitemap-release.md`.
   - Locate `Sitemap 页面生成总表`.
   - Parse the target row and nearby hierarchy context: parent page, child pages, dependencies, role, function, key operations, key data/content, states, rules, and upstream/downstream pages.
   - Resolve the correct release layout file before reading layout rules:
     - Read candidate files matching `product/release/layout/product-layout-release*.md`.
     - Exclude any `*.blockers.md`.
     - Keep only files whose `0. 文档状态` confirms `文档版本 = Release`.
     - Prefer the file whose metadata exactly matches the target row's `Surface`, `页面ID`, and canonical `页面级MD文件` through `Layout Key`, `适用 Surface`, `适用端形态`, and `覆盖页面ID / 页面级MD文件范围`.
     - Use the sitemap row's `Surface`, `Layout 区域`, page ID prefix (`U-` / `M-`), `分 Surface 层级清单`, and the layout file's `Sitemap 到 Layout 映射总表` together for matching.
     - If exactly one release file matches, use it.
     - If no release file matches but a same-family `*.blockers.md` exists, block generation and cite that blocker file instead of guessing.
     - If multiple release files match, block generation and report the ambiguous files.
   - Read the matched release layout file.
   - Parse the target page's layout mapping from that layout file: Surface ID, Shell type, page template, navigation position, child presentation, global region inheritance, responsive rules, global state containers, route behavior, and role/permission layout effects.
   - Read only the source context needed for the selected page. Do not expand into unrelated pages.

2. Build the page bundle model.
   - Identify page purpose, user goal, entry points, exit points, role/permission requirements, global page attributes, and data dependencies.
   - Apply the inherited layout contract before defining elements: app/web shell, top nav, side nav, bottom tabs, breadcrumb, main scroll region, fixed action area, modal/drawer layer, safe-area behavior, and responsive container rules.
   - Decide whether the page bundle includes independent overlay sub-pages using the rules above.
   - For each retained overlay sub-page, assign a concrete filename and scope.
   - If the release layout lacks a material page-level detail, infer the detail only when practical and mark it with a page-level `PA-*` or `PQ-*` ID. Do not introduce or reuse layout-level `LA-*` / `LQ-*` IDs in page drafts.
   - Enumerate all visible and functional elements: buttons, links, icons, tabs, segmented controls, dropdowns, selects, search boxes, filters, forms, inputs, textareas, radio groups, checkboxes, toggles, sliders, steppers, uploaders, date/time pickers, tables, lists, cards, banners, alerts, modals, drawers, toasts, tooltips, charts, images, audio/video, file previews, loading indicators, empty states, navigation bars, tab bars, and any product-specific widgets.
   - Include inferred elements when the release overview is incomplete, but mark them with page-level assumption or confirmation IDs.

3. Define states and behavior.
   - For the page, each overlay sub-page, and every important element, define default, loading, disabled, enabled, focused, selected, hover/pressed when applicable, success, warning, error, empty, permission-denied, offline/network-error, quota-limited, payment-required, and backend-returned states.
   - For each state, list trigger conditions, visual/style definition, available actions, blocked actions, copy/messaging, and recovery path.
   - State triggers may come from global page properties, local form values, validation, permission, membership, quota, device capability, route parameters, or API response fields.

4. Define interactions and APIs.
   - For each element action, describe trigger, precondition, validation, API call or local action, request JSON, response JSON, success effect, failure effect, analytics event ID, analytics event name, tracking requirement, and navigation result.
   - Create an analytics event section for every main page bundle. This section is mandatory, even for simple pages.
   - Define event IDs, event names, trigger timing, counted occurrences, event properties, deduplication rules, upload timing, success/failure attribution, and funnel relationship.
   - Include analytics events for page exposure, key clicks, form submit attempts, validation failures, API success/failure, media interactions, payment/quota prompts, navigation exits, and other page-specific business actions when relevant.
   - At minimum, every main page must include one page exposure event (`EVT-001` or equivalent). Every primary action and every API-triggering action must have an `EVT-*` event ID. Secondary actions that are intentionally not tracked must state `不埋点` and provide a reason.
   - If an API is not described in the overview, infer a practical contract and mark it with `PA-*` or `PQ-*`.
   - Include shared data structures and per-element data binding.

5. Draft the main page and overlay files.
   - Use `references/page-draft-template.md` for the main `index.md`.
   - Include a Mermaid `mindmap` or `flowchart` that mirrors the page structure and interactions.
   - Use tables for element inventory, state matrix, action matrix, analytics events, API contracts, data structures, and edge cases.
   - Put all page-level assumptions/open questions in a unified list near the end of the main page document.
   - Append `## 12. 用户补充描述` as the final section of the main `index.md`. This section is intentionally blank except for concise instructions and a fenced placeholder area where the user can write natural-language modifications to this specific page before release.
   - State that `product-page-release` must analyze and apply the user's supplement when generating the release page.
   - For each standalone overlay file, create a focused page sub-spec under the same bundle root. It must define the overlay's goal, trigger source, layout style, elements, states, actions, APIs, analytics, and edge cases. Overlay files may be shorter than `index.md`, but must still be explicit and operational.
   - In the main `index.md`, list all generated standalone overlay files and how they are entered from the main page.

6. Verify and save.
   - Ensure the bundle root exists.
   - Write exactly one sitemap-row page bundle: one `index.md` plus zero or more overlay sub-page files for that same row.
   - Run the checklist in `references/page-draft-quality-checklist.md` before finishing.

## Page-Level Assumption IDs

Use page-scoped IDs so the page draft can later be converted into a release page document:

- Page assumptions: `PA-001`, `PA-002`, ...
- Page confirmation questions: `PQ-001`, `PQ-002`, ...
- Mark inline content as `（页面假设 PA-001：...）` or `（页面待确认 PQ-001：...）`.
- Every `PA-*` and `PQ-*` used anywhere in the page bundle must appear in the main `index.md` section `页面假设与待确认统一清单`.
- Reuse the same ID when the same uncertainty appears in multiple tables or overlay files.

## Hard Rules

- Generate only one sitemap row per invocation.
- A single selected sitemap row may write multiple files only inside its own page bundle directory.
- Every generated main page must end with `## 12. 用户补充描述`.
- The `用户补充描述` section must be page-specific, editable by the user, and must not contain generated product requirements unless they are instructions telling the user where to write their supplement.
- Every generated main page must include `## 7. 埋点事件统计设计`.
- Every generated main page must include at least one `EVT-*` analytics event ID.
- The `交互 Action 与执行效果` table must include `埋点事件ID`, `埋点事件名称`, and `不埋点原因` columns.
- Do not mark analytics as optional. If an action is not tracked, explain why in `不埋点原因`.
- Do not skip element details because the overview is high level; infer practical details and mark uncertainty.
- Do not produce only prose. The output must contain Mermaid plus detailed tables.
- Do not generate UI implementation code.
- Do not alter `product/release/product-sitemap-release.md`.
- Do not alter any `product/release/layout/product-layout-release*.md` or `*.blockers.md` file.
- Do not ignore layout-family matching; page shell and layout inheritance must come from the uniquely matched release layout file.
- Do not create release-level page MD files; this skill only creates draft page bundles.
- Do not use vague element names such as `按钮1` or `表单项`. Name elements by purpose, for example `发送验证码按钮`, `岗位方向下拉选择器`, `PDF 导出主按钮`.
- Do not treat the sitemap row's `页面级MD文件` as the final single-file output path.
- Do not duplicate a child overlay page that already exists as another sitemap row.

## Resources

- `references/page-draft-template.md`: required output structure for the main page file.
- `references/page-draft-quality-checklist.md`: verification checklist.

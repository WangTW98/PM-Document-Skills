---
name: product-page-draft-update
description: Update exactly one page draft bundle by merging all user-confirmed PA-/PQ- decisions and 用户补充描述 edits into the page body, while re-resolving the correct release layout file for the target sitemap row, then generating the next versioned main draft under the canonical page directory and keeping any auxiliary overlay docs aligned. Use when an AI agent needs to revise one page draft bundle without producing release pages.
---

# Product Page Draft Update

## Overview

Update exactly one page draft bundle after the user has edited confirmation rows or written `用户补充描述`.

One selected sitemap row still means one page bundle:

- main page draft: `.../<page-key>/index.md` or latest `index-vN.md`
- optional auxiliary overlay docs in the same directory

This skill never writes `product/release/pages`; it creates the next reviewable main draft version and keeps the same page bundle aligned.

The output must be a substantively regenerated bundle. It is invalid to copy the previous draft and only change the version number, timestamp, or filename.

## Inputs And Outputs

Required input:

- A single page bundle under `product/development/pages/<page-key>/`

Accepted user targeting forms:

- the page directory
- the bundle main file
- the sitemap row's canonical `页面级MD文件`
- page ID or page name that resolves to exactly one sitemap row

Batch input is not allowed. If multiple page bundles are provided, stop and ask the user to select exactly one.

Default output:

- next versioned main draft inside the same page directory, for example `index-v2.md`, `index-v3.md`
- updated auxiliary overlay docs in the same directory when those docs are affected by the merged changes

Versioning rule:

- versioning applies to the main bundle file
- auxiliary overlay docs keep stable canonical names unless the user explicitly asks to preserve historical copies

## Workflow

1. Select and read one source page bundle.
   - Use the bundle explicitly named by the user.
   - If the user names the bundle root or canonical `index.md` and versioned siblings exist, use the highest versioned main file unless the user explicitly says to use the named file exactly.
   - If the user specifies multiple bundles, do not process any file; ask the user to choose exactly one.
   - If the user does not specify a bundle, list candidate bundle roots under `product/development/pages` excluding status files and ask the user to choose one.
   - Read `skills/product-page-draft/references/page-draft-template.md` and use it as the required main-file output structure.
   - Load the selected bundle main file and all existing auxiliary overlay docs in the same directory.
   - Read `product/release/product-sitemap-release.md` and resolve the selected bundle's sitemap row by page ID, page name, or canonical `页面级MD文件`.
   - Resolve the correct release layout file using the same rules as `product-page-draft`: only consider `product-layout-release*.md`, exclude `*.blockers.md`, require Release metadata, prefer exact match on `Surface`, `页面ID`, and canonical `页面级MD文件`, and block on ambiguity.
   - Verify the selected main draft contains `## 7. 埋点事件统计设计`, `埋点事件ID`, and at least one `EVT-*` event ID. If not, block and ask for regeneration with `product-page-draft`.
   - Locate the final `页面假设与待确认统一清单`.
   - Parse every page assumption row (`PA-001`, `PA-002`, ...) and page confirmation row (`PQ-001`, `PQ-002`, ...).
   - Locate `## 12. 用户补充描述` and extract non-placeholder user-written content from the selected main draft.

2. Interpret user decisions and supplement.
   - Treat filled `用户确认状态`, `用户确认结果`, and `Release 处理方式` cells as user decisions that must be merged into the bundle body.
   - Treat non-empty `用户补充描述` as page modification instructions, not as notes to copy forward.
   - Group changes by affected area: page objective/context, layout sections, element inventory, state matrix, action matrix, analytics events, data model, API contract, edge/error handling, media/resources, and overlay sub-pages.
   - Keep layout-related sections aligned with the matched release layout file. User page edits may refine page-level behavior, but must not silently drift away from the matched shell/navigation contract.
   - If a decision or supplement is ambiguous but the page can still be updated, preserve the uncertainty as a new `PQ-*` row.
   - If a supplement introduces a new overlay-like sub-page, apply the overlay splitting rules from `product-page-draft`.
   - If a supplement removes the need for a previously generated overlay sub-page, remove its references from the main file and either delete the obsolete overlay doc or clearly mark it obsolete in the bundle, depending on the user's instruction.

3. Regenerate the page bundle.
   - Rebuild the main output according to `page-draft-template.md`; preserve the page H1, every required top-level section, subsection, table, Mermaid block, JSON example block, and final fenced `用户补充描述` block.
   - Keep the canonical section order: `0. 页面文档状态`, `1. 页面目标与上下文`, `2. 页面结构思维导图`, `3. 页面布局与内容区块`, `4. 页面元素清单`, `5. 元素状态矩阵`, `6. 交互 Action 与执行效果`, `7. 埋点事件统计设计`, `8. 数据结构与接口定义`, `9. 边界状态与错误处理`, `10. 多媒体与资源`, `11. 页面假设与待确认统一清单`, `12. 用户补充描述`.
   - If the source draft is missing a required template section or table, restore that section from the template and fill it from the merged content where possible.
   - Rewrite affected page sections from the source draft plus user decisions and supplement.
   - Reconcile every dependent section. For example, adding a button or a new overlay must update the element inventory, state matrix, action matrix, analytics events, data/API sections when applicable, edge cases, Mermaid diagram, and overlay file list.
   - Remove or mark as resolved old rows that have been fully applied.
   - Keep unresolved material assumptions/questions and add new ones caused by the merged changes.
   - Use stable unresolved IDs when their meaning is unchanged; add new IDs after the highest existing number. Do not reuse IDs for different meanings.
   - Rewrite affected auxiliary overlay docs so they remain consistent with the new main file.
   - Before saving, verify each actionable user decision or supplement instruction is reflected in the revised bundle. If no actionable user change exists, do not create a version-only draft; explain what user input is missing.

4. Preserve the review loop.
   - Keep the main page draft structure, including `页面级假设 / 待确认编号规则` when present.
   - Keep an updated `页面假设与待确认统一清单` in the main file with processing columns for user confirmation/results and Release handling.
   - Keep mandatory analytics content: `## 7. 埋点事件统计设计`, `埋点事件ID`, and `EVT-*` event IDs.
   - Keep `## 12. 用户补充描述` as the final section of the new main draft, reset to an empty editable placeholder.
   - Set document version to `Draft vN` and record the source main draft path when a metadata/version line exists.

5. Save and verify.
   - Save the next versioned main draft inside the same page directory.
   - If the source main file is `index.md`, write `index-v2.md`; if the source is `index-v2.md`, write `index-v3.md`.
   - Do not write anything under `product/release/pages`.
   - Verify the revised bundle still follows `page-draft-template.md`, has all required sections, has an updated page assumption/question list, mandatory analytics, and an empty final `用户补充描述` section.

## Hard Rules

- Process exactly one source page bundle.
- Do not create release pages.
- Do not change the main page draft template structure, heading order, required tables, Mermaid block, or JSON example sections.
- Do not omit template chapters even if the corresponding content is unchanged or currently empty.
- Do not remove the confirmation workflow sections.
- Do not carry raw `用户补充描述` notes forward; merge them into the bundle and reset the section.
- Do not create a new draft whose only material difference is document version, timestamp, or filename.
- Do not invent user confirmation. Unclear material changes become new `PQ-*` rows.
- Analytics content and `EVT-*` IDs must remain present.
- Do not let the updated page draft drift away from the matched release layout file on shell, navigation position, or responsive contract without recording a page-level assumption/question.
- Do not treat the page bundle as a single-file document; update auxiliary overlay docs when required by the main draft changes.
- Do not duplicate an auxiliary overlay doc for a flow that already has its own sitemap row.

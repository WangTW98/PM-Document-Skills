---
name: product-page-draft-update
description: Update exactly one page-level draft Markdown document by merging all user-confirmed PA-/PQ- decisions and 用户补充描述 edits into the page body, then generating the next versioned draft under product/development/pages while preserving page assumptions, confirmation questions, analytics, and the final empty user supplement section. Use when an AI agent needs to revise a page draft without producing a product/release/pages file.
---

# Product Page Draft Update

## Overview

Update exactly one page draft after the user has edited confirmation rows or written `用户补充描述`. This skill never writes `product/release/pages`; it creates the next reviewable page draft version.

The output must be a substantively regenerated Markdown document. It is invalid to copy the previous draft and only change the version number, timestamp, or filename.

## Inputs And Outputs

Required input:

- A single draft page file under `product/development/pages/*.md`, including a versioned draft such as `product/development/pages/010-login-v2.md`.

Batch input is not allowed. If multiple draft page files are provided, stop and ask the user to select exactly one.

Default output:

- Next versioned draft beside the source page draft, for example `product/development/pages/010-login-v2.md`, `product/development/pages/010-login-v3.md`.

## Workflow

1. Select and read one source page.
   - Use the page file explicitly named by the user.
   - If the user names a canonical page draft and versioned siblings exist, use the highest versioned sibling unless the user explicitly says to use the named file exactly.
   - If the user specifies multiple page files, do not process any file; ask the user to choose exactly one.
   - If the user does not specify a file, list candidate files under `product/development/pages` excluding status files and ask the user to choose one.
   - Read `skills/product-page-draft/references/page-draft-template.md` and use it as the required output structure.
   - Load the selected page MD.
   - Verify the draft contains `## 7. 埋点事件统计设计`, `埋点事件ID`, and at least one `EVT-*` event ID. If not, block and ask for regeneration with `product-page-draft`.
   - Locate the final `页面假设与待确认统一清单`.
   - Parse every page assumption row (`PA-001`, `PA-002`, ...) and page confirmation row (`PQ-001`, `PQ-002`, ...).
   - Locate `## 12. 用户补充描述` and extract non-placeholder user-written content.

2. Interpret user decisions and supplement.
   - Treat filled `用户确认状态`, `用户确认结果`, and `Release 处理方式` cells as user decisions that must be merged into the page body.
   - Treat non-empty `用户补充描述` as page modification instructions, not as notes to copy forward.
   - Group changes by affected area: page objective/context, layout sections, element inventory, state matrix, action matrix, analytics events, data model, API contract, edge/error handling, and media/resources.
   - If a decision or supplement is ambiguous but the page can still be updated, preserve the uncertainty as a new `PQ-*` row.

3. Regenerate the page draft body.
   - Rebuild the output according to `page-draft-template.md`; preserve the page H1, every required top-level section, subsection, table, Mermaid block, JSON example block, and final fenced `用户补充描述` block.
   - Keep the canonical section order: `0. 页面文档状态`, `1. 页面目标与上下文`, `2. 页面结构思维导图`, `3. 页面布局与内容区块`, `4. 页面元素清单`, `5. 元素状态矩阵`, `6. 交互 Action 与执行效果`, `7. 埋点事件统计设计`, `8. 数据结构与接口定义`, `9. 边界状态与错误处理`, `10. 多媒体与资源`, `11. 页面假设与待确认统一清单`, `12. 用户补充描述`.
   - If the source draft is missing a required template section or table, restore that section from the template and fill it from the merged content where possible.
   - Rewrite affected page sections from the source draft plus user decisions and supplement.
   - Reconcile every dependent section. For example, adding a button must update the element inventory, state matrix, action matrix, analytics events, data/API sections when applicable, edge cases, and Mermaid diagram.
   - Remove or mark as resolved old rows that have been fully applied.
   - Keep unresolved material assumptions/questions and add new ones caused by the merged changes.
   - Use stable unresolved IDs when their meaning is unchanged; add new IDs after the highest existing number. Do not reuse IDs for different meanings.
   - Before saving, verify each actionable user decision or supplement instruction is reflected in the revised page body. If no actionable user change exists, do not create a version-only draft; explain what user input is missing.

4. Preserve the review loop.
   - Keep the page draft structure, including `页面级假设 / 待确认编号规则` when present.
   - Keep an updated `页面假设与待确认统一清单` with processing columns for user confirmation/results and Release handling.
   - Keep mandatory analytics content: `## 7. 埋点事件统计设计`, `埋点事件ID`, and `EVT-*` event IDs.
   - Keep `## 12. 用户补充描述` as the final section, reset to an empty editable placeholder.
   - Set document version to `Draft vN` and record the source draft path when a metadata/version line exists.

5. Save and verify.
   - Save beside the source page draft using the next available version suffix.
   - If the source is `010-login.md`, write `010-login-v2.md`; if the source is `010-login-v2.md`, write `010-login-v3.md`.
   - Do not write anything under `product/release/pages`.
   - Verify the revised draft still follows `page-draft-template.md`, has all required sections, has an updated page assumption/question list, mandatory analytics, and an empty final `用户补充描述` section.

## Hard Rules

- Process exactly one source page.
- Do not create a release page.
- Do not change the page draft template structure, heading order, required tables, Mermaid block, or JSON example sections.
- Do not omit template chapters even if the corresponding content is unchanged or currently empty.
- Do not remove the confirmation workflow sections.
- Do not carry raw `用户补充描述` notes forward; merge them into the page body and reset the section.
- Do not create a new draft whose only material difference is document version, timestamp, or filename.
- Do not invent user confirmation. Unclear material changes become new `PQ-*` rows.
- Analytics content and `EVT-*` IDs must remain present.

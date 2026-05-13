---
name: product-layout-draft-update
description: Update a project-level layout draft Markdown document by merging all user-confirmed LA-/LQ- decisions and 用户补充描述 edits into the layout body, then generating the next versioned draft under product/development/layout while preserving layout assumptions, confirmation questions, and the final empty user supplement section. Use when an AI agent needs to revise a layout draft without producing product/release/layout/product-layout-release.md.
---

# Product Layout Draft Update

## Overview

Update the project-level layout draft after the user has edited confirmation rows or written `用户补充描述`. This skill never writes `product/release/layout/product-layout-release.md`; it creates the next reviewable layout draft version.

The output must be a substantively regenerated Markdown document. It is invalid to copy the previous draft and only change the version number, timestamp, or filename.

## Inputs And Outputs

Required input:

- `product/development/layout/product-layout-draft.md` or the latest versioned sibling such as `product/development/layout/product-layout-draft-v2.md`.

Default output:

- Next versioned draft beside the source draft, for example `product/development/layout/product-layout-draft-v2.md`, `product/development/layout/product-layout-draft-v3.md`.

## Workflow

1. Select and read the source draft.
   - If the user names a layout draft file, load that file.
   - If the user does not name a draft file, load the highest available versioned draft matching `product/development/layout/product-layout-draft-vN.md`; if none exists, load `product/development/layout/product-layout-draft.md`.
   - Locate the final `布局假设与待确认统一清单`.
   - Parse every layout assumption row (`LA-001`, `LA-002`, ...) and layout confirmation row (`LQ-001`, `LQ-002`, ...).
   - Locate `## 12. 用户补充描述` and extract non-placeholder user-written content.

2. Interpret user decisions and supplement.
   - Treat filled `用户确认状态`, `用户确认结果`, and `Release 处理方式` cells as user decisions that must be merged into the layout body.
   - Treat non-empty `用户补充描述` as layout modification instructions, not as notes to copy forward.
   - Group changes by affected area: Surface/Shell definitions, global shell regions, page template library, sitemap-to-layout mapping, navigation/route rules, global state layout, role/permission layout effects, responsive behavior, downstream usage rules, and Mermaid layout map.
   - If a decision or supplement is ambiguous but the draft can still be updated, preserve the uncertainty as a new `LQ-*` row.

3. Regenerate the layout draft body.
   - Rewrite affected layout sections from the source draft plus user decisions and supplement.
   - Reconcile every dependent section. For example, changing a Shell or navigation rule must update Surface/Shell definitions, global shell regions, sitemap-to-layout mapping, responsive behavior, downstream usage rules, and Mermaid layout map.
   - Remove or mark as resolved old rows that have been fully applied.
   - Keep unresolved material assumptions/questions and add new ones caused by the merged changes.
   - Use stable unresolved IDs when their meaning is unchanged; add new IDs after the highest existing number. Do not reuse IDs for different meanings.
   - Before saving, verify each actionable user decision or supplement instruction is reflected in the revised layout body. If no actionable user change exists, do not create a version-only draft; explain what user input is missing.

4. Preserve the review loop.
   - Keep the layout draft structure, including `Layout 假设 / 待确认编号规则` when present.
   - Keep an updated `布局假设与待确认统一清单` with processing columns for user confirmation/results and Release handling.
   - Keep `## 12. 用户补充描述` as the final section, reset to an empty editable placeholder.
   - Set document version to `Draft vN` and record the source draft path when a metadata/version line exists.

5. Save and verify.
   - Save beside the source draft using the next available version suffix.
   - If the source is `product-layout-draft.md`, write `product-layout-draft-v2.md`; if the source is `product-layout-draft-v2.md`, write `product-layout-draft-v3.md`.
   - Do not write anything under `product/release/layout`.
   - Verify the revised draft still has an updated layout assumption/question list and an empty final `用户补充描述` section.

## Hard Rules

- Do not create a release layout file.
- Do not remove the confirmation workflow sections.
- Do not carry raw `用户补充描述` notes forward; merge them into the layout body and reset the section.
- Do not create a new draft whose only material difference is document version, timestamp, or filename.
- Do not invent user confirmation. Unclear material changes become new `LQ-*` rows.
- Surface/Shell definitions, sitemap-to-layout mapping, responsive rules, downstream usage rules, and Mermaid layout map must remain consistent.

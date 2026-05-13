---
name: product-sitemap-draft-update
description: Update a product sitemap draft Markdown document by merging all user-confirmed assumption/question decisions and 用户补充描述 edits into the draft body, then generating the next versioned draft under product/development while preserving the review loop. Use when an AI agent needs to revise product/development/product-sitemap-draft.md or product-sitemap-draft-vN.md without producing a final release document.
---

# Product Sitemap Draft Update

## Overview

Update the product overview and sitemap draft after the user has edited confirmation rows or written `用户补充描述`. This skill never writes `product/release/product-sitemap-release.md`; it creates the next reviewable draft version.

The output must be a substantively regenerated Markdown document. It is invalid to copy the previous draft and only change the version number, timestamp, or filename.

## Inputs And Outputs

Required input:

- `product/development/product-sitemap-draft.md` or the latest versioned sibling such as `product/development/product-sitemap-draft-v2.md`.

Default output:

- Next versioned draft beside the source draft, for example `product/development/product-sitemap-draft-v2.md`, `product/development/product-sitemap-draft-v3.md`.

## Workflow

1. Select and read the source draft.
   - If the user names a draft file, load that file.
   - If the user does not name a draft file, load the highest available versioned draft matching `product/development/product-sitemap-draft-vN.md`; if none exists, load `product/development/product-sitemap-draft.md`.
   - Locate the final `假设与待确认统一清单`.
   - Parse every assumption row (`A-001`, `A-002`, ...) and confirmation row (`Q-001`, `Q-002`, ...).
   - Locate `## 6. 用户补充描述` and extract non-placeholder user-written content.

2. Interpret user decisions and supplement.
   - Treat filled `用户确认状态`, `用户确认结果`, and `Release 处理方式` cells as user decisions that must be merged into the draft body.
   - Treat non-empty `用户补充描述` as user modification instructions, not as notes to copy forward.
   - Group changes by affected area: product positioning, business goals, user paths, feature scope, role/permission rules, payment/subscription, notification/review/admin, sitemap rows, Mermaid hierarchy, page generation queue, dependencies, and source notes.
   - If a decision or supplement is ambiguous but the draft can still be updated, preserve the uncertainty as a new `Q-*` row.

3. Regenerate the draft body.
   - Rewrite affected product overview sections from the source draft plus user decisions and supplement.
   - Reconcile every dependent section. For example, adding/removing/renaming a page must update the overview scope, sitemap table, Mermaid hierarchy, page generation queue, dependencies, page-level Markdown paths, and role/permission references.
   - Remove or mark as resolved old rows that have been fully applied.
   - Keep unresolved material assumptions/questions and add new ones caused by the merged changes.
   - Use stable unresolved IDs when their meaning is unchanged; add new IDs after the highest existing number. Do not reuse IDs for different meanings.
   - Before saving, verify each actionable user decision or supplement instruction is reflected in the revised draft body. If no actionable user change exists, do not create a version-only draft; explain what user input is missing.

4. Preserve the review loop.
   - Keep the draft structure, including `假设 / 待确认编号规则` when present.
   - Keep an updated `假设与待确认统一清单` with processing columns for user confirmation/results and Release handling.
   - Keep `## 6. 用户补充描述` as the final section, reset to an empty editable placeholder.
   - Set document version to `Draft vN` and record the source draft path when a metadata/version line exists.

5. Save and verify.
   - Save beside the source draft using the next available version suffix.
   - If the source is `product-sitemap-draft.md`, write `product-sitemap-draft-v2.md`; if the source is `product-sitemap-draft-v2.md`, write `product-sitemap-draft-v3.md`.
   - Do not write anything under `product/release`.
   - Verify the revised draft still has an updated unified assumption/question list and an empty final `用户补充描述` section.

## Hard Rules

- Do not create a release document.
- Do not remove the confirmation workflow sections.
- Do not carry raw `用户补充描述` notes forward; merge them into the draft body and reset the section.
- Do not create a new draft whose only material difference is document version, timestamp, or filename.
- Do not invent user confirmation. Unclear material changes become new `Q-*` rows.
- The sitemap table, Mermaid hierarchy, page generation queue, dependencies, and page-level Markdown paths must remain consistent.

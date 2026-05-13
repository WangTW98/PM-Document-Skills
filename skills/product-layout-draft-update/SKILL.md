---
name: product-layout-draft-update
description: Update one or more project-level layout draft Markdown documents by merging all user-confirmed LA-/LQ- decisions and 用户补充描述 edits into the affected layout bodies, then generating the next versioned draft files under product/development/layout while preserving layout assumptions, confirmation questions, and the final empty user supplement section. Use when an AI agent needs to revise layout-family draft files without producing release layout files.
---

# Product Layout Draft Update

## Overview

Update one or more project-level layout drafts after the user has edited confirmation rows or written `用户补充描述`. This skill never writes `product/release/layout/product-layout-release*.md`; it creates the next reviewable layout draft version for each affected layout family.

The output must be a substantively regenerated Markdown document. It is invalid to copy the previous draft and only change the version number, timestamp, or filename.

## Inputs And Outputs

Required input:

- `product/release/product-sitemap-release.md`
- One or more current layout draft files under `product/development/layout`, including canonical or family-specific files such as `product-layout-draft.md`, `product-layout-draft-user-web.md`, `product-layout-draft-admin-web-v2.md`.

Default output:

- Next versioned draft beside each affected source draft, for example `product/development/layout/product-layout-draft-v2.md`, `product/development/layout/product-layout-draft-user-web-v2.md`, `product/development/layout/product-layout-draft-admin-web-v3.md`.

## Workflow

1. Select and read the source drafts.
   - Read `product/release/product-sitemap-release.md` first so layout-family scope is derived from the current confirmed product shape, not from stale filenames alone.
   - If the user names one or more layout draft files, load those files.
   - If the user does not name draft files, detect the active layout families from the current files under `product/development/layout`.
   - For each family, load the highest versioned sibling that matches the same base file, for example prefer `product-layout-draft-user-web-v3.md` over `product-layout-draft-user-web.md`.
   - If the product shape now requires a new layout family that does not yet have a draft file, create a new base draft file for that family using the naming rules from `product-layout-draft`.
   - Read `skills/product-layout-draft/references/product-layout-draft-template.md` and use it as the required output structure.
   - For each selected draft, locate the final `布局假设与待确认统一清单`.
   - Parse every layout assumption row (`LA-001`, `LA-002`, ...) and layout confirmation row (`LQ-001`, `LQ-002`, ...).
   - Locate `## 12. 用户补充描述` and extract non-placeholder user-written content.
   - Read section `0. 文档状态` metadata from each source draft: `Layout Key`, `适用 Surface`, `适用端形态`, and `覆盖页面ID / 页面级MD文件范围`.

2. Interpret user decisions, supplement, and family impact.
   - Treat filled `用户确认状态`, `用户确认结果`, and `Release 处理方式` cells as user decisions that must be merged into the layout body.
   - Treat non-empty `用户补充描述` as layout modification instructions, not as notes to copy forward.
   - Group changes by affected area: Surface/Shell definitions, global shell regions, page template library, sitemap-to-layout mapping, navigation/route rules, global state layout, role/permission layout effects, responsive behavior, downstream usage rules, and Mermaid layout map.
   - Decide which layout family or families each change touches. Cross-family shared changes must be applied consistently to every affected family; family-specific changes must only update the matching files.
   - If a decision or supplement is ambiguous but the draft can still be updated, preserve the uncertainty as a new `LQ-*` row in the affected family file.

3. Regenerate the affected layout draft bodies.
   - Rebuild each output according to `product-layout-draft-template.md`; preserve the document H1, every required top-level section, subsection, table, Mermaid block, and final fenced `用户补充描述` block.
   - Keep the canonical section order: `0. 文档状态`, `1. 产品布局总览`, `2. Layout 架构图`, `3. Surface 与 Shell 定义`, `4. 全局 Shell 区域规范`, `5. 页面模板库`, `6. Sitemap 到 Layout 映射总表`, `7. 导航与路由规则`, `8. 全局状态与边界 Layout`, `9. 角色与权限对 Layout 的影响`, `10. 下游 Skill 使用规则`, `11. 布局假设与待确认统一清单`, `12. 用户补充描述`.
   - If a source draft is missing a required template section or table, restore that section from the template and fill it from the merged content where possible.
   - Rewrite affected layout sections from the source draft plus user decisions and supplement.
   - Reconcile every dependent section. For example, changing a Shell or navigation rule must update Surface/Shell definitions, global shell regions, sitemap-to-layout mapping, responsive behavior, downstream usage rules, and Mermaid layout map.
   - Remove or mark as resolved old rows that have been fully applied.
   - Keep unresolved material assumptions/questions and add new ones caused by the merged changes.
   - Use stable unresolved IDs when their meaning is unchanged; add new IDs after the highest existing number inside the same family file. Do not reuse IDs for different meanings.
   - Keep section `0. 文档状态` metadata aligned with the file being regenerated.
   - Before saving, verify each actionable user decision or supplement instruction is reflected in the revised layout body. If no actionable user change exists for a family, do not create a version-only draft for that family.

4. Preserve the review loop.
   - Keep the layout draft structure, including `Layout 假设 / 待确认编号规则` when present.
   - Keep an updated `布局假设与待确认统一清单` with processing columns for user confirmation/results and Release handling.
   - Keep `## 12. 用户补充描述` as the final section, reset to an empty editable placeholder.
   - Set document version to `Draft vN` and record the source draft path when a metadata/version line exists.
   - Keep `Layout Key`, `适用 Surface`, `适用端形态`, and `覆盖页面ID / 页面级MD文件范围` accurate for each family.

5. Save and verify.
   - Save beside each source draft using the next available version suffix.
   - Canonical single-family rule: `product-layout-draft.md` -> `product-layout-draft-v2.md` -> `product-layout-draft-v3.md`.
   - Family-specific rule: `product-layout-draft-user-web.md` -> `product-layout-draft-user-web-v2.md` -> `product-layout-draft-user-web-v3.md`.
   - If a new family is introduced during update, create its initial base draft as `product-layout-draft-<layout-key>.md`.
   - Do not write anything under `product/release/layout`.
   - Verify every revised draft still follows `product-layout-draft-template.md`, has all required sections, has updated layout assumption/question lists, and has an empty final `用户补充描述` section.

## Hard Rules

- Do not create release layout files.
- Do not change the layout draft template structure, heading order, required tables, or Mermaid block.
- Do not omit template chapters even if the corresponding content is unchanged or currently empty.
- Do not remove the confirmation workflow sections.
- Do not carry raw `用户补充描述` notes forward; merge them into the layout body and reset the section.
- Do not create a new draft whose only material difference is document version, timestamp, or filename.
- Do not invent user confirmation. Unclear material changes become new `LQ-*` rows.
- Surface/Shell definitions, sitemap-to-layout mapping, responsive rules, downstream usage rules, and Mermaid layout map must remain consistent.
- Do not merge different layout families back into one draft file unless the current sitemap clearly shows the contracts have converged.
- Do not rename a layout family file without also updating its section `0. 文档状态` metadata.

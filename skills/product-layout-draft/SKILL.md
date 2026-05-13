---
name: product-layout-draft
description: "Create one or more project-level layout draft Markdown files under product/development/layout from product/release/product-sitemap-release.md. Use when an AI agent needs to determine how many distinct layout families the product has, split APP/web/admin/mini-program/client/backend surfaces into the minimum set of materially different layout contracts, and write clearly distinguishable draft files that downstream page skills can match back to the correct Surface and terminal shape."
---

# Product Layout Draft

## Overview

Create one or more project-level layout drafts for user review and confirmation. These drafts are later converted by `product-layout-release` into confirmed layout dependencies for explanatory Markdown generation, including `product-page-draft`, `product-page-mock-draft`, `product-page-design-release`, and similar page-level skills.

The output is one of these forms:

- Single layout family: `product/development/layout/product-layout-draft.md`
- Multiple layout families: one file per family, for example `product/development/layout/product-layout-draft-user-web.md`, `product/development/layout/product-layout-draft-admin-web.md`, `product/development/layout/product-layout-draft-mobile-app.md`

This layout draft is built from:

- `product/release/product-sitemap-release.md`
- The `Sitemap 页面生成总表`

If the release overview is incomplete, infer practical product layout conventions and mark them clearly as assumptions or confirmation items. The user will later edit/confirm this draft before `product-layout-release` generates the release layout document.

## Required Input

- `product/release/product-sitemap-release.md`

If the file is missing, stop and ask the user to generate the release overview first.

## Workflow

1. Read the product overview.
   - Load `product/release/product-sitemap-release.md`.
   - Extract product type, target users, roles, surfaces, page scope, function scope, core user paths, permissions, and global business constraints that affect layout.
   - Locate and parse `Sitemap 页面生成总表`.
   - For every sitemap row, extract `生成顺序`, `页面ID`, `父页面ID`, `层级`, `页面名称`, `页面类型`, `页面级MD文件`, `对应功能`, `关键操作`, `关键数据/内容`, `状态与边界`, `权限/规则`, and dependencies when present.

2. Identify application surfaces and layout families.
   - Group sitemap rows by product surface, such as mobile App, responsive web, PC web, mini-program, admin console, operations backend, SaaS dashboard, content platform, or AI tool workspace.
   - Split layouts by distinct layout family, not just by page count. Create a separate layout file whenever shell, navigation model, responsive strategy, safe-area rules, or route/presentation behavior differs materially.
   - Do not split only because of brand/domain duplication. If two domains share the same user web shell and responsive contract, keep them in one layout family.
   - Use the product overview, `Surface 划分`, `分 Surface 层级清单`, the `Surface` column, and explicit terminal statements such as APP / H5 / Web / 小程序 / 管理后台 to decide whether the product needs one or multiple layout families.
   - For each family, define which pages are root destinations, pushed/detail pages, modal/drawer pages, creation/edit flows, settings pages, auth/onboarding pages, and exceptional pages outside the main shell.

3. Name each layout family deterministically.
   - Each layout draft file must have a clear ASCII `layout-key` in kebab-case.
   - The `layout-key` must express terminal shape plus surface role when needed to distinguish files, for example `mobile-app`, `user-web`, `admin-web`, `merchant-miniapp`, `ops-backend`.
   - If two families would otherwise share the same terminal word, add the audience or surface role so filenames remain self-explanatory.
   - If no confident semantic slug can be derived, fall back to a stable surface-based key such as `surface-a`, `surface-b`, but only after checking that a clearer role-based key is not available.

4. Create each layout contract.
   - Define global shell regions: top navigation, side navigation, bottom tab bar, breadcrumb, content header, main scroll area, fixed action area, footer, notification area, modal/drawer layer, global search, user/account entry, and safe-area behavior.
   - Define page container templates by page type: list, detail, create/edit form, wizard, dashboard, report, profile/settings, auth/onboarding, payment/subscription, review/audit, media/upload, AI generation workspace, export/history.
   - Define layout inheritance from sitemap: which pages in the current family use which shell/template and which pages intentionally override it.
   - Define role/permission layout effects: hidden nav items, disabled entries, read-only mode, admin-only regions, audit-only regions, subscription/quota regions.
   - Define responsive/adaptive rules for the current family's mobile, tablet, desktop, wide desktop, and platform-specific states when relevant.
   - Define empty/loading/error/permission/offline layout treatment at the shell and page-container levels.

5. Handle incomplete overview.
   - Use `LA-001`, `LA-002`, ... for layout assumptions.
   - Use `LQ-001`, `LQ-002`, ... for layout confirmation questions.
   - Mark inline uncertainty as `（布局假设 LA-001：...）` or `（布局待确认 LQ-001：...）`.
   - Every `LA-*` and `LQ-*` used anywhere in the document must appear in the final `布局假设与待确认统一清单`.
   - Do not hide layout uncertainty inside definitive language.

6. Draft and save.
   - Use `references/product-layout-draft-template.md`.
   - Append `## 12. 用户补充描述` as the final section. Keep it editable and placeholder-only so the user can write natural-language changes to shell, navigation, page templates, responsive rules, global states, role/permission layout, or sitemap-to-layout mapping before release.
   - Ensure `product/development/layout` exists.
   - If there is exactly one layout family, write `product/development/layout/product-layout-draft.md`.
   - If there are multiple layout families, write one file per family as `product/development/layout/product-layout-draft-<layout-key>.md`.
   - In section `0. 文档状态`, fill `Layout Key`, `适用 Surface`, `适用端形态`, and `覆盖页面ID / 页面级MD文件范围` so downstream skills can resolve the correct file without guessing.
   - Run `references/product-layout-draft-quality-checklist.md` before finishing.

## Required Content

The layout draft must include:

- Product layout summary: application surfaces, global shell patterns, navigation model, responsive strategy, and layout principles.
- Sitemap-to-layout mapping: every sitemap row mapped to a layout surface, shell, page container template, global navigation state, parent-child behavior, responsive behavior, and output dependency.
- Shell and navigation contract: explicit rules for top nav, side nav, bottom tabs, breadcrumb, back behavior, fixed footer/action areas, safe areas, modals/drawers, and cross-surface transitions.
- Page template library: reusable page layout templates with regions, slots, default spacing, state containers, and common exceptions.
- Layout dependency rules for downstream skills: exactly how page draft, mock draft, and design release skills should apply this layout document.
- Layout identification metadata: `Layout Key`, `适用 Surface`, `适用端形态`, `覆盖页面ID / 页面级MD文件范围`, and a filename that clearly distinguishes this layout family from any sibling layout file.
- Mermaid layout map: a Markdown-previewable hierarchy showing surfaces, shells, root pages, child pages, and exceptional flows.
- Layout assumptions and open questions with stable IDs and final unified list.
- User supplement section: draft documents must end with `用户补充描述`; release generation must analyze and apply non-empty supplement content.

## Downstream Dependency Contract

This draft defines the structure that will be confirmed into one or more release layout files under `product/release/layout/`. Downstream explanatory Markdown skills must use the matched release layout file together with `product/release/product-sitemap-release.md`; they should not use these drafts as the formal layout dependency after release layout generation.

Rules for downstream skills:

- `product-page-draft` resolves the correct `product-layout-release*.md` file by matching the target sitemap row's `Surface` and terminal shape to the layout file metadata, then uses that matched file to determine page shell, navigation regions, layout slots, page container template, global states, and inherited layout constraints before defining page elements.
- `product-page-mock-draft` resolves the correct `product-layout-release*.md` file by the same matching rule and uses it to include visible shell/navigation copy, page title/breadcrumb/tab labels, global empty/loading/error shell copy, and layout-dependent content placeholders.
- `product-page-design-release` resolves the correct `product-layout-release*.md` file by the same matching rule and uses it to produce App Shell / Navigation Contract, Figma frame hierarchy, safe-area regions, scroll containers, fixed bars, responsive variants, and layout integrity audit.
- If a page-level source conflicts with the release layout contract, record the conflict and prefer the more specific confirmed source. If the missing or conflicting detail must still be inferred, mark a page-level assumption/question rather than reusing layout draft IDs.
- Page-level skills must not depend on `LA-*` or `LQ-*` IDs after the release layout file exists.

## Hard Rules

- Do not generate page-level requirements; this skill creates project-level layout architecture only.
- Do not create release layout files; this skill writes only draft files under `product/development/layout`.
- Do not omit the final `用户补充描述` section.
- Do not omit sitemap rows from the sitemap-to-layout mapping.
- Do not leave a surface without a shell definition.
- Do not leave root/child/navigation behavior implicit.
- Do not collapse materially different client/app/web/admin shells into one file just to keep a single output.
- Do not create multiple layout files when the shell/responsive contract is effectively the same.
- Do not use ambiguous sibling filenames like `product-layout-draft-2.md`; every multi-layout filename must include a meaningful `layout-key`.
- Do not invent unmarked layout choices when the overview is incomplete; mark them with `LA-*` or `LQ-*`.
- Do not define visual design tokens, brand palettes, or Figma-specific styling in detail; reserve visual styling for design-system and page-design skills. This skill defines layout structure, not final visual skin.

## Resources

- `references/product-layout-draft-template.md`: required output structure.
- `references/product-layout-draft-quality-checklist.md`: verification checklist.

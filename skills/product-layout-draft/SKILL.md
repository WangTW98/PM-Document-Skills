---
name: product-layout-draft
description: "Create product/development/layout/product-layout-draft.md from product/release/product-overview-release.md. Use when an AI agent needs a project-level application layout draft based on the release overview and Sitemap 页面生成总表, covering APP/web/admin/mini-program surfaces, global shell, navigation, page containers, shared regions, responsive behavior, layout inheritance, and layout assumptions/open questions that product-layout-release will convert into the formal downstream layout contract."
---

# Product Layout Draft

## Overview

Create a project-level layout draft for user review and confirmation. This draft is later converted by `product-layout-release` into the confirmed shared layout dependency for explanatory Markdown generation, including `product-page-draft`, `product-page-mock-draft`, `product-page-design-release`, and similar page-level skills.

The output is:

- `product/development/layout/product-layout-draft.md`

This layout draft is built from:

- `product/release/product-overview-release.md`
- The `Sitemap 页面生成总表`

If the release overview is incomplete, infer practical product layout conventions and mark them clearly as assumptions or confirmation items. The user will later edit/confirm this draft before `product-layout-release` generates the release layout document.

## Required Input

- `product/release/product-overview-release.md`

If the file is missing, stop and ask the user to generate the release overview first.

## Workflow

1. Read the product overview.
   - Load `product/release/product-overview-release.md`.
   - Extract product type, target users, roles, surfaces, page scope, function scope, core user paths, permissions, and global business constraints that affect layout.
   - Locate and parse `Sitemap 页面生成总表`.
   - For every sitemap row, extract `生成顺序`, `页面ID`, `父页面ID`, `层级`, `页面名称`, `页面类型`, `页面级MD文件`, `对应功能`, `关键操作`, `关键数据/内容`, `状态与边界`, `权限/规则`, and dependencies when present.

2. Identify application surfaces and layout families.
   - Group sitemap rows by product surface, such as mobile App, responsive web, PC web, mini-program, admin console, operations backend, SaaS dashboard, content platform, or AI tool workspace.
   - For each surface, define the global layout family: tabbed App, stack navigation App, web top nav, marketing site, dashboard sidebar, admin console shell, mini-program bottom tabs, split-pane workspace, modal-heavy tool, or another appropriate shell.
   - Define which pages are root destinations, pushed/detail pages, modal/drawer pages, creation/edit flows, settings pages, auth/onboarding pages, and exceptional pages outside the main shell.

3. Create the shared layout contract.
   - Define global shell regions: top navigation, side navigation, bottom tab bar, breadcrumb, content header, main scroll area, fixed action area, footer, notification area, modal/drawer layer, global search, user/account entry, and safe-area behavior.
   - Define page container templates by page type: list, detail, create/edit form, wizard, dashboard, report, profile/settings, auth/onboarding, payment/subscription, review/audit, media/upload, AI generation workspace, export/history.
   - Define layout inheritance from sitemap: which pages use which shell/template and which pages intentionally override it.
   - Define role/permission layout effects: hidden nav items, disabled entries, read-only mode, admin-only regions, audit-only regions, subscription/quota regions.
   - Define responsive/adaptive rules for mobile, tablet, desktop, wide desktop, and platform-specific surfaces.
   - Define empty/loading/error/permission/offline layout treatment at the shell and page-container levels.

4. Handle incomplete overview.
   - Use `LA-001`, `LA-002`, ... for layout assumptions.
   - Use `LQ-001`, `LQ-002`, ... for layout confirmation questions.
   - Mark inline uncertainty as `（布局假设 LA-001：...）` or `（布局待确认 LQ-001：...）`.
   - Every `LA-*` and `LQ-*` used anywhere in the document must appear in the final `布局假设与待确认统一清单`.
   - Do not hide layout uncertainty inside definitive language.

5. Draft and save.
   - Use `references/product-layout-draft-template.md`.
   - Ensure `product/development/layout` exists.
   - Write `product/development/layout/product-layout-draft.md`.
   - Run `references/product-layout-draft-quality-checklist.md` before finishing.

## Required Content

The layout draft must include:

- Product layout summary: application surfaces, global shell patterns, navigation model, responsive strategy, and layout principles.
- Sitemap-to-layout mapping: every sitemap row mapped to a layout surface, shell, page container template, global navigation state, parent-child behavior, responsive behavior, and output dependency.
- Shell and navigation contract: explicit rules for top nav, side nav, bottom tabs, breadcrumb, back behavior, fixed footer/action areas, safe areas, modals/drawers, and cross-surface transitions.
- Page template library: reusable page layout templates with regions, slots, default spacing, state containers, and common exceptions.
- Layout dependency rules for downstream skills: exactly how page draft, mock draft, and design release skills should apply this layout document.
- Mermaid layout map: a Markdown-previewable hierarchy showing surfaces, shells, root pages, child pages, and exceptional flows.
- Layout assumptions and open questions with stable IDs and final unified list.

## Downstream Dependency Contract

This draft defines the structure that will be confirmed into `product/release/layout/product-layout-release.md`. Downstream explanatory Markdown skills must use the release layout file together with `product/release/product-overview-release.md`; they should not use this draft as the formal layout dependency after release layout generation.

Rules for downstream skills:

- `product-page-draft` uses `product/release/layout/product-layout-release.md` to determine page shell, navigation regions, layout slots, page container template, global states, and inherited layout constraints before defining page elements.
- `product-page-mock-draft` uses `product/release/layout/product-layout-release.md` to include visible shell/navigation copy, page title/breadcrumb/tab labels, global empty/loading/error shell copy, and layout-dependent content placeholders.
- `product-page-design-release` uses `product/release/layout/product-layout-release.md` to produce App Shell / Navigation Contract, Figma frame hierarchy, safe-area regions, scroll containers, fixed bars, responsive variants, and layout integrity audit.
- If a page-level source conflicts with the release layout contract, record the conflict and prefer the more specific confirmed source. If the missing or conflicting detail must still be inferred, mark a page-level assumption/question rather than reusing layout draft IDs.
- Page-level skills must not depend on `LA-*` or `LQ-*` IDs after the release layout file exists.

## Hard Rules

- Do not generate page-level requirements; this skill creates project-level layout architecture only.
- Do not create release layout files; this skill writes only `product/development/layout/product-layout-draft.md`.
- Do not omit sitemap rows from the sitemap-to-layout mapping.
- Do not leave a surface without a shell definition.
- Do not leave root/child/navigation behavior implicit.
- Do not invent unmarked layout choices when the overview is incomplete; mark them with `LA-*` or `LQ-*`.
- Do not define visual design tokens, brand palettes, or Figma-specific styling in detail; reserve visual styling for design-system and page-design skills. This skill defines layout structure, not final visual skin.

## Resources

- `references/product-layout-draft-template.md`: required output structure.
- `references/product-layout-draft-quality-checklist.md`: verification checklist.

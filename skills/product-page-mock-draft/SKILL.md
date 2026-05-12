---
name: product-page-mock-draft
description: Create one content-only page mock draft Markdown document from a release page spec under product/release/pages. Use when an AI agent needs to define detailed display content and mock data for every visible page element, including text, images, audio/video, banners, form option copy, empty/loading/error-state copy, static versus dynamic content source labels, and page-level assumptions/open questions when the release page is incomplete. Outputs same-name mock draft files under product/development/mock and excludes interaction actions, analytics, tracking, and business execution logic.
---

# Product Page Mock Draft

## Overview

Create content-display mock data for exactly one release page. The output describes what the user sees on the page: copy, labels, options, placeholder text, media content, state-specific content, list/card/table sample records, and static/dynamic source labels. This skill must also use `product/release/layout/product-layout-release.md`, so visible shell/navigation content and layout-dependent content placeholders stay consistent with the confirmed project-level layout contract.

This skill is content-only. It must not define interaction execution, analytics events, API behavior, business rules, or implementation logic beyond identifying whether displayed content is static or dynamically sourced.

This skill is runner-neutral. Any AI system can use it by reading this file and the references under `references/`; platform-specific metadata belongs under `adapters/`.

## Inputs And Outputs

Required input:

- A single release page file under `product/release/pages/*.md`.
- Shared layout dependency: `product/release/layout/product-layout-release.md`.

Default output:

- `product/development/mock/<same-relative-page-filename>.md`

Examples:

- Input: `product/release/pages/020-home.md`
- Output: `product/development/mock/020-home.md`

If the user does not specify a page, list available files under `product/release/pages` excluding `_generation-status.md` and ask the user to choose one.

## Workflow

1. Select one release page.
   - Process exactly one release page per invocation.
   - If multiple files or a directory are provided, stop and ask the user to choose one page.
   - Preserve the source filename relative to `product/release/pages` when writing under `product/development/mock`.

2. Read page content requirements.
   - Load the selected release page MD.
   - Load `product/release/layout/product-layout-release.md`. If missing, ask the user to run `product-layout-release` first. Do not fall back to the draft layout for normal generation.
   - Extract page name, page purpose, role, and the following source tables exactly when present: `页面布局与内容区块`, `页面元素清单`, `元素状态矩阵`, and `交互 Action 与执行效果`.
   - Extract the selected page's Surface, Shell, page template, global regions, navigation labels, title/breadcrumb/tab behavior, responsive layout, route behavior, and role/permission layout effects from the release layout.
   - Build the mock by mapping each source row from those four tables into display-content rows. Do not skip source rows just because they lack explicit copy; infer display content and mark it with `MA-*` or `MQ-*`.
   - For `交互 Action 与执行效果`, extract only user-visible content implications such as button labels, confirmation copy, success/failure copy, disabled copy, toast/modal copy, navigation labels, and state-specific messages. Do not reproduce execution steps, API calls, analytics, or business processing logic.
   - Also use data model, media/resource table, and relevant boundary/error states only to enrich visible mock content.

3. Create display content.
   - For every visible element, define display text, placeholder, helper text, option labels, empty/error/loading/success copy, media description, banner content, sample list/table/card data, and accessibility text when relevant.
   - Include visible shell and layout-dependent content from the layout draft when relevant: navigation title, tab label, breadcrumb label, fixed footer/action text, global empty/loading/error shell copy, permission/quota/payment shell placeholders, and responsive-content fallback labels.
   - Mark every content item as `静态` or `动态`.
   - For dynamic content, state the source category only, such as `接口返回`, `用户资料`, `本地缓存`, `系统状态`, `权限状态`, `会员状态`, `上传文件`, or `生成结果`. Do not define API contracts or execution behavior.
   - Provide realistic mock values that match the product domain and page role.
   - Keep source traceability by referencing source `区块ID`, `元素ID`, `状态`, and `ActionID` in the mock tables.

4. Handle incomplete release pages.
   - If the release page does not fully describe display content, infer practical product content and mark it with page mock assumption IDs.
   - Use `MA-001`, `MA-002`, ... for mock assumptions.
   - Use `MQ-001`, `MQ-002`, ... for mock confirmation questions.
   - Every `MA-*` and `MQ-*` used anywhere in the mock file must appear in the final `Mock 假设与待确认统一清单`.

5. Draft and save.
   - Use `references/page-mock-draft-template.md`.
   - Ensure `product/development/mock` exists.
   - Write exactly one mock draft file.
   - Run `references/page-mock-quality-checklist.md` before finishing.

## Content Scope

Include content for:

- Navigation title, tab labels, breadcrumbs, section titles, body copy, helper copy, badges, tags, labels, placeholders, validation copy, and button text.
- Form controls: input placeholders, select/dropdown options, radio labels, checkbox labels, toggle labels, stepper labels, upload hints, date/time picker labels.
- Lists, tables, cards, reports, charts, summaries, and detail fields with realistic mock records.
- Media: image descriptions, image alt text, icons, illustrations, audio/video titles, captions, thumbnails, duration text, transcript/description when relevant.
- Banners, alerts, modals, drawers, toast copy, tooltip copy, empty states, loading states, disabled states, success states, error states, permission states, quota/payment states, offline states.

Exclude:

- Action execution logic.
- Analytics and tracking logic.
- API endpoints, request structures, response structures, retries, status codes, or backend behavior.
- Implementation code.
- Business rules unrelated to displayed content.

## Hard Rules

- Generate only one mock page per invocation.
- Output path must preserve the release page filename under `product/development/mock`.
- The mock page H1 must match the release page H1.
- Every element from the release page that displays user-visible content must appear in the mock content inventory.
- Every row in the release page's `页面布局与内容区块` must map to the mock `区块级内容描述`.
- Every row in the release page's `页面元素清单` must map to the mock `元素内容清单`.
- Every row in the release page's `元素状态矩阵` must map to the mock `状态文案与内容差异`.
- Every row in the release page's `交互 Action 与执行效果` must be reviewed for user-visible content; if it affects displayed copy, it must map to `Action 可见内容映射`.
- Every content row must include `内容来源类型` with value `静态` or `动态`.
- Dynamic content must include `动态来源说明`.
- Do not copy release page interaction, analytics, or API sections into the mock output.
- Do not ignore `product-layout-release`; visible shell/navigation/mock content must align with the confirmed project-level layout contract.
- If content is inferred, mark it with `MA-*` or `MQ-*`.

## Resources

- `references/page-mock-draft-template.md`: required output structure.
- `references/page-mock-quality-checklist.md`: verification checklist.

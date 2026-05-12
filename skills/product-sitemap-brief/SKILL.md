---
name: product-sitemap-brief
description: Analyze natural-language product ideas and uploaded source materials such as Word documents, PDFs, screenshots, images, mind maps, notes, PRDs, research files, or business briefs to produce a project-level Markdown product overview and detailed sitemap. Use when an AI agent needs to clarify product design requirements, infer missing scope with marked assumptions, define product type, target users, business goals, user paths, feature scope, roles and permissions, key operations, monetization/payment/notification/review/admin needs, and especially a layout-aware sitemap for later page-level skill generation. Draft outputs must end with 用户补充描述 for natural-language product/sitemap edits. Outputs draft or release Markdown files under product/development.
---

# Product Sitemap Brief

## Overview

Create a project-level product summary document from incomplete product intent and supporting files. The sitemap is the source of truth for later page-level skills, so make it complete, hierarchical, sequenced, and tied to concrete product functions. Each sitemap row must be detailed enough to become one page-level Markdown generation task.

This skill is runner-neutral. Any AI system can use it by reading this file and the template under `references/`; platform-specific metadata belongs under `adapters/`.

## Workflow

1. Gather inputs.
   - Read the user's natural-language request.
   - Inspect every provided or referenced file path when available.
   - For `.docx`, `.pdf`, spreadsheets, screenshots, images, whiteboard exports, or mind maps, extract visible text and structure before writing the brief. If a file cannot be read, record it as an input-gap confirmation item such as `（待确认 Q-xxx：无法读取某上传文件，需用户补充内容或重新上传）`.
   - Preserve source-specific clues such as headings, user roles, workflows, entities, pricing hints, approvals, notifications, and page names.

2. Build product assumptions before drafting.
   - Infer a coherent product shape when the input is incomplete.
   - Mark every material inference as `假设` or `待确认` with a stable ID; do not hide uncertainty inside definitive language.
   - Use `A-001`, `A-002`, ... for assumptions and `Q-001`, `Q-002`, ... for open questions / items requiring user confirmation.
   - When mentioning uncertainty inline, use explicit syntax such as `（假设 A-001：默认支持微信登录）` or `（待确认 Q-003：是否需要企业微信通知）`.
   - Prefer practical product conventions over abstract descriptions: roles, permissions, information architecture, CRUD operations, review flows, payment/subscription, notifications, analytics, and admin needs.
   - Expand the sitemap to include all naturally required page levels, including list/detail/create/edit/settings/audit/payment/notification/profile/admin/analytics pages when the product type implies them.
   - Do not ask follow-up questions unless the missing information would make the sitemap impossible or materially misleading.

3. Draft the Markdown document.
   - Use `references/product-overview-template.md` as the output structure.
   - Write in Chinese unless the user asks for another language.
   - Put the strongest detail in `Sitemap / 信息架构`; later page-level skills will rely on it.
   - Produce both a complete sitemap table and a Mermaid diagram that represents the same hierarchy.
   - Maintain a final `假设与待确认统一清单` section and ensure every inline `A-*` or `Q-*` reference appears there exactly once.
   - For draft output, append `## 6. 用户补充描述` as the final section. Keep it editable and placeholder-only so the user can write natural-language product, sitemap, role, permission, payment, notification, review, admin, or page-scope changes before release.
   - Use stable headings and lists so the user can manually edit the Markdown.

4. Save the result.
   - Ensure `product/development` exists in the current project.
   - For first-pass or incomplete inputs, write `product/development/product-overview-draft.md` unless the user gives a more specific product name or path.
   - If the user provides a confirmed draft and asks for a release version, write `product/development/product-overview-release.md`.
   - Never overwrite a user-edited file without first reading it and preserving confirmed content.

## Required Content

The document must include:

- Product overview: product type, positioning, target users, core business goals, core user paths, page scope, function scope, roles and permissions, key operations, and likely commercial/payment/subscription/notification/review/admin needs.
- Sitemap: primary layout shape and complete page hierarchy. For every page, subpage, tertiary page, and deeper page when needed, list the parent-child relationship, generation order, corresponding product function, target role, primary operations, key data/content, important states, dependencies, and suggested page-level Markdown output path.
- Sitemap visualization: a Mermaid `flowchart` or `mindmap` block that mirrors the sitemap table and can be previewed in Markdown tools that support Mermaid.
- Assumptions and open questions: explicitly label inferred or incomplete items as `假设` or `待确认` with stable IDs, and repeat all of them in the unified list.
- User supplement section: draft documents must end with `用户补充描述`; release generation must analyze and apply non-empty supplement content.
- Source notes: summarize which user descriptions and uploaded documents informed the draft.

## Assumption and Confirmation IDs

Use IDs so humans and downstream skills can reconcile draft and release documents.

- Assumptions use `A-001`, `A-002`, `A-003` in first appearance order.
- Confirmation questions use `Q-001`, `Q-002`, `Q-003` in first appearance order.
- IDs must be unique and stable within a document. Do not reuse an ID for different meaning.
- Every inline assumption or confirmation marker must also appear in the final unified list.
- Every row in sitemap tables that depends on an assumption or open question must reference the relevant IDs in `来源 / 假设 / 待确认`.
- If a later section repeats the same uncertainty, reference the same ID instead of creating a duplicate.
- If an item is confirmed by user input, mark its status as `已确认` in release documents; draft documents should use `待用户确认` unless the input clearly confirms it.

Recommended inline formats:

- `（假设 A-001：用户端采用手机号验证码登录）`
- `（待确认 Q-002：是否支持企业微信 / 飞书通知）`
- `来源 / 假设 / 待确认：来自上传 PRD；依赖 A-003；待确认 Q-004`

## Sitemap Standards

Treat the sitemap as a build plan, not a navigation sketch. The table is the canonical input for downstream page-level skills.

Required sitemap rules:

- Use a stable page ID for every row, for example `U-010`, `U-010-010`, `A-020-030`. IDs must express hierarchy or be paired with a clear `父页面ID`.
- Include a `父页面ID` column. Root pages use `ROOT` or the surface ID.
- Include a numeric `生成顺序` column so later automation can process rows in order.
- Include a `层级` column with values such as `Surface`, `L1`, `L2`, `L3`, `L4`.
- Include a `页面级MD文件` column with a suggested target such as `product/development/pages/010-home.md`.
- Include `页面类型`, `对应功能`, `关键操作`, `关键数据/内容`, `状态与边界`, `权限/规则`, `依赖页面/对象`, and `假设ID / 待确认ID`.
- In the `来源 / 假设 / 待确认` column, reference assumption and confirmation IDs instead of writing vague phrases such as `待确认`.
- Keep one page per table row. Do not combine multiple pages in one row.
- If a page contains tabs or major modes that require different requirements, model them as child pages or subviews with their own rows.
- Sort rows by surface, then navigation order, then parent-child order.
- Ensure the Mermaid visualization and the sitemap table contain the same pages and parent-child relationships.

Include layout context:

- `Layout`: the global shell pattern, such as marketing site, tabbed App, dashboard sidebar, admin console, mini-program bottom tabs, or hybrid public/private layout.
- `Surface`: product surface such as `官网`, `用户端 App`, `商家后台`, `平台管理后台`, `运营后台`, or `开发者控制台`.
- `主页面`: top-level destinations inside each surface.
- `二级页面 / 三级页面 / 更深层页面`: list, detail, create/edit, settings, audit, payment, notification, profile, analytics, and admin pages under each main page when applicable.

Prefer exhaustive but readable tables. If a product has multiple surfaces, provide one complete table across all surfaces and optionally add grouped subsections for readability.

## Sitemap Completeness Checklist

Before saving the document, verify:

- Every major function in `功能范围` has at least one sitemap row.
- Every role in `角色与权限` has an entry point and permission-bounded pages.
- Every CRUD entity has list/detail/create/edit/delete or a clear reason why some operations are absent.
- Every review, payment, subscription, notification, admin, analytics, import/export, and settings flow has pages or is explicitly marked out of scope.
- Each page row explains what the page is for, not only what it is named.
- Page order matches the user's likely workflow and navigation hierarchy.
- Parent-child relationships are machine-readable through `页面ID` and `父页面ID`.
- Mermaid hierarchy matches the table and does not introduce pages missing from the table.

## Handling Incomplete Inputs

Use a conservative product-manager lens:

- Infer common supporting pages and capabilities only when they naturally follow from the product type.
- Mark inferred items inline with `（假设 A-xxx：...）` and add the same ID to the final unified list.
- Mark unknown business rules, data sources, compliance requirements, payment methods, review criteria, and notification channels inline with `（待确认 Q-xxx：...）` and add the same ID to the final unified list.
- Separate "must-have MVP scope" from "future/optional scope" when the user's description mixes immediate and aspirational requirements.
- When the user gives only a broad product idea, generate a complete MVP sitemap using standard product patterns and mark inferred surfaces/pages with `假设 A-xxx`.

## Output Quality Bar

- Make the document actionable enough that another skill can generate page-level requirements from the sitemap without re-interviewing the user.
- Avoid vague page names such as `功能页` or `详情页` without saying what entity they serve.
- Avoid burying sitemap decisions in prose; the complete hierarchy must live in the canonical sitemap table.
- Keep confirmed facts and assumptions visually distinct.
- Keep the Markdown easy for the user to edit manually.
- Prefer specific page names such as `课程详情页`, `订单退款审核页`, `团队成员权限设置页`, not generic names such as `详情页`.
- The assumption list must let a user confirm or edit all assumptions and open questions without searching the full document.
- The final section must be `用户补充描述` so users can add free-form product and sitemap changes before release.

## Resources

- `references/product-overview-template.md`: use this as the required Markdown structure for generated product overview documents.

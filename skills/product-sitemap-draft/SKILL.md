---
name: product-sitemap-draft
description: Analyze natural-language product ideas and uploaded source materials such as Word documents, PDFs, screenshots, images, mind maps, notes, PRDs, research files, or business briefs to produce a project-level Markdown product overview and detailed sitemap. Use when an AI agent needs to clarify product design requirements, infer missing scope with marked assumptions, define product type, target users, business goals, user paths, feature scope, roles and permissions, key operations, monetization/payment/notification/review/admin needs, and especially a layout-aware sitemap for later page-level skill generation. Draft outputs must strictly follow references/product-overview-template.md section structure, end with 用户补充描述, and be saved as product/development/product-sitemap-draft.md.
---

# Product Sitemap Draft

## Goal

Read the user's product description and all available supporting files, then generate a draft product overview plus a complete page-generation sitemap.

This skill is for producing a structured draft document, not a free-form analysis note. The output is consumed by later page-level skills, so the sitemap table must be complete, stable, and machine-readable.

## Required Resources

Before drafting, read:

1. `references/product-overview-template.md`

Use that file as the output contract, not as loose inspiration.

## Required Output Contract

Write exactly one draft file:

- `product/development/product-sitemap-draft.md`

The output must satisfy all of the following:

- Follow the section order and heading hierarchy from `references/product-overview-template.md`.
- Keep the top-level sections `## 0` through `## 6` present and in the same order.
- Do not rename, merge, skip, or invent top-level sections unless the user explicitly asks for a schema change.
- Keep `## 6. 用户补充描述` as the final section.
- Keep the sitemap canonical in `### 2.3 Sitemap 页面生成总表`.
- Ensure Mermaid, the sitemap table, and the grouped page tree describe the same hierarchy.
- Mark every material inference as `假设 A-xxx` or `待确认 Q-xxx`.
- Ensure every `A-*` and `Q-*` appearing anywhere in正文、表格、Mermaid 附近说明、或列表中，都在 `## 5. 假设与待确认统一清单` 中 exactly once.

## Workflow

1. Gather inputs.
   - Read the user's natural-language request.
   - Read every referenced local file that is available.
   - If uploaded or referenced materials include `.docx`, `.pdf`, spreadsheets, screenshots, images, or mind maps, extract usable text and visible structure first.
   - If a source cannot be read, record that gap with a `Q-*` item instead of silently ignoring it.

2. Build a concrete product model.
   - Infer product type, surfaces, roles, entities, workflows, permissions, commercial model, notification needs, audit/review paths, and backend/admin scope.
   - Infer standard supporting pages when they are naturally required by the product type.
   - Separate confirmed facts from assumptions.
   - Use one stable meaning per `A-*` or `Q-*`; do not create duplicate IDs for the same uncertainty.

3. Fill the template section by section.
   - Start from the template headings, then fill every required subsection.
   - Prefer completing a template subsection with concise concrete content over adding extra prose elsewhere.
   - If a subsection is truly unknown, keep it and write a bounded placeholder with `Q-*` rather than deleting the subsection.
   - The strongest detail should live in section `2. Sitemap / 信息架构`, especially the canonical table in `2.3`.

4. Build the sitemap as a downstream task list.
   - One page or one meaningful subview per row.
   - Use stable `页面ID`, `父页面ID`, `层级`, and `生成顺序`.
   - Include `页面级MD文件` for every row.
   - Model important tabs, modal-like subviews, drawers, wizards, approvals, create/edit pages, settings, payment, audit, and admin pages as separate rows when they need separate requirements.
   - Use `来源 / 假设ID / 待确认ID` to link each row back to evidence or uncertainty.

5. Validate before saving.
   - Check that all template headings still exist and remain ordered.
   - Check that every major function has at least one sitemap row.
   - Check that every role has an entry point and bounded pages.
   - Check that every inline `A-*` / `Q-*` appears once in section `5`.
   - Check that `2.2 Mermaid`, `2.3 页面生成总表`, `2.4 分 Surface 层级清单`, and `2.5 页面生成队列` are mutually consistent.

## Strict Rules

- Do not output a narrative summary in place of the template.
- Do not collapse the sitemap into only `用户端 / 平台端 / 系统支撑` blocks without converting it into the template's canonical page table.
- Do not use vague rows such as `详情页`, `管理页`, `功能页` without naming the entity or workflow they serve.
- Do not write `待确认` without a `Q-*` ID.
- Do not write `假设` without an `A-*` ID.
- Do not omit `页面级MD文件`, even when the exact filename is provisional.
- Do not put important page scope only in Mermaid or prose; it must exist in the canonical sitemap table.
- Do not overwrite an existing user-edited draft without first reading and preserving confirmed content.

## Handling Incomplete Inputs

When inputs are incomplete, produce a useful MVP draft anyway:

- Infer standard pages and flows conservatively.
- Mark inferred product choices with `A-*`.
- Mark missing business rules, data rules, payment rules, review rules, notification channels, tenant boundaries, or integration details with `Q-*`.
- Distinguish MVP scope from later scope when the source mixes both.

The skill should stop and ask for clarification only when the missing information would make the sitemap materially misleading across most of the document.

## Minimal Quality Bar

The draft is acceptable only if:

- Another skill can use section `2.3 Sitemap 页面生成总表` directly as a page-generation queue.
- The document still looks like the template, not like an ad hoc PRD.
- The user can confirm all assumptions and open questions from section `5` without searching the rest of the document.
- The final section is still `## 6. 用户补充描述` and remains editable.

## Resources

- `references/product-overview-template.md`: required output structure and section contract.

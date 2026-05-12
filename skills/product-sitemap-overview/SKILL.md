---
name: product-sitemap-overview
description: Generate a release-ready product overview Markdown document from product/development/product-overview-draft.md created by product-sitemap-brief. Use when an AI agent needs to apply the user's Release handling decisions from the draft's assumption and confirmation list, analyze and apply the draft's final 用户补充描述 natural-language product/sitemap edits, remove all assumptions, open questions, A-/Q- references, draft-only notes, supplement sections, and uncertain language, then write a confirmed release document under product/release.
---

# Product Sitemap Overview Release

## Overview

Create the release version of a product overview document from `product/development/product-overview-draft.md`. The release document is treated as confirmed product source material for downstream skills, so it must not contain unresolved assumptions, open questions, `A-*` / `Q-*` IDs, the draft `用户补充描述` section, or wording that asks the user to confirm scope.

This skill is runner-neutral. Any AI system can use it by reading this file and the references under `references/`; platform-specific metadata belongs under `adapters/`.

## Inputs And Outputs

Required input:

- `product/development/product-overview-draft.md`

Default output:

- `product/release/product-overview-release.md`

Optional blocker output when release cannot be produced:

- `product/release/product-overview-release-blockers.md`

## Workflow

1. Read the draft.
   - Load `product/development/product-overview-draft.md`.
   - Locate the final `假设与待确认统一清单` section.
   - Parse every assumption row (`A-001`, `A-002`, ...) and confirmation row (`Q-001`, `Q-002`, ...).
   - Also scan the whole draft for inline `A-*` and `Q-*` references; every reference must be represented in the final list.
   - Locate `## 6. 用户补充描述` when present.
   - Extract the user-written natural-language supplement. Treat empty content, `无`, `none`, or placeholder-only text as no supplement.

2. Validate release readiness.
   - Every `A-*` item must have a usable `用户确认状态` and `Release 处理方式`.
   - Every `Q-*` item must have a usable `用户确认结果` and `Release 处理方式`.
   - Treat values like `待用户确认`, blank cells, `待确认`, `保留为风险`, or vague text without a concrete decision as unresolved.
   - If any unresolved item affects product scope, sitemap, permissions, payment, notification, review, admin, or page generation, do not create the release document. Write `product/release/product-overview-release-blockers.md` with the blocking rows and the exact fields that need user input.
   - If `用户补充描述` contains ambiguous or contradictory instructions that materially affect product scope, sitemap hierarchy, page generation paths, roles, permissions, payment, notification, review, admin, or downstream page generation, block release and write the blocker file with the conflicting supplement items.

3. Analyze and apply user supplement.
   - Treat non-empty `用户补充描述` content as user-confirmed release modification instructions for the product overview and sitemap.
   - Convert the supplement into concrete changes grouped by affected area: product positioning, business goals, user paths, feature scope, role/permission rules, commercialization/payment/subscription, notification/review/admin, sitemap rows, Mermaid hierarchy, page generation queue, page dependencies, and source notes.
   - Apply supplement changes to all dependent sections, not just one paragraph. For example, adding a page must update the sitemap table, Mermaid hierarchy, page generation queue, dependencies, and any role/permission references.
   - If a supplement instruction conflicts with an `A-*` / `Q-*` release handling row, prefer the more specific user supplement only when it is concrete and clearly refers to the same product item. If the conflict changes sitemap, permissions, payment, or downstream page generation and cannot be resolved confidently, block release.
   - Do not carry the `用户补充描述` section or raw notes into the release file.

4. Apply release handling decisions.
   - `确认为正式内容` / `写入正式内容`: rewrite the related draft content as confirmed product content and remove the ID marker.
   - `删除` / `不需要`: remove the related feature, page row, role, operation, business rule, or note. Also remove dependent child pages if they no longer have a valid parent.
   - `按用户修改替换` / `改为：...`: replace the draft content with the user's confirmed wording.
   - `已否定`: remove or replace every affected mention; do not keep it as a note.
   - If multiple decisions conflict, prefer the more specific item and block release if the conflict changes sitemap or core requirements.

5. Rewrite the document as release content.
   - Set document version to `Release`.
   - Preserve confirmed product overview sections, sitemap tables, Mermaid visualization, page generation queue, dependencies, and source notes when still relevant.
   - Remove the draft-only `假设 / 待确认编号规则`, `假设与待确认统一清单`, `用户补充描述`, and `Release 生成前检查` sections.
   - Remove every `A-001`, `Q-001`, `假设`, `待确认`, `待用户确认`, `置信度`, and uncertainty marker from the release document.
   - Reconcile sitemap after removals or replacements: page IDs, parent IDs, generation order, Mermaid hierarchy, surface tree, and page generation queue must still match.

6. Save and verify.
   - Ensure `product/release` exists.
   - Write `product/release/product-overview-release.md`.
   - Run the release quality checklist in `references/release-quality-checklist.md` manually before finishing.

## Release Rules

- The release document is not a changelog. It is the confirmed version of the product overview.
- Do not include a section explaining which assumptions were removed.
- Do not include unresolved risks, questions, or TODOs.
- Do not include the draft `用户补充描述` section in the release document; apply it into confirmed content or block if ambiguous.
- Do not keep A-/Q- IDs for traceability inside the release file; traceability belongs in the draft.
- Do not silently keep a feature whose release handling says to delete it.
- Do not leave orphan sitemap rows after deleting a parent page.
- Do not invent user confirmation. If the draft does not contain a concrete release decision for a material item, block release.
- Do not ignore non-empty `用户补充描述`; it is part of the user's release input.

## Output Quality Bar

- The release file must be usable by downstream page-level skills without reading the draft.
- Sitemap rows must remain complete and machine-readable after applying release decisions.
- Mermaid visualization must match the final sitemap table.
- Page-level Markdown paths must remain unique.
- All product statements must read as confirmed requirements, not possibilities.
- Any non-empty user supplement from the draft must be reflected in the relevant release sections, with sitemap tables, Mermaid hierarchy, and page generation queue reconciled.

## Resources

- `references/release-quality-checklist.md`: final verification checklist for release output.

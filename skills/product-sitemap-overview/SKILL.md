---
name: product-sitemap-overview
description: Generate a release-ready product overview Markdown document from product/development/product-overview-draft.md created by product-sitemap-brief. Use when an AI agent needs to apply the user's Release handling decisions from the draft's final assumption and confirmation list, remove all assumptions, open questions, A-/Q- references, draft-only notes, and uncertain language, then write a confirmed release document under product/release.
---

# Product Sitemap Overview Release

## Overview

Create the release version of a product overview document from `product/development/product-overview-draft.md`. The release document is treated as confirmed product source material for downstream skills, so it must not contain unresolved assumptions, open questions, `A-*` / `Q-*` IDs, or wording that asks the user to confirm scope.

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

2. Validate release readiness.
   - Every `A-*` item must have a usable `用户确认状态` and `Release 处理方式`.
   - Every `Q-*` item must have a usable `用户确认结果` and `Release 处理方式`.
   - Treat values like `待用户确认`, blank cells, `待确认`, `保留为风险`, or vague text without a concrete decision as unresolved.
   - If any unresolved item affects product scope, sitemap, permissions, payment, notification, review, admin, or page generation, do not create the release document. Write `product/release/product-overview-release-blockers.md` with the blocking rows and the exact fields that need user input.

3. Apply release handling decisions.
   - `确认为正式内容` / `写入正式内容`: rewrite the related draft content as confirmed product content and remove the ID marker.
   - `删除` / `不需要`: remove the related feature, page row, role, operation, business rule, or note. Also remove dependent child pages if they no longer have a valid parent.
   - `按用户修改替换` / `改为：...`: replace the draft content with the user's confirmed wording.
   - `已否定`: remove or replace every affected mention; do not keep it as a note.
   - If multiple decisions conflict, prefer the more specific item and block release if the conflict changes sitemap or core requirements.

4. Rewrite the document as release content.
   - Set document version to `Release`.
   - Preserve confirmed product overview sections, sitemap tables, Mermaid visualization, page generation queue, dependencies, and source notes when still relevant.
   - Remove the draft-only `假设 / 待确认编号规则`, `假设与待确认统一清单`, and `Release 生成前检查` sections.
   - Remove every `A-001`, `Q-001`, `假设`, `待确认`, `待用户确认`, `置信度`, and uncertainty marker from the release document.
   - Reconcile sitemap after removals or replacements: page IDs, parent IDs, generation order, Mermaid hierarchy, surface tree, and page generation queue must still match.

5. Save and verify.
   - Ensure `product/release` exists.
   - Write `product/release/product-overview-release.md`.
   - Run the release quality checklist in `references/release-quality-checklist.md` manually before finishing.

## Release Rules

- The release document is not a changelog. It is the confirmed version of the product overview.
- Do not include a section explaining which assumptions were removed.
- Do not include unresolved risks, questions, or TODOs.
- Do not keep A-/Q- IDs for traceability inside the release file; traceability belongs in the draft.
- Do not silently keep a feature whose release handling says to delete it.
- Do not leave orphan sitemap rows after deleting a parent page.
- Do not invent user confirmation. If the draft does not contain a concrete release decision for a material item, block release.

## Output Quality Bar

- The release file must be usable by downstream page-level skills without reading the draft.
- Sitemap rows must remain complete and machine-readable after applying release decisions.
- Mermaid visualization must match the final sitemap table.
- Page-level Markdown paths must remain unique.
- All product statements must read as confirmed requirements, not possibilities.

## Resources

- `references/release-quality-checklist.md`: final verification checklist for release output.

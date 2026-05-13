---
name: product-sitemap-release
description: Process product/development/product-sitemap-draft.md created by product-sitemap-draft. By default, when the user has not explicitly asked for a final/release/正式版 document, apply completed assumption/question decisions and 用户补充描述 edits into a new versioned draft while preserving the assumption, confirmation, and empty supplement workflow for another review loop. Only when the user explicitly asks for the final/release/正式版 output, generate product/release/product-sitemap-release.md by applying all release handling decisions and removing draft-only uncertainty.
---

# Product Sitemap Release

## Overview

Process a product overview draft from `product/development/product-sitemap-draft.md`.

This skill has two modes:

- Draft revision mode, which is the default unless the user explicitly asks for a final/release/正式版 document.
- Final release mode, which keeps the existing release behavior and writes the confirmed product source material under `product/release`.

In draft revision mode, apply the user's completed edits from the final assumption/question list and `用户补充描述`, then write a new versioned draft that still contains the assumption/question list and an empty `用户补充描述` section for the next review loop. In final release mode, the release document must not contain unresolved assumptions, open questions, `A-*` / `Q-*` IDs, the draft `用户补充描述` section, or wording that asks the user to confirm scope.

This skill is runner-neutral. Any AI system can use it by reading this file and the references under `references/`; platform-specific metadata belongs under `adapters/`.

## Inputs And Outputs

Required input:

- `product/development/product-sitemap-draft.md` or the latest versioned sibling such as `product/development/product-sitemap-draft-v2.md`.

Default output when the user has not explicitly asked for final release:

- A versioned draft beside the source draft, using the next available suffix, for example `product/development/product-sitemap-draft-v2.md`, `product/development/product-sitemap-draft-v3.md`.

Final release output only when explicitly requested:

- `product/release/product-sitemap-release.md`

Optional blocker output when release cannot be produced:

- `product/release/product-sitemap-release-blockers.md`

## Workflow

0. Determine mode before writing any file.
   - Use final release mode only when the user explicitly asks to generate the final/release/正式版 document, publish the confirmed release, write under `product/release`, or remove all draft confirmation structures.
   - Use draft revision mode for requests such as "处理修改", "根据我填写的确认项更新", "继续完善", "修改 draft", or any request that does not clearly ask for final/release output.
   - If the request is ambiguous, choose draft revision mode. Do not create or overwrite `product/release/product-sitemap-release.md` without explicit final-release intent.

1. Read the draft.
   - If the user names a draft file, load that file.
   - If the user does not name a draft file, load the highest available versioned draft matching `product/development/product-sitemap-draft-vN.md`; if none exists, load `product/development/product-sitemap-draft.md`.
   - Treat the loaded file as the source draft for both draft revision and final release mode.
   - Locate the final `假设与待确认统一清单` section.
   - Parse every assumption row (`A-001`, `A-002`, ...) and confirmation row (`Q-001`, `Q-002`, ...).
   - Also scan the whole draft for inline `A-*` and `Q-*` references; every reference must be represented in the final list.
   - Locate `## 6. 用户补充描述` when present.
   - Extract the user-written natural-language supplement. Treat empty content, `无`, `none`, or placeholder-only text as no supplement.

2. Validate according to the selected mode.
   - In draft revision mode, unresolved `A-*` / `Q-*` items are allowed and should remain in the revised draft if still material.
   - In draft revision mode, completed decisions and concrete supplement edits must be applied; unresolved or newly exposed uncertainty must be represented as assumptions/questions in the revised draft, not as blockers unless the document cannot be updated coherently.
   - In draft revision mode, ambiguous or contradictory supplement items should become new `Q-*` confirmation rows when a reasonable draft can still be produced.
   - In final release mode, use the stricter readiness rules below.
   - Every `A-*` item must have a usable `用户确认状态` and `Release 处理方式`.
   - Every `Q-*` item must have a usable `用户确认结果` and `Release 处理方式`.
   - Treat values like `待用户确认`, blank cells, `待确认`, `保留为风险`, or vague text without a concrete decision as unresolved.
   - If any unresolved item affects product scope, sitemap, permissions, payment, notification, review, admin, or page generation, do not create the release document. Write `product/release/product-sitemap-release-blockers.md` with the blocking rows and the exact fields that need user input.
   - If `用户补充描述` contains ambiguous or contradictory instructions that materially affect product scope, sitemap hierarchy, page generation paths, roles, permissions, payment, notification, review, admin, or downstream page generation, block release and write the blocker file with the conflicting supplement items.

3. Analyze and apply user supplement.
   - Treat non-empty `用户补充描述` content as user-confirmed modification instructions for the product overview and sitemap.
   - Convert the supplement into concrete changes grouped by affected area: product positioning, business goals, user paths, feature scope, role/permission rules, commercialization/payment/subscription, notification/review/admin, sitemap rows, Mermaid hierarchy, page generation queue, page dependencies, and source notes.
   - Apply supplement changes to all dependent sections, not just one paragraph. For example, adding a page must update the sitemap table, Mermaid hierarchy, page generation queue, dependencies, and any role/permission references.
   - If a supplement instruction conflicts with an `A-*` / `Q-*` release handling row, prefer the more specific user supplement only when it is concrete and clearly refers to the same product item. If the conflict changes sitemap, permissions, payment, or downstream page generation and cannot be resolved confidently, block release.
   - In draft revision mode, do not carry raw supplement notes forward. Apply them into the draft content, then reset `用户补充描述` to an empty editable placeholder.
   - In final release mode, do not carry the `用户补充描述` section or raw notes into the release file.

4. Apply release handling decisions.
   - `确认为正式内容` / `写入正式内容`: rewrite the related draft content as confirmed product content and remove the ID marker.
   - `删除` / `不需要`: remove the related feature, page row, role, operation, business rule, or note. Also remove dependent child pages if they no longer have a valid parent.
   - `按用户修改替换` / `改为：...`: replace the draft content with the user's confirmed wording.
   - `已否定`: remove or replace every affected mention; do not keep it as a note.
   - If multiple decisions conflict, prefer the more specific item and block release if the conflict changes sitemap or core requirements.

5. Rewrite the document as release content when in final release mode.
   - Set document version to `Release`.
   - Preserve confirmed product overview sections, sitemap tables, Mermaid visualization, page generation queue, dependencies, and source notes when still relevant.
   - Remove the draft-only `假设 / 待确认编号规则`, `假设与待确认统一清单`, `用户补充描述`, and `Release 生成前检查` sections.
   - Remove every `A-001`, `Q-001`, `假设`, `待确认`, `待用户确认`, `置信度`, and uncertainty marker from the release document.
   - Reconcile sitemap after removals or replacements: page IDs, parent IDs, generation order, Mermaid hierarchy, surface tree, and page generation queue must still match.

6. Rewrite as a new review draft when in draft revision mode.
   - Preserve the draft structure, including `假设 / 待确认编号规则` when present, `假设与待确认统一清单`, `用户补充描述`, and release-check guidance.
   - Apply all completed user decisions and supplement edits into the relevant product overview, sitemap table, Mermaid hierarchy, page generation queue, dependencies, and source notes.
   - Remove or mark as resolved the old rows that have been fully applied. Keep unresolved material rows and add new assumptions/questions caused by the latest edits.
   - Keep the `假设与待确认统一清单` processing columns so the user can continue confirming: assumption/question ID, item, affected section, impact, user confirmation/result, and Release handling fields.
   - Use stable unresolved IDs when their meaning is unchanged; add new IDs after the highest existing number. Do not reuse IDs for different meanings.
   - Keep `## 6. 用户补充描述` as the final section, but reset it to an empty placeholder for the next user edit cycle.
   - Set document version to `Draft vN` and record the source draft path.
   - Save beside the source draft using the next available version suffix. If the source is `product-sitemap-draft.md`, write `product-sitemap-draft-v2.md`; if the source is already `product-sitemap-draft-v2.md`, write `product-sitemap-draft-v3.md`.
   - Do not write `product/release/product-sitemap-release.md` in draft revision mode.

7. Save and verify.
   - In final release mode, ensure `product/release` exists.
   - In final release mode, write `product/release/product-sitemap-release.md`.
   - In final release mode, run the release quality checklist in `references/release-quality-checklist.md` manually before finishing.
   - In draft revision mode, verify the revised draft still has the unified assumption/question list and an empty final `用户补充描述` section.

## Mode Rules

- Default to draft revision mode unless final-release intent is explicit.
- Draft revision mode is a loop: apply the user's completed decisions and supplement, then produce the next reviewable draft with remaining/new assumptions and questions.
- Final release mode is terminal: all material assumptions/questions must be resolved before writing `product/release/product-sitemap-release.md`.
- A request to "处理已修改内容", "继续修改", "生成新版 draft", or "根据补充描述更新" is not final-release intent.
- Phrases such as "生成最终版", "生成正式版", "生成 release", "输出到 product/release", or "去掉所有待确认" are final-release intent.

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
- Do not use final release behavior for ordinary draft update requests.

## Output Quality Bar

- The release file must be usable by downstream page-level skills without reading the draft.
- Sitemap rows must remain complete and machine-readable after applying release decisions.
- Mermaid visualization must match the final sitemap table.
- Page-level Markdown paths must remain unique.
- All product statements must read as confirmed requirements, not possibilities.
- Any non-empty user supplement from the draft must be reflected in the relevant release sections, with sitemap tables, Mermaid hierarchy, and page generation queue reconciled.

## Resources

- `references/release-quality-checklist.md`: final verification checklist for release output.

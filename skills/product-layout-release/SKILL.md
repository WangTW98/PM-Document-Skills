---
name: product-layout-release
description: "Generate one or more confirmed release layout Markdown files under product/release/layout from product/development/layout draft files after the user has resolved layout assumptions and confirmation questions. Use when an AI agent needs to release the minimum set of distinct layout-family contracts, keep filenames clearly distinguishable for downstream matching, and remove all assumptions, open questions, draft-only notes, supplement sections, and uncertain language from each released layout file."
---

# Product Layout Release

## Overview

Create the release version of the project-level layout contract or contracts from the latest layout drafts.

The output is one of these forms:

- Single layout family: `product/release/layout/product-layout-release.md`
- Multiple layout families: one file per family, for example `product/release/layout/product-layout-release-user-web.md`, `product/release/layout/product-layout-release-admin-web.md`, `product/release/layout/product-layout-release-mobile-app.md`

This skill only creates final release output. For iterative layout draft updates, use `product-layout-draft-update`.

The release layout is the confirmed layout dependency for downstream explanatory Markdown generation, including `product-page-draft`, `product-page-mock-draft`, `product-page-design-release`, and similar page-level skills. It must not contain unresolved assumptions, open questions, `LA-*` / `LQ-*` IDs, the draft `用户补充描述` section, or wording that asks the user to confirm layout details.

This skill is runner-neutral. Any AI system can use it by reading this file and the references under `references/`; platform-specific metadata belongs under `adapters/`.

## Inputs And Outputs

Required input:

- `product/release/product-sitemap-release.md`
- One or more layout draft files under `product/development/layout`, including canonical or family-specific drafts such as `product-layout-draft.md`, `product-layout-draft-user-web-v2.md`, `product-layout-draft-admin-web-v3.md`.

Default output:

- Matching release layout files under `product/release/layout`

Optional blocker output when release cannot be produced:

- `product/release/layout/product-layout-release-blockers.md` for single-family release
- `product/release/layout/product-layout-release-<layout-key>.blockers.md` for family-specific blockers when multiple families exist

## Workflow

1. Read the layout drafts.
   - Read `product/release/product-sitemap-release.md` first so the current product shape and active surfaces are clear.
   - If the user names one or more layout draft files, load those files.
   - If the user does not name draft files, load the highest available versioned draft for every active layout family under `product/development/layout`. If the product has only one family, this may be `product-layout-draft.md` or its latest versioned sibling.
   - For each draft, read section `0. 文档状态` metadata: `Layout Key`, `适用 Surface`, `适用端形态`, and `覆盖页面ID / 页面级MD文件范围`.
   - Locate the final `布局假设与待确认统一清单`.
   - Parse every layout assumption row (`LA-001`, `LA-002`, ...).
   - Parse every layout confirmation row (`LQ-001`, `LQ-002`, ...).
   - Scan the entire draft for inline `LA-*` and `LQ-*` references; every reference must be represented in the final list.
   - Locate `## 12. 用户补充描述` when present.
   - Extract the user-written natural-language supplement. Treat empty content, `无`, `none`, or placeholder-only text as no supplement.

2. Validate release readiness per layout family.
   - Every `LA-*` item must have a usable `用户确认状态` and `Release 处理方式`.
   - Every `LQ-*` item must have a usable `用户确认结果` and `Release 处理方式`.
   - Treat values like `待用户确认`, blank cells, `待确认`, `保留为风险`, or vague text without a concrete decision as unresolved.
   - If any unresolved item affects Surface, Shell, navigation, route hierarchy, page template, safe area, responsive behavior, global states, role/permission layout, or downstream skill usage, do not create that family's release file. Write the matching blocker file with the blocking rows and exact missing fields.
   - If `用户补充描述` contains ambiguous or contradictory instructions that materially affect Surface, Shell, navigation, route hierarchy, page template, safe area, responsive behavior, global states, role/permission layout, sitemap-to-layout mapping, or downstream skill usage, block release and write the blocker file with the conflicting supplement items.

3. Analyze and apply user supplement.
   - Treat non-empty `用户补充描述` content as user-confirmed release modification instructions for the layout contract.
   - Convert the supplement into concrete changes grouped by affected area: Surface/Shell definitions, global shell regions, page template library, sitemap-to-layout mapping, navigation/route rules, global state layout, role/permission layout effects, responsive behavior, downstream usage rules, and Mermaid layout map.
   - Apply supplement changes to all dependent sections, not just one row. For example, changing a root navigation model must update Surface/Shell definitions, navigation rules, sitemap-to-layout mapping, responsive behavior, and Mermaid hierarchy.
   - If a supplement instruction conflicts with an `LA-*` / `LQ-*` release handling row, prefer the more specific user supplement only when it is concrete and clearly refers to the same layout item. If the conflict changes shell, navigation, page-template, or responsive behavior and cannot be resolved confidently, block release.
   - Do not carry the `用户补充描述` section or raw notes into the release file.

4. Apply release handling decisions.
   - `确认为正式内容` / `写入正式内容`: rewrite the related layout content as confirmed layout contract and remove the ID marker.
   - `删除` / `不需要`: remove the related layout rule, shell region, template, exception, responsive rule, role rule, or note.
   - `按用户修改替换` / `改为：...`: replace the draft content with the user's confirmed wording.
   - `已否定`: remove or replace every affected mention; do not keep it as a note.
   - If multiple decisions conflict, prefer the more specific item and block release if the conflict changes navigation, shell, page-template, or responsive behavior.

5. Rewrite each file as a release layout contract.
   - Set document version to `Release`.
   - Preserve confirmed layout summary, Surface/Shell definitions, global shell regions, page template library, sitemap-to-layout mapping, navigation/route rules, global state layout, role/permission layout effects, downstream skill usage rules, and Mermaid layout map.
   - Remove the draft-only `Layout 假设 / 待确认编号规则`, `布局假设与待确认统一清单`, `用户补充描述`, and release-check wording.
   - Remove every `LA-001`, `LQ-001`, `布局假设`, `布局待确认`, `假设`, `待确认`, `待用户确认`, `置信度`, and uncertainty marker from the release file.
   - Reconcile tables after removals or replacements: Surface IDs, Shell IDs, template IDs, page IDs, parent IDs, navigation positions, and Mermaid hierarchy must remain consistent.
   - Ensure downstream rules explicitly instruct page-level skills to resolve the matched `product/release/layout/product-layout-release*.md` file by `Surface` + terminal shape + layout metadata.

6. Save and verify.
   - Ensure `product/release/layout` exists.
   - Single-family rule: write `product/release/layout/product-layout-release.md`.
   - Multi-family rule: write `product/release/layout/product-layout-release-<layout-key>.md` for each released family.
   - Run `references/product-layout-release-quality-checklist.md` manually before finishing.

## Release Rules

- The release layout document is not a changelog. It is the confirmed project-level layout contract.
- Do not include a section explaining which assumptions were removed.
- Do not include unresolved risks, questions, TODOs, or confirmation workflow sections.
- Do not include the draft `用户补充描述` section in the release file; apply it into confirmed layout content or block if ambiguous.
- Do not keep `LA-*` / `LQ-*` IDs for traceability inside the release file; traceability belongs in the draft.
- Do not invent user confirmation. If the draft does not contain a concrete release decision for a material layout item, block release.
- Do not ignore non-empty `用户补充描述`; it is part of the user's release input.
- Do not silently keep a layout rule whose release handling says to delete it.
- Do not leave sitemap rows unmapped after deleting or replacing a Surface, Shell, or page template.

## Output Quality Bar

- Each release layout file must be usable without reading its draft layout file.
- Across the full released layout set, every sitemap row must remain mapped to exactly one layout Surface, Shell, page template, navigation position, and responsive behavior.
- Every Surface must have a confirmed Shell definition.
- Navigation, route hierarchy, global state layout, safe-area/fixed-region behavior, and role/permission layout effects must read as confirmed rules.
- No `LA-*`, `LQ-*`, assumption labels, confirmation prompts, or draft-only release-handling columns may remain.
- Any non-empty user supplement from the draft must be reflected in the relevant release sections, with Surface/Shell definitions, sitemap-to-layout mapping, navigation rules, responsive behavior, and Mermaid layout map reconciled.

## Resources

- `references/product-layout-release-quality-checklist.md`: final verification checklist.

---
name: product-layout-release
description: "Generate product/release/layout/product-layout-release.md from product/development/layout/product-layout-draft.md after the user has resolved layout assumptions and confirmation questions. Use when an AI agent needs to apply LA-/LQ- Release handling decisions from the draft's final 布局假设与待确认统一清单, remove all assumptions, open questions, draft-only notes, and uncertain language, then write a confirmed project-level layout contract for downstream page, mock, and design skills."
---

# Product Layout Release

## Overview

Create the release version of the project-level layout contract from `product/development/layout/product-layout-draft.md`.

The output is:

- `product/release/layout/product-layout-release.md`

The release layout is the confirmed layout dependency for downstream explanatory Markdown generation, including `product-page-draft`, `product-page-mock-draft`, `product-page-design-release`, and similar page-level skills. It must not contain unresolved assumptions, open questions, `LA-*` / `LQ-*` IDs, or wording that asks the user to confirm layout details.

This skill is runner-neutral. Any AI system can use it by reading this file and the references under `references/`; platform-specific metadata belongs under `adapters/`.

## Inputs And Outputs

Required input:

- `product/development/layout/product-layout-draft.md`

Default output:

- `product/release/layout/product-layout-release.md`

Optional blocker output when release cannot be produced:

- `product/release/layout/product-layout-release-blockers.md`

## Workflow

1. Read the layout draft.
   - Load `product/development/layout/product-layout-draft.md`.
   - Locate the final `布局假设与待确认统一清单`.
   - Parse every layout assumption row (`LA-001`, `LA-002`, ...).
   - Parse every layout confirmation row (`LQ-001`, `LQ-002`, ...).
   - Scan the entire draft for inline `LA-*` and `LQ-*` references; every reference must be represented in the final list.

2. Validate release readiness.
   - Every `LA-*` item must have a usable `用户确认状态` and `Release 处理方式`.
   - Every `LQ-*` item must have a usable `用户确认结果` and `Release 处理方式`.
   - Treat values like `待用户确认`, blank cells, `待确认`, `保留为风险`, or vague text without a concrete decision as unresolved.
   - If any unresolved item affects Surface, Shell, navigation, route hierarchy, page template, safe area, responsive behavior, global states, role/permission layout, or downstream skill usage, do not create the release file. Write `product/release/layout/product-layout-release-blockers.md` with the blocking rows and exact missing fields.

3. Apply release handling decisions.
   - `确认为正式内容` / `写入正式内容`: rewrite the related layout content as confirmed layout contract and remove the ID marker.
   - `删除` / `不需要`: remove the related layout rule, shell region, template, exception, responsive rule, role rule, or note.
   - `按用户修改替换` / `改为：...`: replace the draft content with the user's confirmed wording.
   - `已否定`: remove or replace every affected mention; do not keep it as a note.
   - If multiple decisions conflict, prefer the more specific item and block release if the conflict changes navigation, shell, page-template, or responsive behavior.

4. Rewrite as release layout contract.
   - Set document version to `Release`.
   - Preserve confirmed layout summary, Surface/Shell definitions, global shell regions, page template library, sitemap-to-layout mapping, navigation/route rules, global state layout, role/permission layout effects, downstream skill usage rules, and Mermaid layout map.
   - Remove the draft-only `Layout 假设 / 待确认编号规则`, `布局假设与待确认统一清单`, and release-check wording.
   - Remove every `LA-001`, `LQ-001`, `布局假设`, `布局待确认`, `假设`, `待确认`, `待用户确认`, `置信度`, and uncertainty marker from the release file.
   - Reconcile tables after removals or replacements: Surface IDs, Shell IDs, template IDs, page IDs, parent IDs, navigation positions, and Mermaid hierarchy must remain consistent.
   - Ensure downstream rules explicitly instruct page-level skills to use `product/release/layout/product-layout-release.md`.

5. Save and verify.
   - Ensure `product/release/layout` exists.
   - Write `product/release/layout/product-layout-release.md`.
   - Run `references/product-layout-release-quality-checklist.md` manually before finishing.

## Release Rules

- The release layout document is not a changelog. It is the confirmed project-level layout contract.
- Do not include a section explaining which assumptions were removed.
- Do not include unresolved risks, questions, TODOs, or confirmation workflow sections.
- Do not keep `LA-*` / `LQ-*` IDs for traceability inside the release file; traceability belongs in the draft.
- Do not invent user confirmation. If the draft does not contain a concrete release decision for a material layout item, block release.
- Do not silently keep a layout rule whose release handling says to delete it.
- Do not leave sitemap rows unmapped after deleting or replacing a Surface, Shell, or page template.

## Output Quality Bar

- The release layout file must be usable without reading the draft layout file.
- Every sitemap row must remain mapped to a layout Surface, Shell, page template, navigation position, and responsive behavior.
- Every Surface must have a confirmed Shell definition.
- Navigation, route hierarchy, global state layout, safe-area/fixed-region behavior, and role/permission layout effects must read as confirmed rules.
- No `LA-*`, `LQ-*`, assumption labels, confirmation prompts, or draft-only release-handling columns may remain.

## Resources

- `references/product-layout-release-quality-checklist.md`: final verification checklist.

---
name: product-layout-release
description: "Process product/development/layout/product-layout-draft.md. By default, when the user has not explicitly asked for a final/release/正式版 layout contract, apply completed LA-/LQ- decisions and 用户补充描述 edits into a new versioned draft while preserving the layout assumption, confirmation, and empty supplement workflow for another review loop. Only when the user explicitly asks for final/release output, generate product/release/layout/product-layout-release.md by removing assumptions, open questions, draft-only notes, supplement sections, and uncertain language."
---

# Product Layout Release

## Overview

Process the project-level layout draft from `product/development/layout/product-layout-draft.md`.

This skill has two modes:

- Draft revision mode, which is the default unless the user explicitly asks for a final/release/正式版 layout contract.
- Final release mode, which keeps the existing release behavior and writes the confirmed layout dependency under `product/release/layout`.

Default draft revision output is a versioned draft beside the source draft, for example:

- `product/development/layout/product-layout-draft-v2.md`

Final release output only when explicitly requested:

- `product/release/layout/product-layout-release.md`

The release layout is the confirmed layout dependency for downstream explanatory Markdown generation, including `product-page-draft`, `product-page-mock-draft`, `product-page-design-release`, and similar page-level skills. It must not contain unresolved assumptions, open questions, `LA-*` / `LQ-*` IDs, the draft `用户补充描述` section, or wording that asks the user to confirm layout details.

This skill is runner-neutral. Any AI system can use it by reading this file and the references under `references/`; platform-specific metadata belongs under `adapters/`.

## Inputs And Outputs

Required input:

- `product/development/layout/product-layout-draft.md` or the latest versioned sibling such as `product/development/layout/product-layout-draft-v2.md`.

Default output when the user has not explicitly asked for final release:

- A versioned draft beside the source draft, using the next available suffix, for example `product/development/layout/product-layout-draft-v2.md`, `product/development/layout/product-layout-draft-v3.md`.

Final release output only when explicitly requested:

- `product/release/layout/product-layout-release.md`

Optional blocker output when release cannot be produced:

- `product/release/layout/product-layout-release-blockers.md`

## Workflow

0. Determine mode before writing any file.
   - Use final release mode only when the user explicitly asks to generate the final/release/正式版 layout contract, publish the confirmed release, write under `product/release/layout`, or remove all draft confirmation structures.
   - Use draft revision mode for requests such as "处理修改", "根据我填写的确认项更新", "继续完善", "修改 draft", or any request that does not clearly ask for final/release output.
   - If the request is ambiguous, choose draft revision mode. Do not create or overwrite `product/release/layout/product-layout-release.md` without explicit final-release intent.

1. Read the layout draft.
   - If the user names a layout draft file, load that file.
   - If the user does not name a draft file, load the highest available versioned draft matching `product/development/layout/product-layout-draft-vN.md`; if none exists, load `product/development/layout/product-layout-draft.md`.
   - Treat the loaded file as the source draft for both draft revision and final release mode.
   - Locate the final `布局假设与待确认统一清单`.
   - Parse every layout assumption row (`LA-001`, `LA-002`, ...).
   - Parse every layout confirmation row (`LQ-001`, `LQ-002`, ...).
   - Scan the entire draft for inline `LA-*` and `LQ-*` references; every reference must be represented in the final list.
   - Locate `## 12. 用户补充描述` when present.
   - Extract the user-written natural-language supplement. Treat empty content, `无`, `none`, or placeholder-only text as no supplement.

2. Validate according to the selected mode.
   - In draft revision mode, unresolved `LA-*` / `LQ-*` items are allowed and should remain in the revised draft if still material.
   - In draft revision mode, completed decisions and concrete supplement edits must be applied; unresolved or newly exposed uncertainty must be represented as layout assumptions/questions in the revised draft, not as blockers unless the layout cannot be updated coherently.
   - In draft revision mode, ambiguous or contradictory supplement items should become new `LQ-*` confirmation rows when a reasonable draft can still be produced.
   - In final release mode, use the stricter readiness rules below.
   - Every `LA-*` item must have a usable `用户确认状态` and `Release 处理方式`.
   - Every `LQ-*` item must have a usable `用户确认结果` and `Release 处理方式`.
   - Treat values like `待用户确认`, blank cells, `待确认`, `保留为风险`, or vague text without a concrete decision as unresolved.
   - If any unresolved item affects Surface, Shell, navigation, route hierarchy, page template, safe area, responsive behavior, global states, role/permission layout, or downstream skill usage, do not create the release file. Write `product/release/layout/product-layout-release-blockers.md` with the blocking rows and exact missing fields.
   - If `用户补充描述` contains ambiguous or contradictory instructions that materially affect Surface, Shell, navigation, route hierarchy, page template, safe area, responsive behavior, global states, role/permission layout, sitemap-to-layout mapping, or downstream skill usage, block release and write the blocker file with the conflicting supplement items.

3. Analyze and apply user supplement.
   - Treat non-empty `用户补充描述` content as user-confirmed modification instructions for the layout contract.
   - Convert the supplement into concrete changes grouped by affected area: Surface/Shell definitions, global shell regions, page template library, sitemap-to-layout mapping, navigation/route rules, global state layout, role/permission layout effects, responsive behavior, downstream usage rules, and Mermaid layout map.
   - Apply supplement changes to all dependent sections, not just one row. For example, changing a root navigation model must update Surface/Shell definitions, navigation rules, sitemap-to-layout mapping, responsive behavior, and Mermaid hierarchy.
   - If a supplement instruction conflicts with an `LA-*` / `LQ-*` release handling row, prefer the more specific user supplement only when it is concrete and clearly refers to the same layout item. If the conflict changes shell, navigation, page-template, or responsive behavior and cannot be resolved confidently, block release.
   - In draft revision mode, do not carry raw supplement notes forward. Apply them into the draft content, then reset `用户补充描述` to an empty editable placeholder.
   - In final release mode, do not carry the `用户补充描述` section or raw notes into the release file.

4. Apply release handling decisions.
   - `确认为正式内容` / `写入正式内容`: rewrite the related layout content as confirmed layout contract and remove the ID marker.
   - `删除` / `不需要`: remove the related layout rule, shell region, template, exception, responsive rule, role rule, or note.
   - `按用户修改替换` / `改为：...`: replace the draft content with the user's confirmed wording.
   - `已否定`: remove or replace every affected mention; do not keep it as a note.
   - If multiple decisions conflict, prefer the more specific item and block release if the conflict changes navigation, shell, page-template, or responsive behavior.

5. Rewrite as release layout contract when in final release mode.
   - Set document version to `Release`.
   - Preserve confirmed layout summary, Surface/Shell definitions, global shell regions, page template library, sitemap-to-layout mapping, navigation/route rules, global state layout, role/permission layout effects, downstream skill usage rules, and Mermaid layout map.
   - Remove the draft-only `Layout 假设 / 待确认编号规则`, `布局假设与待确认统一清单`, `用户补充描述`, and release-check wording.
   - Remove every `LA-001`, `LQ-001`, `布局假设`, `布局待确认`, `假设`, `待确认`, `待用户确认`, `置信度`, and uncertainty marker from the release file.
   - Reconcile tables after removals or replacements: Surface IDs, Shell IDs, template IDs, page IDs, parent IDs, navigation positions, and Mermaid hierarchy must remain consistent.
   - Ensure downstream rules explicitly instruct page-level skills to use `product/release/layout/product-layout-release.md`.

6. Rewrite as a new review draft when in draft revision mode.
   - Preserve the layout draft structure, including `Layout 假设 / 待确认编号规则` when present, `布局假设与待确认统一清单`, `用户补充描述`, and release-check guidance.
   - Apply all completed user decisions and supplement edits into the relevant Surface/Shell definitions, global shell regions, page template library, sitemap-to-layout mapping, navigation/route rules, global state layout, role/permission layout effects, responsive behavior, downstream usage rules, and Mermaid layout map.
   - Remove or mark as resolved the old rows that have been fully applied. Keep unresolved material rows and add new assumptions/questions caused by the latest edits.
   - Keep the `布局假设与待确认统一清单` processing columns so the user can continue confirming: layout assumption/question ID, item, affected section, impact, user confirmation/result, and Release handling fields.
   - Use stable unresolved IDs when their meaning is unchanged; add new IDs after the highest existing number. Do not reuse IDs for different meanings.
   - Keep `## 12. 用户补充描述` as the final section, but reset it to an empty placeholder for the next user edit cycle.
   - Set document version to `Draft vN` and record the source draft path.
   - Save beside the source draft using the next available version suffix. If the source is `product-layout-draft.md`, write `product-layout-draft-v2.md`; if the source is already `product-layout-draft-v2.md`, write `product-layout-draft-v3.md`.
   - Do not write `product/release/layout/product-layout-release.md` in draft revision mode.

7. Save and verify.
   - In final release mode, ensure `product/release/layout` exists.
   - In final release mode, write `product/release/layout/product-layout-release.md`.
   - In final release mode, run `references/product-layout-release-quality-checklist.md` manually before finishing.
   - In draft revision mode, verify the revised draft still has the unified layout assumption/question list and an empty final `用户补充描述` section.

## Mode Rules

- Default to draft revision mode unless final-release intent is explicit.
- Draft revision mode is a loop: apply the user's completed decisions and supplement, then produce the next reviewable draft with remaining/new layout assumptions and questions.
- Final release mode is terminal: all material layout assumptions/questions must be resolved before writing `product/release/layout/product-layout-release.md`.
- A request to "处理已修改内容", "继续修改", "生成新版 draft", or "根据补充描述更新" is not final-release intent.
- Phrases such as "生成最终版", "生成正式版", "生成 release", "输出到 product/release/layout", or "去掉所有待确认" are final-release intent.

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
- Do not use final release behavior for ordinary draft update requests.

## Output Quality Bar

- The release layout file must be usable without reading the draft layout file.
- Every sitemap row must remain mapped to a layout Surface, Shell, page template, navigation position, and responsive behavior.
- Every Surface must have a confirmed Shell definition.
- Navigation, route hierarchy, global state layout, safe-area/fixed-region behavior, and role/permission layout effects must read as confirmed rules.
- No `LA-*`, `LQ-*`, assumption labels, confirmation prompts, or draft-only release-handling columns may remain.
- Any non-empty user supplement from the draft must be reflected in the relevant release sections, with Surface/Shell definitions, sitemap-to-layout mapping, navigation rules, responsive behavior, and Mermaid layout map reconciled.

## Resources

- `references/product-layout-release-quality-checklist.md`: final verification checklist.

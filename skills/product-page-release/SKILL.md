---
name: product-page-release
description: Generate exactly one confirmed release page Markdown document per invocation from a draft page file under product/development/pages. Use when an AI agent needs to read a single page draft created by product-page-draft, apply the user's Release handling decisions from the 页面假设与待确认统一清单, analyze and apply the draft's final 用户补充描述 natural-language page edits, remove all PA-/PQ- references, assumptions, open questions, draft-only notes, supplement sections, and uncertain language, then write the confirmed page document under product/release/pages. Never process multiple page files in one execution; this prevents context overflow, hallucination, and inaccurate cross-page changes.
---

# Product Page Release

## Overview

Create the release version of exactly one page MD from `product/development/pages`. The release page is confirmed source material for downstream design, UI, API, and implementation work, so it must not contain unresolved assumptions, open questions, `PA-*` / `PQ-*` IDs, the draft `用户补充描述` section, or wording that asks the user to confirm page details.

This skill is intentionally single-page-only. If a user asks to release multiple pages or all pages, process only the first explicitly selected page, or ask the user to choose one page. Do not loop through a directory.

This skill is runner-neutral. Any AI system can use it by reading this file and the references under `references/`; platform-specific metadata belongs under `adapters/`.

## Inputs And Outputs

Required input:

- A single draft page file under `product/development/pages/*.md`.

Batch input is not allowed. If multiple draft page files are provided, stop and ask the user to select exactly one.

Default output:

- `product/release/pages/<same-relative-page-filename>.md`

Examples:

- Input: `product/development/pages/010-login.md`
- Output: `product/release/pages/010-login.md`

Optional blocker output when release cannot be produced:

- `product/release/pages/<same-relative-page-filename>.release-blockers.md`

## Workflow

1. Select one source page.
   - Use the page file explicitly named by the user.
   - If the user specifies multiple page files, do not process any file; ask the user to choose exactly one.
   - If the user asks for all pages or a directory-level release, do not iterate; explain that this skill processes one page per execution and ask for the first target page.
   - If the user does not specify a file, list candidate files under `product/development/pages` excluding `_generation-status.md` and ask the user to choose one.
   - Process exactly one page draft per invocation.

2. Read and parse the draft.
   - Load the selected page MD.
   - Verify the draft contains `## 7. 埋点事件统计设计`, `埋点事件ID`, and at least one `EVT-*` event ID.
   - If the draft lacks analytics content, do not create a release page. Write a blocker file saying the page draft must be regenerated with `product-page-draft` version `0.4.0` or later.
   - Locate the final `页面假设与待确认统一清单` section.
   - Parse every page assumption row (`PA-001`, `PA-002`, ...) and page confirmation row (`PQ-001`, `PQ-002`, ...).
   - Scan the entire draft for inline `PA-*` and `PQ-*` references; every reference must be represented in the final list.
   - Locate `## 12. 用户补充描述` when present.
   - Extract the user-written natural-language supplement. Treat empty content, `无`, `none`, or placeholder-only text as no supplement.
   - If the draft was generated before `product-page-draft` version `0.5.0` and lacks `用户补充描述`, continue only if the user did not request supplement-based changes; otherwise block and ask for the draft to be regenerated or manually updated with that section.

3. Validate release readiness.
   - Analytics content is release-critical. Missing `## 7. 埋点事件统计设计`, missing `埋点事件ID`, or missing `EVT-*` must block release.
   - Every `PA-*` item must have a usable `用户确认状态` and `Release 处理方式`.
   - Every `PQ-*` item must have a usable `用户确认结果` and `Release 处理方式`.
   - Treat values like `待用户确认`, blank cells, `待确认`, `保留为风险`, or vague text without a concrete decision as unresolved.
   - If any unresolved item affects page structure, elements, states, interactions, actions, API contracts, data structures, validation, permissions, media/resources, edge cases, or error handling, do not create the release page. Write a blocker file with the blocking rows and exact missing fields.
   - If `用户补充描述` contains ambiguous or contradictory instructions that materially affect page structure, elements, states, interactions, API contracts, data structures, validation, permissions, media/resources, edge cases, or error handling, do not guess. Write a blocker file that quotes or summarizes only the conflicting supplement items and asks for clarification.

4. Analyze and apply user supplement.
   - Treat non-empty `用户补充描述` content as user-confirmed release modification instructions for this page.
   - Convert the supplement into concrete changes grouped by affected area: page objective/context, layout section, element inventory, state matrix, action matrix, analytics events, data model, API contract, edge/error handling, and media/resources.
   - Apply supplement changes to all dependent sections, not just one paragraph. For example, adding a new button must update the element inventory, state matrix, action matrix, analytics table, API/data sections when applicable, edge cases, and Mermaid diagram.
   - If a supplement instruction conflicts with a `PA-*` / `PQ-*` release handling row, prefer the more specific user supplement only when it is concrete and clearly refers to the same page item. If the conflict changes behavior, API, permissions, or analytics and cannot be resolved confidently, block release.
   - Do not carry the `用户补充描述` section or its raw notes into the release file. Its content must be incorporated as confirmed requirements.

5. Apply release handling decisions.
   - `确认为正式内容` / `写入正式内容`: rewrite the related draft content as confirmed page content and remove the ID marker.
   - `删除` / `不需要`: remove the related element, state, action, API, data field, validation rule, media/resource, edge case, or note.
   - `按用户修改替换` / `改为：...`: replace the draft content with the user's confirmed wording.
   - `已否定`: remove or replace every affected mention; do not keep it as a note.
   - If multiple decisions conflict, prefer the more specific item and block release if the conflict changes page behavior or API contract.

6. Rewrite the page as release content.
   - Set document version to `Release`.
   - Preserve confirmed page objective, source sitemap row metadata, Mermaid diagram, layout sections, element inventory, state matrix, action matrix, analytics event statistics, data model, API contracts, edge cases, and media/resources when still relevant.
   - Remove the draft-only `页面级假设 / 待确认编号规则`, `页面假设与待确认统一清单`, `用户补充描述`, and release-check wording.
   - Remove every `PA-001`, `PQ-001`, `页面假设`, `页面待确认`, `假设`, `待确认`, `待用户确认`, `置信度`, and uncertainty marker from the release page.
   - Reconcile tables after removals or replacements: element IDs, action IDs, API IDs, data references, state references, and Mermaid nodes must still match.

7. Save and verify.
   - Ensure `product/release/pages` exists.
   - Preserve the input page filename relative to `product/development/pages`.
   - Write the release page under `product/release/pages`.
   - Run `references/page-release-quality-checklist.md` manually before finishing.

## Release Rules

- Process exactly one source page and write at most one release page or one blocker file per invocation.
- Never scan and release every file in `product/development/pages` in the same execution.
- The release page is not a changelog. It is the confirmed page specification.
- Do not include a section explaining which assumptions were removed.
- Do not include unresolved risks, questions, TODOs, or confirmation workflow sections.
- Do not include the draft `用户补充描述` section in the release page; apply it into confirmed content or block if ambiguous.
- Do not keep `PA-*` / `PQ-*` IDs for traceability inside the release file; traceability belongs in the draft page.
- Do not silently keep an element, state, action, API, or data field whose release handling says to delete it.
- Do not invent user confirmation. If the draft does not contain a concrete release decision for a material item, block release.
- Do not ignore non-empty `用户补充描述`; it is part of the user's release input.
- Do not create a release page from a stale draft that lacks analytics event statistics; block and require page draft regeneration.

## Output Quality Bar

- The release page must be usable without reading the draft page.
- The page file must contain only confirmed product/page requirements.
- Mermaid diagram must match the final element/action/state tables.
- Analytics event statistics must remain in the release page with confirmed `EVT-*` IDs, event names, counted occurrences, properties, deduplication, upload timing, attribution, and funnel relationship.
- Element IDs, Action IDs, API IDs, and referenced data objects must remain internally consistent.
- API request/response contracts must not contain unresolved alternative fields or vague placeholders.
- Any non-empty user supplement from the draft must be reflected in the relevant release sections, with all affected tables and diagrams reconciled.

## Resources

- `references/page-release-quality-checklist.md`: final verification checklist for release page output.

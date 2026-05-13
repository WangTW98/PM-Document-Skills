---
name: product-page-release
description: Generate exactly one confirmed release page bundle per invocation from a draft page bundle under product/development/pages, while re-resolving the correct release layout file for final validation. Use when an AI agent needs to read a single page draft bundle, apply the user's Release handling decisions from the 页面假设与待确认统一清单, apply the draft's 用户补充描述 edits, remove all PA-/PQ- references and uncertain language, and write the confirmed page bundle under product/release/pages.
---

# Product Page Release

## Overview

Create the release version of exactly one page bundle from `product/development/pages`.

The release bundle is confirmed source material for downstream design, UI, API, and implementation work, so it must not contain unresolved assumptions, open questions, `PA-*` / `PQ-*` IDs, the draft `用户补充描述` section, or wording that asks the user to confirm page details.

One selected sitemap row still means one page bundle:

- release main page: `product/release/pages/<page-key>/index.md`
- optional release overlay docs in the same directory

This skill only creates final release output. For iterative page draft updates, use `product-page-draft-update`.

This skill is intentionally single-page-only. If a user asks to release multiple pages or all pages, process only the first explicitly selected page, or ask the user to choose one page bundle. Do not loop through a directory.

This skill is runner-neutral. Any AI system can use it by reading this file and the references under `references/`; platform-specific metadata belongs under `adapters/`.

## Inputs And Outputs

Required input:

- a single draft page bundle under `product/development/pages/<page-key>/`

Batch input is not allowed. If multiple draft bundles are provided, stop and ask the user to select exactly one.

Default output:

- `product/release/pages/<page-key>/index.md`
- optional release overlay docs inside `product/release/pages/<page-key>/`

Examples:

- Draft canonical page key: `product/development/pages/110-order-detail.md`
- Draft bundle root: `product/development/pages/110-order-detail/`
- Release bundle root: `product/release/pages/110-order-detail/`
- Release main file: `product/release/pages/110-order-detail/index.md`

Optional blocker output when release cannot be produced:

- `product/release/pages/<page-key>/release-blockers.md`

## Workflow

1. Select one source page bundle.
   - Use the bundle explicitly named by the user.
   - If the user names the canonical bundle root or `index.md` and versioned main files exist, use the highest versioned main file unless the user explicitly says to use the named file exactly.
   - If the user specifies multiple bundles, do not process any file; ask the user to choose exactly one.
   - If the user asks for all pages or a directory-level release, do not iterate; explain that this skill processes one page bundle per execution and ask for the first target.
   - If the user does not specify a bundle, list candidate bundles under `product/development/pages` excluding status files and ask the user to choose one.
   - Process exactly one page bundle per invocation.

2. Read and parse the draft bundle.
   - Load the selected main draft file and all existing auxiliary overlay docs in the same bundle directory.
   - Read `product/release/product-sitemap-release.md` and resolve the page's sitemap row by page ID, page name, or canonical `页面级MD文件` path.
   - Resolve the correct release layout file using the same matching rules as `product-page-draft`: only consider `product-layout-release*.md`, exclude `*.blockers.md`, require Release metadata, and block on ambiguity.
   - Verify the selected main draft contains `## 7. 埋点事件统计设计`, `埋点事件ID`, and at least one `EVT-*` event ID.
   - If the draft lacks analytics content, do not create a release bundle. Write a blocker file saying the page draft must be regenerated with `product-page-draft`.
   - Locate the final `页面假设与待确认统一清单` section.
   - Parse every page assumption row (`PA-001`, `PA-002`, ...) and page confirmation row (`PQ-001`, `PQ-002`, ...).
   - Scan the entire draft bundle for inline `PA-*` and `PQ-*` references; every reference must be represented in the final list.
   - Locate `## 12. 用户补充描述` when present in the selected main draft.
   - Extract the user-written natural-language supplement. Treat empty content, `无`, `none`, or placeholder-only text as no supplement.

3. Validate release readiness.
   - Analytics content is release-critical. Missing `## 7. 埋点事件统计设计`, missing `埋点事件ID`, or missing `EVT-*` must block release.
   - Every `PA-*` item must have a usable `用户确认状态` and `Release 处理方式`.
   - Every `PQ-*` item must have a usable `用户确认结果` and `Release 处理方式`.
   - Treat values like `待用户确认`, blank cells, `待确认`, `保留为风险`, or vague text without a concrete decision as unresolved.
   - If any unresolved item affects page structure, elements, states, interactions, actions, overlay split decisions, API contracts, data structures, validation, permissions, media/resources, edge cases, or error handling, do not create the release bundle. Write a blocker file with the blocking rows and exact missing fields.
   - If `用户补充描述` contains ambiguous or contradictory instructions that materially affect page structure, overlay files, elements, states, interactions, API contracts, data structures, validation, permissions, media/resources, edge cases, or error handling, do not guess. Write a blocker file that quotes or summarizes only the conflicting supplement items and asks for clarification.

4. Analyze and apply user supplement.
   - Treat non-empty `用户补充描述` content as user-confirmed release modification instructions for this page bundle.
   - Convert the supplement into concrete changes grouped by affected area: page objective/context, layout section, element inventory, state matrix, action matrix, analytics events, data model, API contract, edge/error handling, media/resources, and overlay sub-pages.
   - Apply supplement changes to all dependent sections, not just one paragraph. For example, adding a new button or a new modal must update the element inventory, state matrix, action matrix, analytics table, API/data sections when applicable, edge cases, Mermaid diagram, and related overlay doc.
   - If a supplement instruction conflicts with a `PA-*` / `PQ-*` release handling row, prefer the more specific user supplement only when it is concrete and clearly refers to the same page item. If the conflict changes behavior, API, permissions, analytics, or overlay structure and cannot be resolved confidently, block release.
   - Do not carry the `用户补充描述` section or its raw notes into the release bundle. Its content must be incorporated as confirmed requirements.

5. Apply release handling decisions.
   - `确认为正式内容` / `写入正式内容`: rewrite the related draft content as confirmed page content and remove the ID marker.
   - `删除` / `不需要`: remove the related element, state, action, overlay doc, API, data field, validation rule, media/resource, edge case, or note.
   - `按用户修改替换` / `改为：...`: replace the draft content with the user's confirmed wording.
   - `已否定`: remove or replace every affected mention; do not keep it as a note.
   - If multiple decisions conflict, prefer the more specific item and block release if the conflict changes page behavior or API contract.

6. Rewrite the page bundle as release content.
   - Set document version to `Release`.
   - Preserve confirmed page objective, source sitemap row metadata, Mermaid diagram, layout sections, element inventory, state matrix, action matrix, analytics event statistics, data model, API contracts, edge cases, and media/resources when still relevant.
   - Remove the draft-only `页面级假设 / 待确认编号规则`, `页面假设与待确认统一清单`, `用户补充描述`, and release-check wording from the main file.
   - Remove every `PA-001`, `PQ-001`, `页面假设`, `页面待确认`, `假设`, `待确认`, `待用户确认`, `置信度`, and uncertainty marker from the entire release bundle.
   - Reconcile tables after removals or replacements: element IDs, action IDs, API IDs, data references, state references, Mermaid nodes, and overlay doc names must still match.
   - Validate that the final shell, navigation position, responsive rules, and overlay model remain consistent with the matched release layout file.
   - Generate release overlay docs when the confirmed page bundle still requires them.
   - Do not create a duplicate overlay doc in the release bundle if the same flow is already covered by a separate sitemap row.

7. Save and verify.
   - Ensure the release bundle root exists.
   - Write the release bundle under `product/release/pages/<page-key>/`.
   - Write either one release bundle or one blocker file for the selected page.
   - Run `references/page-release-quality-checklist.md` manually before finishing.

## Release Rules

- Process exactly one source page bundle per invocation.
- Never scan and release every file in `product/development/pages` in the same execution.
- The release bundle is not a changelog. It is the confirmed page specification bundle.
- Do not include a section explaining which assumptions were removed.
- Do not include unresolved risks, questions, TODOs, or confirmation workflow sections.
- Do not include the draft `用户补充描述` section in the release bundle; apply it into confirmed content or block if ambiguous.
- Do not keep `PA-*` / `PQ-*` IDs for traceability inside the release bundle; traceability belongs in the draft page bundle.
- Do not silently keep an element, state, action, overlay doc, API, or data field whose release handling says to delete it.
- Do not invent user confirmation. If the draft does not contain a concrete release decision for a material item, block release.
- Do not ignore non-empty `用户补充描述`; it is part of the user's release input.
- Do not create a release page from a stale draft that lacks analytics event statistics; block and require page draft regeneration.
- Do not release a page against the wrong layout family. If the layout match is ambiguous, block and ask the user to fix the layout file metadata or naming.

## Output Quality Bar

- The release bundle must be usable without reading the draft bundle.
- The bundle must contain only confirmed product/page requirements.
- Mermaid diagrams must match the final element/action/state tables.
- Analytics event statistics must remain in the release main file with confirmed `EVT-*` IDs, event names, counted occurrences, properties, deduplication, upload timing, attribution, and funnel relationship.
- Element IDs, Action IDs, API IDs, overlay references, and referenced data objects must remain internally consistent.
- API request/response contracts must not contain unresolved alternative fields or vague placeholders.
- Any non-empty user supplement from the draft must be reflected in the relevant release sections, with all affected tables and overlay docs reconciled.

## Resources

- `references/page-release-quality-checklist.md`: final verification checklist for release page output.

---
name: product-page-mock-release
description: Generate exactly one confirmed release mock Markdown document per invocation from a mock draft page file under product/development/mock. Use when an AI agent needs to read a single content-only mock draft created by product-page-mock-draft or product-page-mock-brief, apply the user's Release handling decisions from the final Mock 假设与待确认统一清单, remove all MA-/MQ- references, assumptions, open questions, draft-only notes, and uncertain language, then write the confirmed content-only mock document under product/release/mock. Never process multiple mock files in one execution; this prevents context overflow, hallucination, and inaccurate cross-page changes.
---

# Product Page Mock Release

## Overview

Create the release version of exactly one mock page MD from `product/development/mock`. The release mock page is confirmed display-content material for downstream visual design, UI copy, sample data, prototype, and implementation work, so it must not contain unresolved assumptions, open questions, `MA-*` / `MQ-*` IDs, or wording that asks the user to confirm mock content.

This skill is intentionally single-page-only. If a user asks to release multiple mock pages or all mock pages, process only the first explicitly selected page, or ask the user to choose one page. Do not loop through a directory.

This skill is content-only. It confirms display text, labels, options, media descriptions, sample records, state copy, and static/dynamic source labels. It must not introduce interaction execution, analytics, tracking, API contracts, backend behavior, or implementation logic.

This skill is runner-neutral. Any AI system can use it by reading this file and the references under `references/`; platform-specific metadata belongs under `adapters/`.

## Inputs And Outputs

Required input:

- A single mock draft page file under `product/development/mock/*.md`.

Batch input is not allowed. If multiple mock draft page files are provided, stop and ask the user to select exactly one.

Default output:

- `product/release/mock/<same-relative-mock-filename>.md`

Examples:

- Input: `product/development/mock/010-login.md`
- Output: `product/release/mock/010-login.md`

Optional blocker output when release cannot be produced:

- `product/release/mock/<same-relative-mock-filename>.release-blockers.md`

## Workflow

1. Select one source mock draft.
   - Use the mock draft file explicitly named by the user.
   - If the user specifies multiple mock files, do not process any file; ask the user to choose exactly one.
   - If the user asks for all mock pages or a directory-level release, do not iterate; explain that this skill processes one mock page per execution and ask for the first target page.
   - If the user does not specify a file, list candidate files under `product/development/mock` excluding `_generation-status.md` and ask the user to choose one.
   - Process exactly one mock draft per invocation.

2. Read and parse the mock draft.
   - Load the selected mock draft MD.
   - Verify it is a content-only mock draft and contains `Mock 假设与待确认统一清单`.
   - Parse every mock assumption row (`MA-001`, `MA-002`, ...) and every mock confirmation row (`MQ-001`, `MQ-002`, ...).
   - Scan the entire draft for inline `MA-*` and `MQ-*` references; every reference must be represented in the final list.
   - Verify the draft contains the expected mock content sections, especially source mapping, element content, state copy, action visible-content mapping, and static/dynamic source labels.

3. Validate release readiness.
   - Every `MA-*` item must have a usable `用户确认状态` and `Release 处理方式`.
   - Every `MQ-*` item must have a usable `用户确认结果` and `Release 处理方式`.
   - Treat values like `待用户确认`, blank cells, `待确认`, `保留为风险`, or vague text without a concrete decision as unresolved.
   - If any unresolved item affects display copy, image/audio/video content, form labels/options, placeholders, state copy, empty/error/loading copy, sample records, static/dynamic labels, or accessibility text, do not create the release mock page. Write a blocker file with the blocking rows and exact missing fields.

4. Apply release handling decisions.
   - `确认为正式内容` / `写入正式内容`: rewrite the related mock content as confirmed display content and remove the ID marker.
   - `删除` / `不需要`: remove the related copy, media description, form option, state content, mock record, or note.
   - `按用户修改替换` / `改为：...`: replace the draft content with the user's confirmed wording.
   - `已否定`: remove or replace every affected mention; do not keep it as a note.
   - If multiple decisions conflict, prefer the more specific item and block release if the conflict changes visible content meaning.

5. Rewrite the mock as release content.
   - Set document version to `Release`.
   - Preserve confirmed page content overview, source mapping matrix, Mermaid content structure, section-level content, element content inventory, form and option content, list/card/table mock data, media content, state-specific display copy, action visible-content mapping, and content source summary when still relevant.
   - Preserve `静态` / `动态` and `动态来源说明` labels for content rows.
   - Remove the draft-only `Mock 假设 / 待确认编号规则`, `Mock 假设与待确认统一清单`, and release-check wording.
   - Remove every `MA-001`, `MQ-001`, `Mock 假设`, `Mock 待确认`, `假设`, `待确认`, `待用户确认`, `置信度`, and uncertainty marker from the release mock page.
   - Reconcile tables after removals or replacements: section IDs, element IDs, state IDs, data IDs, media IDs, source references, and Mermaid nodes must still match.

6. Save and verify.
   - Ensure `product/release/mock` exists.
   - Preserve the input mock filename relative to `product/development/mock`.
   - Write the release mock page under `product/release/mock`.
   - Run `references/page-mock-release-quality-checklist.md` manually before finishing.

## Release Rules

- Process exactly one source mock page and write at most one release mock page or one blocker file per invocation.
- Never scan and release every file in `product/development/mock` in the same execution.
- The release mock page is not a changelog. It is the confirmed page display-content and mock-data specification.
- Do not include a section explaining which assumptions were removed.
- Do not include unresolved risks, questions, TODOs, or confirmation workflow sections.
- Do not keep `MA-*` / `MQ-*` IDs for traceability inside the release mock file; traceability belongs in the draft mock page.
- Do not silently keep content whose release handling says to delete it.
- Do not invent user confirmation. If the mock draft does not contain a concrete release decision for a material item, block release.
- Do not introduce API endpoint definitions, request/response schemas, interaction execution steps, analytics events, backend behavior, or implementation code.

## Output Quality Bar

- The release mock page must be usable without reading the draft mock page.
- The page file must contain only confirmed display content and mock data.
- The release mock page must remain aligned with the original release page structure through source references when those references exist.
- Every content row must retain a clear `静态` or `动态` source label.
- Dynamic content must retain a concrete source category such as `接口返回`, `用户资料`, `本地缓存`, `系统状态`, `权限状态`, `会员状态`, `上传文件`, or `生成结果`.
- Mermaid content structure, section content, element content, state content, media content, and mock data tables must remain internally consistent.
- No `MA-*` / `MQ-*` IDs, assumption labels, confirmation prompts, or draft-only release-handling columns may remain.

## Resources

- `references/page-mock-release-quality-checklist.md`: final verification checklist for release mock output.

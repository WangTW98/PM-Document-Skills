---
name: product-pages-design2figma
description: "Create exactly one Figma page design from one product/release/design/... page Markdown file using Figma Remote MCP. Use when the user provides one page design MD, a Figma link, and a target Figma page. The skill must parse the Figma link, inspect the target Figma file/page, verify the intended destination before writing, then create the page design into the correct Figma page. Never process multiple page MD files in one execution; this avoids context overflow, hallucination, and inaccurate Figma output."
---

# Product Pages Design2Figma

## Overview

Create a Figma design from exactly one page-level design release MD under `product/release/design`.

This skill is for the final handoff step from Markdown design specification to Figma. It consumes one page MD produced by `product-page-design-release` and writes the corresponding visual design into the user-provided Figma file and target Figma page through Figma Remote MCP. Because this is the step where layout rules become actual nodes, it must preserve and verify the MD's layout integrity requirements: clear hierarchy, stable spacing, explicit sizing, responsive behavior, overflow handling, and no unresolved stacking, compression, clipping, unintended overlap, hidden-control, or layer-order issues.

This skill must never guess the destination. It must analyze both:

- The user-provided Figma link.
- The user-specified Figma page or target node inside the Figma file.

The purpose is to create the design in the correct Figma file, correct Figma page, and correct insertion location.

## Inputs

Required inputs:

- Exactly one page design MD under `product/release/design/*.md`.
- One Figma link provided by the user.
- One target Figma page, provided as a page name, page node, selected node, or an explicit target node in the Figma link.

Optional inputs:

- Insert mode: create new top-level frame, append next to an existing frame, replace a marked frame, or create inside a section.
- Frame size preference: mobile, tablet, desktop, responsive set, or dimensions from the page design MD.
- User-requested application form, such as mobile app, PC web, responsive web, admin console, mini-program, tablet app, kiosk, or another surface.
- Whether to reuse existing Figma styles/components when available.

If multiple page MD files, a directory, or "all pages" are provided, stop and ask the user to choose exactly one `product/release/design/...` file. Do not batch pages in this skill.

## Single-Page Execution Limit

This limit is mandatory and exists to avoid context overflow, hallucination, and inaccurate Figma output:

- Read exactly one `product/release/design/...` file per invocation.
- Create exactly one page design in Figma per invocation.
- Do not summarize, merge, compare, or partially process multiple page design MD files.
- Do not keep looping after one page is created.
- If the user wants all pages created in Figma, use or create an orchestration skill that calls this single-page skill once per page and records status between pages.

## Figma Link And Target Resolution

Before writing anything, resolve and verify the destination.

1. Parse the Figma link.
   - Extract `fileKey` from supported URLs such as `figma.com/design/<fileKey>/...`.
   - If the URL contains `/branch/<branchKey>/`, use the branch key according to the active Figma MCP convention.
   - Extract `node-id` when present and convert URL format like `1-2` to Figma node format `1:2`.
   - Preserve the original URL in notes for traceability.

2. Inspect the target file and page.
   - Use Figma Remote MCP metadata or Plugin API inspection to list pages and identify the linked node.
   - If the user specified a page name, match it against actual Figma page names.
   - If the Figma link points to a page node, use that page as the target page.
   - If the Figma link points to a frame, section, or other node, determine its containing Figma page and use that as the target page, unless the user explicitly requested a different page.
   - If the destination page is ambiguous, stop and ask the user to specify the target Figma page. Do not write to a default page.

3. Confirm insertion location.
   - If the user requested a specific frame/section/node, insert relative to that node according to the requested insert mode.
   - If the target page is clear but no target node is specified, create a new top-level frame on that Figma page using the required MD-derived layer name.
   - If replacing an existing frame, only replace when the user explicitly requested replacement or the target node is clearly marked for this page.

## Application Form Compatibility Gate

Before writing anything to Figma, determine whether the requested page should be created as a mobile app, PC web page, responsive web page, admin console, mini-program, tablet app, or another application form, then verify that the selected design system supports that form.

Run this gate after reading the page MD and design system, but before Figma write operations.

1. Determine the requested application form.
   - First, parse the user's current instruction for explicit words such as `APP`, `移动端`, `手机端`, `iOS`, `Android`, `小程序`, `PC`, `网页`, `Web`, `响应式`, `管理后台`, `运营后台`, `控制台`, `桌面端`, `平板`, or `大屏`.
   - If the user explicitly requests a form, treat that as the target form for this invocation.
   - If the user does not explicitly specify a form, read `product/release/product-sitemap-release.md` when it exists and infer the relevant form from the product introduction, surface descriptions, sitemap rows, page ID, page name, parent page, and source mock/design MD metadata.
   - Product overview may include multiple forms, such as APP + PC admin, APP + responsive web, or mini-program + operations console. In that case, infer the form for the selected page, not for the whole product globally.
   - If multiple forms remain plausible for the selected page after reading the page MD and product overview, stop and ask the user to choose one before Figma creation.

2. Determine the selected design system's supported application forms.
   - Read the design system referenced by the page MD or selected by the user, especially `design/<design-system>/DESIGN.md`, `tokens.json`, `visual-spec.md`, `usage.md`, and `handoff/figma-remote-mcp-guide.md` when present.
   - Extract supported forms from explicit sections such as platform, surface, breakpoint, responsive behavior, frame sizes, target devices, component rules, navigation patterns, and Figma handoff notes.
   - Treat explicit exclusion as authoritative. For example, if the design system only defines PC web/admin layouts and has no mobile app rules, it does not support mobile APP creation.
   - Responsive web support is not automatically the same as native mobile app support. It can satisfy a mobile browser/responsive web request, but not an APP request unless the design system explicitly supports app/mobile-native patterns.

3. Compare requested form and supported forms.
   - If the requested/inferred form is supported, record the form in the creation plan and use the matching frame size, navigation pattern, responsive variant, and component behavior.
   - If unsupported, stop immediately before Figma write operations. Explain the mismatch clearly: requested form, selected design system path, supported forms found, missing requirements, and the required next step such as selecting another design system or generating/updating a compatible design system.
   - Do not silently adapt a PC-only design system into an APP design, or an APP-only design system into a PC/admin/web design.
   - Do not create a Figma page using a generic frame size when form compatibility is unresolved.

## App Shell And Navigation Gate

Before any Figma write operation, derive and validate the page's required shell regions. This gate is mandatory because navigation and shell omissions create incomplete or inconsistent Figma outputs.

1. Derive the product-level shell.
   - Read `product/release/product-sitemap-release.md` when it exists.
   - Extract `产品类型`, Surface, `Layout 类型`, `全局导航`, `全局操作区`, sitemap row `Layout 区域`, page level, parent page, and sibling tab-root pages.
   - For mobile App products that use `底部 Tab 导航 + 层级推入页面`, classify the selected page as login/independent, tab-root, or pushed child page.

2. Validate the selected page MD.
   - The page MD must contain an App Shell / Navigation Contract section or equivalent structured `app_shell` data in `AI 可读样式结构`.
   - The contract must explicitly state Top Navigation Bar, Main Scroll Container, Bottom Tab Bar, Fixed Footer / Bottom Action, Safe Area, and any exception reasons.
   - Tab-root pages must include a consistent Bottom Tab Bar. Pushed child pages must include a consistent Top Navigation Bar with back affordance. Login/onboarding pages may omit shell navigation only with an explicit exception reason.
   - If the MD lacks these requirements, stop before writing and require regenerating/fixing the design release MD. Do not infer or patch missing navigation during Figma creation.

3. Convert the contract into a build plan.
   - The default mobile App frame hierarchy must be `page-root` -> `safe-area-frame` -> `top-nav` when required, `main-scroll`, `bottom-tab` or `fixed-footer` when required, then named overlays.
   - Top Nav, Main Scroll, Bottom Tab, Fixed Footer, lists, forms, cards, and sections must be Auto Layout frames with explicit direction, padding, gap, alignment, resizing, and overflow.
   - Absolute positioning is allowed only for named decorative background effects and intentional overlays documented by the MD, such as FABs, badges, modals, or tooltips.

## Codex Figma MCP Notes

When using Codex with the Figma MCP tools:

- Load the `figma:figma-use` skill before every `use_figma` call.
- Prefer `get_metadata` or `get_design_context` to inspect the target node/page before writing.
- Use `use_figma` for write operations: create pages, frames, auto-layout groups, text nodes, rectangles, images/placeholders, styles, and variables.
- Do not call `generate_figma_design`; this skill creates Figma nodes from a design MD, not by capturing a web page.

## Workflow

1. Select and read one page MD.
   - Load exactly one file under `product/release/design/*.md`.
   - Verify the MD contains:
     - `文档版本 | Release`
     - Natural language style description.
     - `AI 可读样式结构`.
     - `Figma Remote MCP 生成提示`.
     - `布局完整性审核`.
     - `App Shell / 导航合同` or equivalent structured `app_shell`.
   - If the MD contains unresolved `MA-*`, `MQ-*`, `假设`, or `待确认`, stop and require a fixed release design MD.
   - If the MD lacks layout integrity audit, or any audit item is not `通过` / `已解决`, stop and require the page design release to be regenerated or fixed before Figma creation.
   - If the MD lacks `Figma Remote MCP 生成提示`, stop and require a fixed release design MD. Do not infer Figma construction guidance from prose alone.
   - If the MD lacks an App Shell / Navigation Contract or has required shell regions missing without explicit exception reasons, stop and require a fixed release design MD.

2. Extract creation plan from the MD.
   - Parse page name, output source metadata, design system path, frame hierarchy, component list, token references, content-to-style bindings, responsive rules, state display styles, and `Figma Remote MCP 生成提示`.
   - Derive the required top-level Figma layer/frame name as `<md-file-name> / <page-name-from-md>`.
   - `md-file-name` is the selected page MD basename including `.md`, for example `010-login.md`.
   - `page-name-from-md` is the page name declared inside the MD, preferring the `页面名称` metadata field, then the top-level Design Release heading if metadata is unavailable.
   - Treat the `AI 可读样式结构` as the primary machine-readable build plan.
   - Treat the `Figma Remote MCP 生成提示` as mandatory creation guidance, not advisory prose.
   - Extract every actionable instruction from `Figma Remote MCP 生成提示`, especially Frame creation order, Auto Layout settings, size constraints, overflow/clipping settings, layer order, token application, component grouping, text node naming, media placeholders, responsive variants, layout QA requirements, and prohibited generation actions.
   - Reconcile `Figma Remote MCP 生成提示` with the `AI 可读样式结构`; when they conflict, prefer the more specific instruction and stop for clarification if the conflict affects hierarchy, sizing, layer order, responsive behavior, or visible content.
   - Treat the `布局完整性审核` and Figma Remote MCP layout notes as mandatory constraints, not advisory notes.
   - Extract parent-child hierarchy, layout mode, sizing constraints, min/max dimensions, padding, gap, alignment, wrapping/truncation, overflow policy, clip-content behavior, scroll containers, layer order, and intentional overlay rules.
   - Extract app shell regions, safe-area behavior, top navigation, bottom tab navigation, fixed footer/bottom action, main scroll bottom inset, and cross-page navigation consistency rules.
   - Use natural language style sections to resolve visual nuance and hierarchy.
   - Exclude interaction execution, analytics, API contracts, backend behavior, business process logic, and implementation code.

3. Verify application form compatibility.
   - Run the Application Form Compatibility Gate.
   - Record the target form, inference source, selected design system path, supported forms, and compatibility result.
   - Stop before Figma destination resolution if the selected design system does not support the target form.

4. Verify App Shell and navigation.
   - Run the App Shell And Navigation Gate.
   - Record the selected page classification, required shell regions, explicit exceptions, and shell build plan.
   - Stop before Figma destination resolution if the selected page MD is missing required shell/navigation data.

5. Resolve Figma destination.
   - Parse the provided Figma URL.
   - Inspect the Figma file, actual pages, and target node when available.
   - Decide the exact target page and insertion node.
   - If there is any doubt about the target page, stop before writing.

6. Create Figma structure.
   - Create a top-level frame named exactly `<md-file-name> / <page-name-from-md>`, such as `010-login.md / 登录页`.
   - Use the same naming rule for a target insertion frame when the user asks to create inside a section or append near a target node.
   - Use the frame size, surface pattern, navigation pattern, and responsive variant that match the verified target application form.
   - Create the App Shell first: root device frame, safe-area frame, required top navigation, main scroll container, required bottom tab or fixed footer, then content sections and named overlays.
   - Follow the `Figma Remote MCP 生成提示` while creating nodes. Use its frame creation order, Auto Layout settings, token application method, component grouping, text naming, media placeholders, responsive variants, and prohibited actions as the operational checklist for the Figma write.
   - Apply frame size and responsive variants from the MD.
   - Build the hierarchy from top to bottom: page frame, layout regions, sections, groups, cards, text, media placeholders, form controls, tables/lists, and state variants when relevant.
   - Use auto layout directions, padding, gaps, alignment, constraints, and resizing rules from the MD.
   - Apply explicit min/max size, fill/hug/fixed sizing, wrapping/truncation, overflow, clip-content, scroll-axis, and layer-order rules from the MD.
   - Use absolute positioning only for explicitly intentional overlays such as modal scrims, badges, tooltips, floating actions, or specified media treatments. Preserve their documented layer order and collision/viewport rules.
   - Use token values from the MD/design system for fill, text, stroke, radius, elevation, typography, and spacing.
   - Preserve text content exactly from the release design MD unless Figma line wrapping requires visual layout adjustment.
   - When text or content would visibly collide or overflow at the target frame size, adjust the Figma layout according to the MD's responsive/overflow rules rather than shrinking text arbitrarily or allowing overlap.
   - Do not create small fixed wrapper frames around wider children. If a wrapper contains a button, card, input, or list wider than itself, resize the wrapper or make it Auto Layout hug/fill according to the MD before continuing.

7. Verify created design.
   - Re-inspect or screenshot the created target node when tooling allows.
   - Confirm the design was created in the intended file and page.
   - Confirm the created Figma frame matches the verified application form and was not generated with an unsupported design-system variant.
   - Confirm the top-level Figma layer/frame name exactly equals `<md-file-name> / <page-name-from-md>`.
   - Confirm required App Shell regions exist with expected names and roles: root device frame, safe-area frame, top navigation when required, main scroll container, bottom tab or fixed footer when required.
   - Confirm tab-root pages include the consistent Bottom Tab Bar and selected tab. Confirm pushed child pages include a consistent Top Navigation Bar with back affordance.
   - Confirm every actionable item in `Figma Remote MCP 生成提示` was followed or explicitly marked not applicable with a reason.
   - Confirm the Figma node names follow the MD hierarchy.
   - Confirm visible content, layout hierarchy, token usage, and responsive variants match the MD.
   - Confirm layout integrity in the created Figma nodes: no unintended overlap, clipped text/media, compressed unreadable controls, hidden key elements, ambiguous parent-child hierarchy, incorrect layer order, or responsive variant collisions.
   - Check that Auto Layout, constraints, resizing behavior, overflow/clip settings, and wrapping/truncation match the MD.
   - Check metadata for abnormal wrappers: no parent frame may be smaller than a non-overlay child in a way that clips or visually detaches the child; button/input/card wrappers must not have placeholder sizes such as `100x100` unless the child fits.
   - Check that normal layout regions are not implemented as absolute-positioned loose nodes. Only documented background effects and intentional overlays may be absolute-positioned.
   - If screenshots or metadata reveal layout problems, fix the Figma nodes before finalizing. Do not report success with unresolved visual layout issues.
   - Confirm no interaction execution, analytics, API, backend, or business logic nodes were created.

## Output In Figma

The Figma output should contain:

- One named top-level page frame or target insertion frame.
- The top-level Figma layer/frame must be named exactly `<md-file-name> / <page-name-from-md>`, for example `010-login.md / 登录页`.
- Frame size, navigation pattern, and responsive variant must match the verified application form: mobile app, PC web, responsive web, admin console, mini-program, tablet app, or other supported form.
- App Shell regions matching the page MD: root device frame, safe-area frame, top navigation, main scroll, bottom tab or fixed footer as applicable.
- Frame creation order, Auto Layout, token application, grouping, text naming, media placeholders, responsive variants, and prohibited actions must follow the MD's `Figma Remote MCP 生成提示`.
- Section frames matching the page MD structure.
- Element nodes matching the page MD content and style definitions.
- Text nodes using confirmed display copy.
- Media placeholders or imported assets when specified.
- State display variants when the page MD includes visual states.
- Responsive variants when requested or specified in the MD.
- Clear layer names based on page, section, and element IDs.
- Layout-safe frame structure with clear parent-child hierarchy, Auto Layout, constraints, overflow handling, wrapping/truncation, and layer order matching the MD.
- Metadata-safe frame structure with no abnormal wrapper dimensions, missing navigation, hidden key controls, or loose absolute-positioned normal content.

## Hard Rules

- Process exactly one `product/release/design/...` page MD per invocation.
- Create exactly one page design in Figma per invocation.
- Do not process directories or all pages.
- Do not write to Figma before parsing and verifying the Figma URL.
- Do not write to Figma before identifying the correct target page.
- Do not write to Figma before verifying application form compatibility between the user's request / inferred page surface and the selected design system.
- Do not write to Figma before verifying App Shell and navigation requirements from the product overview and selected page MD.
- Do not write to Figma from a page design MD that lacks `Figma Remote MCP 生成提示`.
- Do not write to Figma from a page design MD that lacks an App Shell / Navigation Contract or equivalent structured `app_shell` data.
- Do not ignore, skip, or loosely paraphrase actionable `Figma Remote MCP 生成提示`; apply them or stop when they conflict with other required constraints.
- Do not use a default page when the destination is ambiguous.
- Do not use a PC-only design system to create mobile APP designs, or an APP-only design system to create PC/web/admin designs.
- Do not treat responsive web support as APP support unless the design system explicitly supports APP/mobile-native patterns.
- Do not proceed when the product overview and selected page imply multiple possible forms and the user has not clarified which one to create.
- Do not create or alter unrelated Figma pages, frames, or components.
- Do not create interaction prototypes, analytics layers, API annotations, backend diagrams, or business workflow nodes.
- Do not invent content missing from the release design MD unless it is necessary as a visual placeholder; if so, label it as a visual placeholder node.
- Do not overwrite existing Figma content unless the user explicitly requested replacement.
- Do not use an alternative top-level layer name such as only the page name, only the file name, or `<Page Name> - Design Release`. The required name is `<md-file-name> / <page-name-from-md>`.
- Do not create from a page design MD that lacks layout integrity audit or has unresolved audit items.
- Do not create from a page design MD that omits required top navigation, bottom tab navigation, fixed footer, safe-area, main scroll, or explicit exception reasons.
- Do not leave Figma output with unintended overlap, stacking, clipping, squeezed unreadable text, hidden controls, ambiguous hierarchy, or incorrect layer order.
- Do not mark a Figma output complete if metadata or screenshots reveal missing navigation, abnormal wrapper sizes, child nodes wider/taller than non-overlay parents, or normal content implemented as loose absolute-positioned nodes.
- Do not use absolute positioning as a shortcut for normal layout. Use Auto Layout and constraints for normal structure; reserve absolute positioning for documented overlays only.

## Resources

- `references/figma-target-resolution-checklist.md`: required destination verification checklist.
- `references/design2figma-quality-checklist.md`: final Figma creation quality checklist.

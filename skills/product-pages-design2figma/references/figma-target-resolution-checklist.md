# Figma Target Resolution Checklist

Use this checklist before writing anything to Figma.

## Required Checks

- Exactly one `product/release/design/...` page MD was selected.
- The selected page MD contains a completed layout integrity audit and is eligible for Figma creation.
- The selected page MD contains `Figma Remote MCP 生成提示` and can provide concrete Figma creation guidance.
- The selected page MD contains an App Shell / Navigation Contract or equivalent structured `app_shell` data.
- The target application form was determined from the user's instruction, or inferred from `product/release/product-overview-release.md` plus the selected page MD.
- If the product overview includes multiple forms, the selected page's specific form was identified rather than applying a global product-level form.
- The selected design system's supported application forms were read from `DESIGN.md`, `tokens.json`, `visual-spec.md`, `usage.md`, or `handoff/figma-remote-mcp-guide.md` when present.
- The selected design system supports the target application form.
- If form compatibility was ambiguous or unsupported, no Figma write operation was performed.
- Product-level shell/navigation rules were derived from `product/release/product-overview-release.md` when present.
- The selected page's shell category was resolved: login/independent, tab-root, pushed child, or another explicit category.
- Required shell regions were resolved before writing: root device frame, safe-area frame, top navigation, main scroll, bottom tab, fixed footer/bottom action, and named overlays.
- If shell/navigation requirements were missing, ambiguous, or unsupported by the page MD, no Figma write operation was performed.
- Exactly one Figma link was provided.
- `fileKey` was extracted from the Figma link.
- If present, `node-id` was extracted and converted from URL form (`1-2`) to Figma node form (`1:2`).
- The Figma file was inspected before writing.
- The actual target Figma page was identified.
- If the link points to a frame/section/node, its containing page was identified.
- If the user supplied a page name, it was matched against actual Figma page names.
- If there are duplicate page names or ambiguous targets, writing stopped and the user was asked to clarify.
- Insert mode was resolved: new top-level frame, append near target, replace target, or create inside target.
- No Figma write operation was performed before destination verification.

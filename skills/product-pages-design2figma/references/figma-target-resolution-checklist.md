# Figma Target Resolution Checklist

Use this checklist before writing anything to Figma.

## Required Checks

- Exactly one `product/release/design/...` page MD was selected.
- The selected page MD contains a completed layout integrity audit and is eligible for Figma creation.
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

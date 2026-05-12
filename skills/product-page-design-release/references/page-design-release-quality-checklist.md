# Page Design Release Quality Checklist

Use this checklist before writing the final response.

## Required Checks

- Exactly one `product/release/mock/...` page was used as the content source.
- Exactly one `design/<design-system>/` directory was used as the design constraint source.
- `DESIGN.md` from the selected design system was read.
- `tokens.json`, `visual-spec.md`, and `handoff/figma-remote-mcp-guide.md` were used when present and relevant.
- Exactly one output file was created or updated under `product/release/design`.
- The output filename preserves the input mock filename relative to `product/release/mock`.
- The output document version is `Release`.
- The output contains both human-readable natural language style description and AI-readable structured style description.
- Every major content section from the release mock page maps to a layout/style treatment.
- Every major visible element from the release mock page maps to an element-level visual definition.
- The design uses token names and exact values from the selected design system where available.
- Layout integrity audit is present and every audit row is `通过` or `已解决`.
- Every major frame has a clear parent, layout mode, sizing rule, padding, gap, alignment, overflow policy, and responsive behavior.
- Every major visible element has a clear parent container, minimum size, text wrapping/truncation behavior, and alignment rule.
- No unresolved page design rule would cause unintended overlap, stacking, clipping, compressed unreadable text, hidden controls, or incorrect layer order.
- Any intentional overlay, floating element, modal, badge, or media overlap has an explicit layer order, viewport/collision rule, and justification.
- Responsive rules are included for mobile, tablet, desktop, and wide desktop when applicable.
- Responsive rules explain how navigation, grids, cards, tables/lists, button groups, forms, media, and long text adapt without visual collisions.
- State display styles are present for relevant loading, empty, error, disabled, success, and media failure states.
- Figma Remote MCP handoff notes are included.
- Figma Remote MCP handoff notes include Auto Layout, constraints, overflow/clipping, layer order, and post-generation layout QA requirements.
- The output contains no interaction execution, analytics, tracking, API contracts, backend behavior, implementation code, or business process logic.
- The output contains no `MA-*`, `MQ-*`, assumptions, open questions, or pending confirmation sections.

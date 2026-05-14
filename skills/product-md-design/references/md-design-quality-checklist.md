# MD Design Quality Checklist

Use this checklist before finishing.

## Required Checks

- Exactly one source Markdown file under `product/release/pages` was processed.
- The selected source is not a status file.
- `product/release/product-sitemap-release.md` was read.
- A unique matched release layout file was resolved from `product/release/layout/product-layout-release*.md`.
- If the source file is not the main page file, its owning page directory was still resolved correctly.
- If a design spec was explicitly provided, it was read and cited in both JSON files.
- If no design spec was provided, both JSON files clearly mark `generated_mode` as `layout-inferred-style`.
- Both JSON files were written under `product/design` with the same relative path and basename as the source file.
- The `.md` suffix was replaced with `-figma.json` and `-pencil.json`.
- Both JSON files keep only visual structure, style clues, visible states, and visible copy.
- Neither JSON file contains product goals, KPI text, analytics event IDs, API IDs, request/response fields, route targets, or backend workflow prose.
- The visible hierarchy in both JSON files matches the same page interpretation.
- The matched shell type, page template, and major layout regions are preserved.
- Visible empty/loading/error/success/warning states were kept only when the source page or matched layout shows them as actual UI variants.
- Every visible state named in the source page or matched layout has corresponding drawable example content in the JSON, not just a metadata label.
- Non-leaf containers define enough width/height behavior for downstream tools to preserve child-driven expansion instead of guessing collapsed wrappers.
- Cards, toolbars, sections, rows, and grouped controls define explicit `gap` / `padding` values instead of leaving spacing to downstream inference.
- When a design spec was provided, the spacing scale follows that spec before any raw inferred values are introduced.
- When no design spec was provided, the page still uses a coherent spacing scale rather than inconsistent one-off numbers.
- Any list/table header and item rows use a shared column model or an equivalent consistent row-cell structure.
- Corresponding header and body columns use matching width behavior, spacing rhythm, and alignment semantics.
- No table/list output relies on ad hoc header widths or free-positioned body text that would cause visible misalignment.
- List, table, queue, and card outputs include enough representative sample items to cover each visibly distinct item status described by the source page.
- Loading, empty, error, expired, warning, done, or similar states are backed by concrete placeholder/example nodes when the source describes them.
- Any scroll, clip, or overflow behavior needed by the visible layout is explicitly encoded instead of implied.
- Icon-bearing controls such as checkbox, radio, select, search, password toggle, close, chevron, upload, and status badges are represented as icon/media semantics rather than plain text glyphs.
- Figma-targeted icon nodes include enough family, geometry, or fallback metadata that a downstream renderer will not collapse them into generic circles or rectangles.
- No control icon that should be a chevron, magnifier, checkbox, radio, or close glyph has been reduced to an unrelated primitive placeholder.
- No extra tabs, dialogs, banners, cards, metrics, or pages were invented without source support.
- The JSON is valid and contains no Markdown fences.
- If layout matching or source interpretation was ambiguous, a blocker file was written instead of guessing.

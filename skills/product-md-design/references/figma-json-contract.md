# Figma JSON Contract

Use this contract for `*-figma.json`.

## Purpose

The file is a design-ready, Figma-oriented node tree. It is not raw Figma API output, but it must be structured so an AI agent can map it into Figma frames, auto layout, text, shapes, and variants without re-reading the source Markdown.

## Required Top-Level Shape

```json
{
  "meta": {},
  "layout_match": {},
  "style_source": {},
  "page": {},
  "state_examples": {},
  "nodes": []
}
```

## Required Fields

- `meta`
  - `source_md`
  - `output_file`
  - `page_id`
  - `page_name`
  - `generated_mode`: `"with-design-spec"` or `"layout-inferred-style"`
- `layout_match`
  - `layout_file`
  - `layout_key`
  - `surface`
  - `shell_type`
  - `page_template`
  - `responsive_rule_summary`
- `style_source`
  - `design_spec_path` or `null`
  - `token_refs`
  - `style_notes`
- `page`
  - `frame_name`
  - `frame_type`
  - `visible_state_set`
  - `copy_tone`
- `state_examples`
  - keyed by visible state name such as `default`, `loading`, `empty`, `error`
  - each entry summarizes which example rows, cards, placeholders, banners, or state blocks should be rendered for that state
- `nodes`
  - ordered visible tree under the page frame

## Layout Integrity Rules

Every non-leaf container must make sizing behavior explicit enough for a downstream Figma agent to create reliable auto-layout:

- `layout.width` and `layout.height` must be set intentionally per axis as one of:
  - `hug`
  - `fill`
  - `fixed`
  - `viewport`
- Add `layout.min_width` / `layout.min_height` when the container should not collapse below a control-safe size.
- Add `layout.max_width` / `layout.max_height` when the source page clearly caps a container.
- Add `layout.children_width` / `layout.children_height` when children should stretch, hug, or remain fixed by default.
- Add `layout.overflow` with one of:
  - `visible`
  - `clip`
  - `scroll-x`
  - `scroll-y`
  - `scroll-both`
- If a frame is intended to be expanded by children, do not omit its `hug` behavior and do not replace it with a vague free-form note.

## Spacing Rules

Every container that groups visible content must encode spacing explicitly enough for a downstream Figma agent to avoid cramped or arbitrary layouts:

- `layout.gap` is required for every auto-layout container with more than one visible child unless the intended gap is truly `0`.
- `layout.padding` is required for every card, panel, toolbar, input-like control, modal, banner, table row wrapper, and section wrapper unless the intended padding is truly `0`.
- Prefer spacing tokens or design-spec references when a design spec exists, such as `token.space.4`, `token.space.6`, `token.space.8`.
- If no design spec exists, use a consistent local spacing scale such as `local.space.4`, `local.space.8`, `local.space.12`, `local.space.16`, `local.space.24`, `local.space.32`.
- Use tighter gaps for micro-assemblies such as icon+text, label+value, and segmented chips.
- Use larger gaps for section separation, card stacking, and page-level rhythm.
- Do not mix many unrelated spacing values in the same page without a clear structural reason.

## List And Table Column Consistency Rules

Any list-like or table-like structure with visible columns must encode a shared column model so header and body rows can be created without drift:

- Add a `columns` definition on the nearest table/list container whenever the UI has a stable multi-column structure.
- Each column entry should define:
  - `id`
  - `name`
  - `width` or equivalent width behavior
  - `align`
  - optional `padding`
  - optional `gap`
- Header cells and body cells must follow the same column order and reference the same logical columns.
- If one column is `fill`, the corresponding header cell and every body cell in that column must use that same fill rule.
- If one column is fixed-width, header and body must use the same fixed-width definition unless the source explicitly shows a different structure.
- Row padding and inter-column spacing must be consistent across header and item rows unless the source explicitly introduces a different visual treatment.
- Prefer a dedicated `table`, `table-header`, `table-row`, and `table-cell` hierarchy, or an equivalent frame-row-cell hierarchy that still preserves shared column rules.
- Do not encode header widths and body widths as unrelated ad hoc values.
- Do not place header labels and row values as independent free-positioned text nodes when the intended structure is a column-aligned list or table.

## Node Shape

Each node must use this shape:

```json
{
  "id": "N-001",
  "name": "Header / Status Overview",
  "type": "frame",
  "role": "section",
  "layout": {
    "mode": "vertical",
    "gap": 24,
    "padding": [24, 24, 24, 24],
    "width": "fill",
    "height": "hug",
    "min_height": 56,
    "children_width": "stretch",
    "children_height": "hug",
    "overflow": "visible",
    "align": "stretch"
  },
  "style": {
    "fill": "token.color.surface.card",
    "stroke": "token.color.border.default",
    "radius": 16,
    "shadow": "token.shadow.card"
  },
  "content": {
    "text": null,
    "items": []
  },
  "states": [
    {
      "name": "default",
      "visible": true
    }
  ],
  "children": []
}
```

For leaf nodes that are not text-only, include a `size` object when the rendered footprint matters:

```json
{
  "size": {
    "width": 16,
    "height": 16
  }
}
```

## Allowed Node Types

- `frame`
- `text`
- `rectangle`
- `ellipse`
- `line`
- `group`
- `instance`
- `icon`
- `image`

Prefer `frame` for most containers.

## Required Semantics

- Preserve visible order top-to-bottom, left-to-right.
- Use `role` to explain intent, such as `page`, `shell-region`, `section`, `card`, `list`, `table`, `form`, `cta`, `badge`, `empty-state`, `status-banner`, `drawer`, `modal`.
- Use semantic style references when a design spec exists.
- If no design spec exists, use readable local aliases such as `local.color.surface.emphasis` and `local.space.24`.
- If a design spec exists, spacing values should usually come from its token scale before introducing raw numbers.
- Represent visible variants under `states`, not as backend flags.
- Use `content.text` only for on-screen copy.
- Use `content.items` for repeated visible slots, such as metric labels, table headers, card bullets, or placeholder rows.
- Use `content.items`, `example_items`, or equivalent structured fields for repeatable content so downstream Figma generation does not need to invent list rows, table rows, card examples, or timeline entries.
- When a page has multiple visually distinct states, include state-specific example data that makes each state directly renderable.
- For list and table pages, the `default` state should usually include more than one example item whenever the source describes multiple row-level statuses.
- For row-level states, prefer explicit per-item fields such as `status`, `variant`, `badge`, `priority`, `due_state`, `action_state`, or equivalent visible markers.
- Encode real UI icons as `type: "icon"` nodes, not as `text` nodes.
- Every icon node must include:
  - `icon.name`: semantic icon name such as `checkbox-check`, `chevron-down`, `close`, `search`, `eye-off`
  - `icon.style`: `outlined`, `filled`, `duotone`, or `brand`
  - `icon.size`: visible pixel size such as `12`, `16`, `20`, or `24`
  - `icon.fallback_shape`: shape hint such as `check`, `chevron`, `circle-check`, `x`, `qr-refresh`
- Every Figma icon node should also include, whenever possible:
  - `icon.source_family`: icon source family such as `design-spec`, `lucide`, `material-symbols`, `phosphor`, `local-asset`, or `vector-described`
  - `icon.stroke_weight`: intended visual stroke weight when the icon is outline-based
  - `icon.render_preference`: preferred downstream rendering order such as `component-instance`, `vector-path`, `svg-asset`, `draw-manually`
  - `icon.geometry_hint`: short geometric description such as `downward outlined chevron`, `magnifier with circular lens and short handle`, `unchecked rounded square with centered check when active`
- If the exact icon family is unknown, set `icon.source_family` to `vector-described` and provide a non-trivial `icon.geometry_hint` that allows faithful vector reconstruction.
- `icon.fallback_shape` is a last-resort hint, not permission to replace the icon with a generic blob.
- For compound controls such as checkbox, radio, select, password field, search box, upload area, dismissible banner, and accordion rows, include the icon node as a child of the control container instead of embedding the symbol in `content.text`.
- For column-aligned list/table UIs, place header labels and row values inside matching cell containers rather than as loose sibling text nodes.

## Icon Fallback Rules

Downstream Figma rendering must preserve icon intent instead of collapsing to placeholder shapes:

- Prefer an existing design-system icon component or local asset when the design spec defines one.
- Otherwise prefer a known icon family named in `icon.source_family`.
- Otherwise draw the icon as vector geometry using `icon.geometry_hint`, `icon.style`, `icon.size`, and `icon.stroke_weight`.
- Only use `icon.fallback_shape` to guide vector reconstruction of the intended icon silhouette.
- Do not substitute a select chevron, search icon, checkbox, radio, close icon, or status icon with a filled circle, a plain rectangle, or another unrelated primitive unless the source page explicitly shows that primitive.

## State And Example Coverage Rules

The JSON must carry enough example data to render all documented visible states without ad hoc invention:

- Add `state_examples.default` for the primary populated state.
- Add a separate entry for every other visible state explicitly described by the source page or matched layout, such as `loading`, `empty`, `error`, `disabled`, `expired`, `overdue`, `success`, or `warning`.
- If the page is list-like or table-like and the source mentions multiple row statuses, include one representative example item for each distinct row appearance.
- Do not compress visually different item states into a single generic example row.
- If a state replaces the list entirely, such as `empty` or `loading`, represent the visible placeholder structure for that state instead of only naming it.

## Prohibited Content

Do not include:

- API IDs, route targets, analytics IDs, request fields, or backend enums
- invisible business objects
- raw Markdown tables
- Mermaid source
- icons represented only by text glyphs such as `v`, `x`, `√`, `□`, `○`, `>` when the intent is graphical UI chrome
- icons represented only by generic filled circles or rectangles when the intended control requires a semantic symbol such as a chevron, magnifier, check mark, radio dot, or close glyph
- container nodes whose child-driven sizing behavior is left implicit
- container nodes whose visible spacing is left implicit or arbitrary
- state declarations that exist in metadata but have no accompanying example content for a downstream renderer to draw

## Minimal Example

```json
{
  "meta": {
    "source_md": "product/release/pages/020-user-dashboard/index.md",
    "output_file": "product/design/020-user-dashboard/index-figma.json",
    "page_id": "U-200",
    "page_name": "用户工作台",
    "generated_mode": "with-design-spec"
  },
  "layout_match": {
    "layout_file": "product/release/layout/product-layout-release-user-web.md",
    "layout_key": "user-web",
    "surface": "用户端",
    "shell_type": "top-nav",
    "page_template": "TPL-004",
    "responsive_rule_summary": "desktop-first content container with wrapped cards on narrow widths"
  },
  "style_source": {
    "design_spec_path": "design/enterprise-admin-minimal-ant-card/DESIGN.md",
    "token_refs": ["color.surface.card", "space.24", "radius.card"],
    "style_notes": ["summary metrics use stronger contrast than service shortcuts"]
  },
  "page": {
    "frame_name": "U-200 用户工作台",
    "frame_type": "desktop-page",
    "visible_state_set": ["default", "empty", "loading"],
    "copy_tone": "clear, task-oriented"
  },
  "nodes": []
}
```

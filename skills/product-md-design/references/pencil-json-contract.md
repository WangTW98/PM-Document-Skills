# Pencil JSON Contract

Use this contract for `*-pencil.json`.

## Purpose

The file is a Pencil-style node tree that maps closely to `.pen` concepts. It is intended to be easy for an AI agent to convert into Pencil document nodes or batch operations without carrying product logic noise.

## Required Top-Level Shape

```json
{
  "meta": {},
  "layout_match": {},
  "style_source": {},
  "state_examples": {},
  "document": {}
}
```

## Required `document` Shape

```json
{
  "type": "frame",
  "name": "Page Root",
  "layout": "vertical",
  "gap": 24,
  "padding": 24,
  "width": 1440,
  "height": "hug",
  "fill": "$color-background-canvas",
  "children": []
}
```

The root `document` object must represent the visible page root, not the global file tree.

## Allowed Node Types

- `frame`
- `group`
- `rectangle`
- `ellipse`
- `line`
- `polygon`
- `text`
- `icon_font`
- `ref`

Prefer `frame` for nearly all layout containers.

## Node Rules

- Use only properties compatible with the intended node type.
- Express visible layout using Pencil-style fields such as:
  - `layout`
  - `gap`
  - `padding`
  - `justifyContent`
  - `alignItems`
  - `width`
  - `height`
  - `minWidth`
  - `minHeight`
  - `fill`
  - `stroke`
  - `cornerRadius`
- For every container, make per-axis sizing explicit enough to preserve child-driven growth:
  - use `height: "hug"` and/or `width: "hug"` when parent size must be expanded by children
  - use `height: "fill_container"` or `width: "fill_container"` only when a parent truly constrains that axis
  - add `clipContent: true` only when clipping is intentional
- Do not rely on downstream tools to infer whether a wrapper should hug its contents.
- Every frame or group that visually organizes multiple children must also define intentional spacing:
  - use `gap` for inter-child rhythm
  - use `padding` for card, panel, toolbar, filter, input, row, and section insets
  - prefer design-spec spacing variables when available
  - otherwise use a small consistent local spacing scale instead of arbitrary values
- For list-like and table-like structures, keep a shared row-and-cell layout model across header and item rows:
  - corresponding header and body cells must use the same width behavior
  - corresponding header and body cells must use the same alignment rule
  - row padding and inter-cell gap must be consistent unless the source explicitly varies them
  - prefer a `frame -> row -> cell -> text/icon` hierarchy rather than free-positioned text fragments
- If a multi-column list has a visible header row, do not let header cells and item cells define unrelated widths or arbitrary horizontal offsets.
- Use `text` nodes for visible copy only.
- Use variable references such as `$color-surface-card` when a design spec exists.
- If no design spec exists, use stable local variable-style names such as `$local-color-surface-card`.
- Encode UI chrome icons as `icon_font`, `ref`, or vector-capable non-text nodes when the final rendering is supposed to be graphic.
- Do not encode checkbox marks, select arrows, radio dots, close icons, password toggles, or status symbols as ordinary `text` nodes.
- When the exact icon font or component family is unknown, keep enough semantic metadata in the node name or context to preserve the intended icon, for example `icon/select-chevron-down/outlined` instead of a generic `icon`.
- Do not replace semantic control icons with arbitrary geometric placeholders if a downstream Pencil renderer can draw or map a closer icon.

## Required Metadata

- `meta.source_md`
- `meta.output_file`
- `meta.page_id`
- `meta.page_name`
- `meta.generated_mode`
- `layout_match.layout_file`
- `layout_match.layout_key`
- `layout_match.page_template`
- `style_source.design_spec_path`
- `state_examples`
  - keyed by visible state name
  - each entry describes representative rows, cards, placeholders, or banners needed to render that state in Pencil

## Visible Variant Encoding

Visible states must be stored under `meta.visible_states` and, when needed, on individual nodes through a `variant` field.
- Use `state_examples` to describe which sample items belong to each visible state when one static document tree is not sufficient by itself.
- For list/table/card pages, include representative sample items for each materially different visible state described by the source page.

Example:

```json
{
  "type": "frame",
  "name": "Refund CTA",
  "variant": "warning",
  "layout": "horizontal",
  "children": [
    {
      "type": "text",
      "content": "申请退款",
      "fontSize": 14
    }
  ]
}
```

## Prohibited Content

Do not include:

- backend data objects
- analytics or API identifiers
- raw Mermaid content
- action execution prose
- hidden business decision notes
- icon-only UI affordances expressed as plain text placeholders
- semantic UI icons degraded to generic circles, rectangles, or other placeholder geometry that no longer matches the intended control
- containers whose hug/fill intent is ambiguous for downstream layout engines
- containers whose grouping spacing is ambiguous or omitted without reason
- table/list header rows and item rows whose column widths, gaps, or alignment semantics drift apart
- visible states listed in metadata but not backed by drawable example rows, cards, placeholders, or state blocks

## Minimal Example

```json
{
  "meta": {
    "source_md": "product/release/pages/110-order-detail/index.md",
    "output_file": "product/design/110-order-detail/index-pencil.json",
    "page_id": "U-420",
    "page_name": "订单详情",
    "generated_mode": "layout-inferred-style",
    "visible_states": ["default", "warning", "error"]
  },
  "layout_match": {
    "layout_file": "product/release/layout/product-layout-release-user-web.md",
    "layout_key": "user-web",
    "page_template": "TPL-004"
  },
  "style_source": {
    "design_spec_path": null
  },
  "document": {
    "type": "frame",
    "name": "U-420 订单详情",
    "layout": "vertical",
    "gap": 24,
    "padding": 24,
    "width": 1280,
    "height": "hug",
    "fill": "$local-color-background-canvas",
    "children": []
  }
}
```

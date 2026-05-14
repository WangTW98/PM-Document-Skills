# Content Filter Rules

Use this reference when converting a release page Markdown file into a design-only source.

## Keep

Keep content that affects what the designer should draw:

- Page title, shell type, page template, breadcrumb, tabs, local nav, section order.
- Region and container structure from Mermaid diagrams when those nodes are visible UI regions.
- Layout tables that define sections, cards, tables, forms, summaries, sidebars, drawers, modals, banners, empty states, or sticky/fixed areas.
- Element tables when they describe visible controls, visible labels, visible placeholders, visible summaries, visible media, or visible grouping.
- State tables when they describe visible variants such as empty, loading, success, error, disabled, expanded, selected, highlighted, danger, sticky, compact, open, or unread.
- Media tables when they describe icons, illustrations, avatars, thumbnails, or files with visible presentation.
- Layout-file shell rules that determine top nav, side nav, fixed footer, scroll area, modal layer, content container width, or responsive collapse rules.

## Remove

Remove content that should not create nodes:

- 页面目的, 用户目标, 业务目标, 成功标准.
- 入口与出口 tables unless they define visible breadcrumb or navigation placement already expressed elsewhere.
- Role/permission tables unless the permission failure is itself a dedicated visible screen state.
- Route params, global store names, API names, request/response fields, event names, event IDs, analytics dimensions, dedup rules, upload timing.
- Action tables that explain system execution, form submission logic, redirects, or backend side effects.
- Data structure tables unless a field is directly rendered as a visible labeled value.
- Validation internals and business rules that do not change visible layout.

## Rewrite Mixed Content

When a row mixes visual and non-visual information:

- Keep the visible component type.
- Keep the visible label or content.
- Keep style or hierarchy clues.
- Keep the visible variant trigger only when it changes appearance.
- Drop backend references, IDs, API names, and execution details.

Example:

- Source: `点击“去支付”后调用 API-002 并跳转 U-330`
- Keep: `主按钮：去支付`
- Drop: `API-002` and route target mechanics

Example:

- Source: `状态="已支付" 且 服务未启动时显示申请退款按钮`
- Keep: `状态变体：已支付未启动时显示次级按钮“申请退款”`
- Drop: deeper business explanation beyond visible condition

## Visible Error And Edge States

Keep edge/error rows only when they imply an actual screen or component variant, such as:

- 404 not found page
- empty illustration state
- inline error banner
- disabled CTA with helper copy
- timeout warning panel
- refund-processing status strip

Drop any recovery, retry, or backend handling prose that does not change the visible design.

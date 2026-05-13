# Figma Remote MCP Guide

## 必读文件

1. `design/ios-26-liquid-glass-cross-platform/DESIGN.md`
2. `design/ios-26-liquid-glass-cross-platform/tokens.json`
3. `design/ios-26-liquid-glass-cross-platform/visual-spec.md`

## Token 到 Figma 的映射

- Colors -> Figma Variables: `color/light/*`, `color/dark/*`
- Typography -> Text Styles: `type/hero`, `type/h1`, `type/body`, `type/caption`
- Space / Radius -> Variables
- Shadow / Blur -> Effect Styles

## 推荐文件结构

- `00 Foundations`
- `01 Components`
- `02 Patterns`
- `03 Templates`
- `10 iPhone`
- `11 H5 Web`
- `12 iPad Tablet`
- `13 PC Web`

## 必建组件

- Button: Primary / Secondary / Ghost / Destructive
- Input: Default / Focus / Error / Disabled
- Search Bar
- Segmented Control / Tabs / Chips
- Card / Glass Card / Metric Card
- Top Bar / Bottom Tab / Sidebar / Breadcrumb
- Table Row / List Item / Empty State / Toast / Modal / Popover

## 命名规范

- 变量: `color/light/glass/fill`
- 文字样式: `type/body/regular`
- 组件: `Button/Primary/Default`
- 端模板: `Template/Dashboard/Desktop`

## Frame 建议

- iPhone: `390x844`
- H5 Mobile: `393x852`
- iPad Portrait: `834x1194`
- iPad Landscape: `1194x834`
- Desktop: `1440x1024`

## 响应式检查

- 同一页面至少提供 4 套 frame
- Tablet 必须有 Sidebar / Split View / Popover 逻辑
- Desktop 必须有 Header / Sidebar / Breadcrumb / Hover 反馈
- H5 必须标注安全区与 Sticky CTA

## 无障碍与降级

- 玻璃文本区不够清晰时改用 `fillStrong`
- Reduce Transparency 页面需复制一套实底演示
- Reduce Motion 页面需标注“无位移动效”交互策略

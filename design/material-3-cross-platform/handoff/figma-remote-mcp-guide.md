# Figma Remote MCP Guide

## 必读文件

1. `design/material-3-cross-platform/DESIGN.md`
2. `design/material-3-cross-platform/tokens.json`
3. `design/material-3-cross-platform/visual-spec.md`

## Token 到 Figma 的映射

- 颜色变量: `color/light/*`, `color/dark/*`
- 字体样式: `type/display/*`, `type/headline/*`, `type/body/*`, `type/label/*`
- 圆角变量: `radius/*`
- 阴影样式: `shadow/level/*`
- 间距变量: `space/*`

## 推荐页面结构

- `00 Foundations`
- `01 Components`
- `02 Patterns`
- `03 Templates`
- `10 Mobile App`
- `11 H5 Web`
- `12 Tablet`
- `13 PC Web`

## 必建组件

- Button: Filled / Tonal / Outlined / Text / Elevated
- Text Field: Outlined / Filled / Error / Disabled / Focused
- Select / Dropdown / Autocomplete
- Top App Bar / Bottom Navigation / Navigation Rail / Sidebar
- Card / List Item / Data Table / Filter Chips
- Snackbar / Dialog / Bottom Sheet / Empty State

## 命名规范

- Variable: `color/light/primary/base`
- Text Style: `type/body/large`
- Effect: `shadow/level/2`
- Component: `Button/Filled/Default`
- Template: `Template/Dashboard/Desktop`

## Frame 建议

- Mobile App: `390x844`
- H5 Web: `393x852`
- Tablet Portrait: `834x1194`
- Tablet Landscape: `1194x834`
- Desktop: `1440x1024`

## 响应式检查

- 每个关键页面至少 4 套 frame
- Tablet 必须体现 rail 或 split view
- Desktop 必须体现 sidebar、table、breadcrumb 或 detail pane
- Mobile / H5 必须体现导航简化与触控优先

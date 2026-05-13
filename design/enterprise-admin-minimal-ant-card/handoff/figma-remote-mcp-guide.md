# Figma Remote MCP Guide

## 必读文件

1. `design/enterprise-admin-minimal-ant-card/DESIGN.md`
2. `design/enterprise-admin-minimal-ant-card/tokens.json`
3. `design/enterprise-admin-minimal-ant-card/visual-spec.md`

## Token 到 Figma 的映射

- Color Variables: `color/light/*`, `color/dark/*`
- Text Styles: `type/h1`, `type/body`, `type/bodySmall`, `type/caption`
- Radius Variables: `radius/*`
- Effect Styles: `shadow/card`, `shadow/float`, `shadow/drawer`
- Layout Variables: `space/*`, `component/nav/*`, `component/table/*`

## 推荐页面结构

- `00 Foundations`
- `01 Components`
- `02 Patterns`
- `03 Templates`
- `10 PC Web`
- `11 Tablet`
- `12 H5 Web`
- `13 Mobile App`

## 必建组件

- Header / Sidebar / Breadcrumb / Page Header
- Button / Input / Select / Search / Date Picker / Tag
- Filter Bar / Table / Pagination / Tabs
- Metric Card / Detail Card / Summary Card
- Drawer / Modal / Popover / Tooltip
- Alert / Message / Notification / Result / Empty State

## 命名规范

- Variable: `color/light/brand/primary`
- Text Style: `type/body/default`
- Effect: `shadow/card`
- Component: `Table/Default/WithToolbar`
- Template: `Template/Dashboard/Desktop`

## Frame 建议

- PC Web: `1440x1024`
- Wide Desktop: `1600x1024`
- Tablet: `1194x834`
- H5 Web: `393x852`
- Mobile App: `390x844`

## 响应式检查

- Desktop 模板必须体现 Header + Sidebar + Content
- Tablet 模板必须体现折叠导航与抽屉
- H5 / Mobile 必须体现列表转卡片或摘要
- 表单与表格必须有深色模式样例

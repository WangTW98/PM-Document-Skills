---
design_system:
  name: "Enterprise Admin Minimal Ant Card"
  version: "0.1.0"
  intent: "为企业级后台管理系统提供一套融合极简企业风、Ant Design 管理体验和卡片化轻量布局的跨端设计系统，适用于 Dashboard、列表、表单、审批、设置和数据管理场景。"
  tokens:
    color:
      light:
        brand:
          primary: "#1677FF"
          primary_hover: "#4096FF"
          primary_active: "#0958D9"
          primary_bg: "#E6F4FF"
          primary_border: "#91CAFF"
        neutral:
          bg_canvas: "#F5F7FA"
          bg_layout: "#F0F2F5"
          bg_container: "#FFFFFF"
          bg_elevated: "#FFFFFF"
          bg_mask: "rgba(0,0,0,0.45)"
        text:
          primary: "#1F2329"
          secondary: "#4E5969"
          tertiary: "#86909C"
          quaternary: "#C9CDD4"
          inverse: "#FFFFFF"
        border:
          subtle: "#E5E6EB"
          default: "#D9DDE3"
          strong: "#C9CDD4"
          focus: "#69B1FF"
        state:
          success: "#00B42A"
          success_bg: "#E8FFEA"
          warning: "#FF7D00"
          warning_bg: "#FFF3E8"
          error: "#F53F3F"
          error_bg: "#FFECE8"
          info: "#1677FF"
          info_bg: "#E6F4FF"
        data:
          blue: "#1677FF"
          cyan: "#13C2C2"
          green: "#52C41A"
          gold: "#FAAD14"
          orange: "#FA8C16"
          red: "#F5222D"
          purple: "#722ED1"
      dark:
        brand:
          primary: "#3C89FF"
          primary_hover: "#69A7FF"
          primary_active: "#1D5FD1"
          primary_bg: "rgba(22,119,255,0.20)"
          primary_border: "#1668DC"
        neutral:
          bg_canvas: "#0F1115"
          bg_layout: "#14171A"
          bg_container: "#1D2129"
          bg_elevated: "#232830"
          bg_mask: "rgba(0,0,0,0.60)"
        text:
          primary: "#F2F3F5"
          secondary: "#C9CDD4"
          tertiary: "#86909C"
          quaternary: "#4E5969"
          inverse: "#1F2329"
        border:
          subtle: "#2A2F38"
          default: "#3A3F47"
          strong: "#4E5969"
          focus: "#69A7FF"
        state:
          success: "#23C343"
          success_bg: "rgba(0,180,42,0.18)"
          warning: "#FF9A2E"
          warning_bg: "rgba(255,125,0,0.18)"
          error: "#FF6B6B"
          error_bg: "rgba(245,63,63,0.18)"
          info: "#3C89FF"
          info_bg: "rgba(22,119,255,0.20)"
        data:
          blue: "#3C89FF"
          cyan: "#33D1C9"
          green: "#7BE188"
          gold: "#FFD666"
          orange: "#FFB65D"
          red: "#FF7875"
          purple: "#B37FEB"
    typography:
      family:
        sans: "\"Inter\",\"PingFang SC\",\"Helvetica Neue\",Arial,system-ui,sans-serif"
        mono: "\"JetBrains Mono\",\"SF Mono\",\"Menlo\",monospace"
      size:
        hero: "36px"
        h1: "28px"
        h2: "24px"
        h3: "20px"
        title: "16px"
        body: "14px"
        body_small: "13px"
        caption: "12px"
      line_height:
        hero: 1.22
        h1: 1.3
        h2: 1.34
        h3: 1.4
        title: 1.5
        body: 1.57
        caption: 1.5
      weight:
        regular: 400
        medium: 500
        semibold: 600
    space:
      0: "0px"
      1: "4px"
      2: "8px"
      3: "12px"
      4: "16px"
      5: "20px"
      6: "24px"
      8: "32px"
      10: "40px"
      12: "48px"
      16: "64px"
    radius:
      xs: "4px"
      sm: "6px"
      md: "8px"
      lg: "12px"
      xl: "16px"
      xxl: "20px"
      pill: "999px"
    shadow:
      card: "0 2px 8px rgba(31,35,41,0.06)"
      float: "0 6px 20px rgba(31,35,41,0.10)"
      drawer: "0 10px 30px rgba(31,35,41,0.16)"
      focus_ring: "0 0 0 3px rgba(22,119,255,0.20)"
    border:
      width:
        hairline: "1px"
        strong: "2px"
    motion:
      duration:
        fast: "120ms"
        standard: "180ms"
        slow: "240ms"
      easing:
        standard: "cubic-bezier(0.2, 0, 0, 1)"
        enter: "cubic-bezier(0, 0, 0, 1)"
        exit: "cubic-bezier(0.3, 0, 1, 1)"
      reduce_motion:
        fast: "0ms"
        standard: "80ms"
    breakpoint:
      phone: "0-767px"
      tablet: "768-1199px"
      desktop: "1200-1599px"
      wide: "1600px+"
    container:
      phone: "100% with 16px gutters"
      tablet: "100% with 24px gutters"
      desktop: "1440px max with 24px gutters"
      wide: "1600px max with 32px gutters"
    component:
      button:
        height:
          sm: "28px"
          md: "36px"
          lg: "40px"
      input:
        height:
          sm: "32px"
          md: "36px"
          lg: "40px"
      table:
        row_height:
          compact: "40px"
          default: "48px"
      nav:
        header_height:
          desktop: "56px"
          mobile: "52px"
        sidebar_width:
          desktop: "240px"
          collapsed: "64px"
---

# DESIGN.md

## 1. Visual Theme & Atmosphere

这是一个面向后台管理系统的企业级极简风设计系统。界面应清楚、有秩序、低噪声、强调效率与可读性，并继承 Ant Design 风格中对数据、表单、表格、抽屉和复杂业务交互的成熟处理方式。

- 用于: SaaS 后台、运营平台、审核系统、风控系统、CRM/ERP、中台工具、工单/审批系统。
- 不用于: 强营销导向官网、娱乐产品、潮流视觉导向产品。
- AI 应用规则: 优先使用清晰布局、卡片化模块和企业中性色，不要引入消费型夸张插画、玻璃质感或高饱和大面积渐变。

## 2. Color Palette & Roles

- `brand.primary` 用于主要 CTA、激活态、链接和进度性强调。
- `neutral.bg_*` 是页面骨架，负责 layout、card、drawer、mask 的层级。
- `text.*` 控制信息密度与层级，是后台可读性的核心。
- `border.*` 用于分组、表格、输入框、工具栏和分割。
- `state.*` 用于消息、标签、状态提示和结果反馈。
- AI 应用规则: 后台界面优先用中性色构建骨架，只在关键操作和状态处使用品牌色或状态色。

## 3. Typography Rules

- 默认正文 `14px`，兼顾信息密度与办公场景阅读舒适度。
- 表格与表单标签可使用 `13px`。
- 页面标题与卡片标题层级明确，但不追求大视觉冲击。
- AI 应用规则: Dashboard 可使用 `24-28px` 页面标题，表格、筛选区、字段说明维持 `12-14px` 为主。

## 4. Spacing & Layout

- 基础 spacing 为 `4px`，常用节奏 `8/12/16/24/32`。
- 列表页和筛选区强调横纵对齐，避免自由散排。
- 卡片内边距优先 `16px / 20px / 24px`。
- AI 应用规则: 用间距和卡片容器组织复杂度，避免同时叠加过多边框、阴影和背景色。

## 5. Component Styling

- Button: 企业风、简洁、边界清晰；默认圆角 `8px`。
- Input / Select: 默认 `36px` 高，支持紧凑高密度后台使用。
- Card: 轻阴影 + 细边框，白底或深色容器底。
- Table: 清晰表头、斑马纹可选、hover 行高亮、操作列固定。
- AI 应用规则: 后台核心组件必须有 `default / hover / active / disabled / focus / error / loading` 状态。

## 6. Depth, Borders & Elevation

- 主内容区优先平面卡片系统，不堆叠重阴影。
- 抽屉、弹窗、悬浮过滤器才使用 `shadow.float` 或 `shadow.drawer`。
- AI 应用规则: 层级优先靠布局与容器背景区分，阴影只用于高层交互。

## 7. Icons, Imagery & Illustration

- 图标优先企业工具风线性图标，视觉清晰，适合表格、按钮、菜单、状态。
- 默认尺寸 `14px / 16px / 18px`，桌面工具栏可用 `12px` 辅助图标。
- AI 应用规则: 后台页面优先图标辅助理解，不用大面积插画破坏效率。

## 8. Motion & Interaction

- 动效短、轻、准，服务于抽屉、筛选展开、表格反馈和状态变化。
- AI 应用规则: 管理后台不使用夸张进场动画，重点保证 hover、focus、drawer、modal 的反馈连贯。

## 9. Responsive Behavior

- `0-767px`: 将复杂表格转为卡片或摘要列表，重要操作前置。
- `768-1199px`: 支持折叠 sidebar、双栏信息、抽屉详情。
- `1200-1599px`: 标准后台布局，Sidebar + Header + Content + Filter Bar。
- `1600px+`: 支持多卡片仪表盘、宽表格和右侧详情区。
- AI 应用规则: PC 优先，Mobile 作为业务核心能力的收缩版，不保留完整桌面密度。

## 10. Accessibility Rules

- 正文对比度目标至少 `4.5:1`。
- 桌面最小点击热区 `32px`，移动端至少 `44px`。
- Focus 必须明显可见，不能仅靠 hover。
- 深色模式采用独立 token，不简单反相。
- AI 应用规则: 后台高密度不等于低可读性，字段、按钮、标签和状态必须足够清晰。

## 11. Do's and Don'ts

- Do: 使用 Header、Sidebar、Breadcrumb、Page Header 和 Card 明确业务层级。
- Do: 用 Drawer 处理行内详情与编辑，而不是频繁跳页。
- Do: 在 Dashboard 中用卡片和图表分组组织复杂信息。
- Don't: 用营销型 Hero、超大留白和装饰插画替代业务结构。
- Don't: 在表单、表格、筛选器中使用过圆或过松散的移动端样式。
- Don't: 把所有信息都塞进一个大白板区域。

## 12. Agent Prompt Guide

- 先读: `design/enterprise-admin-minimal-ant-card/DESIGN.md`
- 再读: `design/enterprise-admin-minimal-ant-card/tokens.json`
- 扩展说明: `design/enterprise-admin-minimal-ant-card/visual-spec.md`
- 视觉核芯 8 个 token:
  - `color.light.brand.primary`
  - `color.light.neutral.bg_container`
  - `color.dark.neutral.bg_container`
  - `typography.size.body`
  - `space.4`
  - `radius.md`
  - `shadow.card`
  - `component.table.row_height.default`
- 组件默认:
  - 按钮默认 `36px`
  - 输入框默认 `36px`
  - 表格默认行高 `48px`
  - 抽屉优先用于详情编辑
- 布局默认:
  - Desktop: Header + Sidebar + Content
  - Tablet: 折叠 Sidebar + Drawer
  - Mobile: 卡片化任务流
- 硬约束:
  - 不引入玻璃、霓虹、营销插画风
  - 不用厚重阴影覆盖页面
  - 不让深色模式退化为简单反相
  - 不把手机端做成缩小桌面端
- 生成代码或 Figma 时保留 token 名称。

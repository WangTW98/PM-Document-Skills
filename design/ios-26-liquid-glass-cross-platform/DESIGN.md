---
design_system:
  name: "iOS 26 Liquid Glass Cross-Platform"
  version: "0.1.0"
  intent: "为 iPhone App、H5 Mobile Web、iPad/Tablet、PC Web 提供高级、简洁、Apple 感的跨端响应式设计系统，基于可读性优先的 Liquid Glass 材质语言。"
  tokens:
    color:
      light:
        background:
          canvas: "#F4F5F7"
          elevated: "#FFFFFF"
          sunken: "#ECEEF2"
        text:
          primary: "#111318"
          secondary: "#4D5562"
          tertiary: "#7E8794"
          inverse: "#F8FAFC"
        action:
          primary: "#0A84FF"
          primary_hover: "#0077ED"
          primary_pressed: "#0063C7"
          secondary: "#E7F1FF"
          secondary_hover: "#D5E8FF"
          destructive: "#FF453A"
        border:
          subtle: "rgba(17,19,24,0.08)"
          strong: "rgba(17,19,24,0.14)"
          focus: "rgba(10,132,255,0.42)"
        glass:
          fill: "rgba(255,255,255,0.56)"
          fill_strong: "rgba(255,255,255,0.72)"
          stroke: "rgba(255,255,255,0.44)"
          highlight: "rgba(255,255,255,0.82)"
          shadow: "rgba(15,23,42,0.12)"
        state:
          success: "#30D158"
          warning: "#FF9F0A"
          error: "#FF453A"
          info: "#64D2FF"
      dark:
        background:
          canvas: "#0D1117"
          elevated: "#151B24"
          sunken: "#0A0E14"
        text:
          primary: "#F5F7FA"
          secondary: "#C4CBD6"
          tertiary: "#8892A0"
          inverse: "#101318"
        action:
          primary: "#4DA3FF"
          primary_hover: "#6BB2FF"
          primary_pressed: "#2D8EFF"
          secondary: "rgba(77,163,255,0.18)"
          secondary_hover: "rgba(77,163,255,0.24)"
          destructive: "#FF6961"
        border:
          subtle: "rgba(255,255,255,0.10)"
          strong: "rgba(255,255,255,0.18)"
          focus: "rgba(77,163,255,0.48)"
        glass:
          fill: "rgba(18,24,33,0.58)"
          fill_strong: "rgba(18,24,33,0.74)"
          stroke: "rgba(255,255,255,0.14)"
          highlight: "rgba(255,255,255,0.20)"
          shadow: "rgba(0,0,0,0.42)"
        state:
          success: "#32D74B"
          warning: "#FFD60A"
          error: "#FF6961"
          info: "#64D2FF"
    typography:
      family:
        sans: "\"SF Pro Display\",\"SF Pro Text\",\"PingFang SC\",\"Inter\",\"Helvetica Neue\",system-ui,sans-serif"
        mono: "\"SF Mono\",\"JetBrains Mono\",\"Menlo\",monospace"
      size:
        hero: "40px"
        h1: "32px"
        h2: "24px"
        h3: "20px"
        title: "18px"
        body: "16px"
        body_small: "14px"
        caption: "12px"
      line_height:
        hero: 1.15
        h1: 1.2
        h2: 1.25
        h3: 1.3
        body: 1.5
        caption: 1.4
      weight:
        regular: 400
        medium: 500
        semibold: 600
        bold: 700
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
      20: "80px"
    radius:
      xs: "8px"
      sm: "12px"
      md: "16px"
      lg: "20px"
      xl: "24px"
      xxl: "28px"
      pill: "999px"
    shadow:
      glass_float: "0 8px 24px rgba(15,23,42,0.12), 0 1px 0 rgba(255,255,255,0.4) inset"
      glass_modal: "0 24px 60px rgba(15,23,42,0.18), 0 1px 0 rgba(255,255,255,0.28) inset"
      soft_card: "0 6px 20px rgba(15,23,42,0.08)"
      focus_ring: "0 0 0 4px rgba(10,132,255,0.18)"
    border:
      width:
        hairline: "1px"
        strong: "1.5px"
      blur:
        nav: "20px"
        card: "24px"
        modal: "30px"
      saturate:
        glass: 1.2
    motion:
      duration:
        fast: "120ms"
        standard: "180ms"
        slow: "280ms"
      easing:
        standard: "cubic-bezier(0.2, 0.8, 0.2, 1)"
        entrance: "cubic-bezier(0.16, 1, 0.3, 1)"
        exit: "cubic-bezier(0.4, 0, 1, 1)"
      reduce_motion:
        fast: "0ms"
        standard: "80ms"
    breakpoint:
      phone: "0-767px"
      tablet: "768-1199px"
      desktop: "1200-1439px"
      wide: "1440px+"
    container:
      phone: "100% with 16px gutters"
      tablet: "100% with 24px gutters"
      desktop: "1200px max with 32px gutters"
      wide: "1440px max with 40px gutters"
    component:
      button:
        height:
          compact: "36px"
          default: "44px"
          large: "52px"
      input:
        height:
          default: "48px"
      nav:
        topbar_height:
          mobile: "56px"
          desktop: "64px"
      sidebar:
        width:
          tablet: "280px"
          desktop: "296px"
---

# DESIGN.md

## 1. Visual Theme & Atmosphere

使用清透但克制的玻璃语言。界面核心不是“炫光”，而是“信息分层明确 + 浮层自然存在 + 内容始终可读”。

- 用于: 顶部导航、底部工具栏、悬浮筛选器、Popover、模态、关键操作卡片。
- 不用于: 长篇正文背景、复杂表格底板、低对比图表主绘图区。
- AI 应用规则: 默认先布置清晰内容层，再在导航与浮层添加玻璃材质，避免整页都玻璃化。

## 2. Color Palette & Roles

- `background.canvas` 是页面主底色；浅色模式用雾白灰，深色模式用低噪石墨。
- `glass.fill` 只用于浮层；若底图复杂或对比不足，升级为 `glass.fill_strong`。
- `action.primary` 用于主要 CTA、激活标签、选中态。
- `state.*` 必须搭配文字或图标，不允许仅靠颜色传达状态。
- AI 应用规则: 文本优先使用 `text.primary` / `text.secondary`，不要直接把高亮蓝用于正文文字。

## 3. Typography Rules

- 端内默认字体栈使用 SF Pro / PingFang / Inter 兼容组合。
- iPhone 与 H5 正文字号默认 `16px`，辅助文案 `14px`，说明文 `12px`。
- PC Web Dashboard 表格正文可下调为 `14px`，但交互标签与按钮不能低于 `14px`。
- AI 应用规则: 移动端标题层级压缩为 `32/24/20/18/16/14/12`；桌面端可增加 `40px hero` 但不能无限放大。

## 4. Spacing & Layout

- 基础步进为 `4px`，常用节奏为 `8/12/16/24/32`。
- 卡片内边距: Mobile `16px`，Tablet `20px`，Desktop `24px`。
- 区块间距: Mobile `24px`，Tablet `32px`，Desktop `40px`。
- AI 应用规则: 小屏优先减少并列关系，保留主轴间距，不要通过压缩文字行高换空间。

## 5. Component Styling

- Primary Button: 实底高对比，`44px` 最小高度，`pill` 圆角。
- Secondary Button: 淡色底或轻玻璃底，文字保持主色。
- Input: `48px` 高度，内边距 `0 16px`，聚焦态边框 + 外环。
- Card: 优先使用 `radius.lg` 与 `soft_card` 或 `glass_float`。
- AI 应用规则: 组件默认应有 `default / hover / pressed / disabled / focus / error` 状态定义。

## 6. Depth, Borders & Elevation

- 玻璃层使用“高光内描边 + 柔和投影 + 背景模糊”组合，而不是强阴影。
- 层级建议:
  - L0 页面背景
  - L1 普通内容卡片
  - L2 玻璃导航 / 悬浮面板
  - L3 模态 / Popover / 命令面板
- AI 应用规则: 阴影只用于拉开层级，不能替代结构边界。

## 7. Icons, Imagery & Illustration

- 图标采用 2px 视觉笔画、圆角端点、简化几何造型。
- 默认尺寸 `18/20/24px`；触控场景点击热区至少 `44px`。
- 插图与空状态图应低饱和，避免破坏整体克制感。
- AI 应用规则: 优先线性图标；只有状态结果页允许少量面性高光。

## 8. Motion & Interaction

- 标准过渡 `180ms`，进入动画 `180-280ms`，退出动画 `120-180ms`。
- 常见动效: 浮层轻微上浮、导航背景渐显、列表插入淡入、分栏切换平移。
- 禁止: 大幅弹跳、强缩放、连续视差。
- AI 应用规则: 动效帮助理解层级，不作为装饰表演。

## 9. Responsive Behavior

- `0-767px`: 单列内容，底部操作优先，表格转卡片或横向滑动摘要。
- `768-1199px`: 支持 Sidebar、Split View、Popover；列表与详情可并置。
- `1200-1439px`: 顶栏 + Sidebar + 主内容区；表格、面包屑、过滤器完整呈现。
- `1440px+`: 增加留白和多列信息密度，不无限拉宽阅读列。
- AI 应用规则: PC 不得简单放大 Mobile；必须重构导航、信息密度与 hover 反馈。

## 10. Accessibility Rules

- 正文对比度目标至少 `4.5:1`，大字至少 `3:1`。
- 触控最小点击区 `44x44px`；桌面 hover 元素仍需 focus 可见。
- `Reduce Transparency`: 玻璃层退化为 `background.elevated` + 常规边框，blur 设为 `0`。
- `Reduce Motion`: 全局动画时长降至 `0-80ms`，移除位移动画，仅保留显隐。
- AI 应用规则: 任何状态变化必须有文字、图标或结构反馈，不可只靠颜色或动画。

## 11. Do's and Don'ts

- Do: 让导航、筛选、关键操作悬浮，保持主体内容平稳。
- Do: 在复杂背景上自动提高玻璃底不透明度。
- Do: 在 H5 场景为底部 CTA 预留安全区。
- Don't: 用大面积模糊破坏表格、表单、正文可读性。
- Don't: 把所有卡片都做成强玻璃层。
- Don't: 在桌面端保留纯移动端底部 Tab 作为唯一导航。

## 12. Agent Prompt Guide

- 先读: `design/ios-26-liquid-glass-cross-platform/DESIGN.md`
- 再读: `design/ios-26-liquid-glass-cross-platform/tokens.json`
- 扩展说明: `design/ios-26-liquid-glass-cross-platform/visual-spec.md`
- 视觉核芯 8 个 token:
  - `color.light.glass.fill`
  - `color.dark.glass.fill`
  - `color.light.action.primary`
  - `typography.size.body`
  - `space.4`
  - `radius.pill`
  - `shadow.glass_float`
  - `border.blur.card`
- 组件默认:
  - 主按钮 44px 高，胶囊圆角。
  - 输入框 48px 高，聚焦时有边框与外环。
  - 顶部导航在各端均为浮层，不与页面背景硬切。
- 布局默认:
  - Mobile 单列。
  - Tablet 可双栏 + Sidebar。
  - Desktop 三段式信息架构。
- 硬约束:
  - 不把玻璃用于长文本主底。
  - 不让 PC 复制手机信息层级。
  - 不破坏浅/深色与无障碍降级规则。
- 生成代码或 Figma 时保留 token 名称，不要随意重命名为框架私有变量。

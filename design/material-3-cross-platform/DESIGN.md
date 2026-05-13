---
design_system:
  name: "Material Design 3 Cross-Platform"
  version: "0.1.0"
  intent: "为移动端、Tablet、PC Web、H5 Web 提供一套遵循 Material Design 3 的现代化设计系统，突出 Color Roles、Surface System、可访问性、组件状态和响应式布局。"
  tokens:
    color:
      light:
        primary:
          base: "#6750A4"
          on: "#FFFFFF"
          container: "#EADDFF"
          on_container: "#21005D"
        secondary:
          base: "#625B71"
          on: "#FFFFFF"
          container: "#E8DEF8"
          on_container: "#1D192B"
        tertiary:
          base: "#7D5260"
          on: "#FFFFFF"
          container: "#FFD8E4"
          on_container: "#31111D"
        error:
          base: "#B3261E"
          on: "#FFFFFF"
          container: "#F9DEDC"
          on_container: "#410E0B"
        surface:
          background: "#FFFBFE"
          surface: "#FFFBFE"
          surface_dim: "#DED8E1"
          surface_bright: "#FFFBFE"
          surface_container_low: "#F7F2FA"
          surface_container: "#F3EDF7"
          surface_container_high: "#ECE6F0"
          surface_container_highest: "#E6E0E9"
        text:
          primary: "#1C1B1F"
          secondary: "#49454F"
          tertiary: "#79747E"
          inverse: "#F4EFF4"
        outline:
          base: "#79747E"
          variant: "#CAC4D0"
          focus: "#6750A4"
        state:
          success: "#146C2E"
          on_success: "#FFFFFF"
          success_container: "#C4EED0"
          warning: "#8A4F00"
          on_warning: "#FFFFFF"
          warning_container: "#FFDDB5"
          info: "#0057D8"
          on_info: "#FFFFFF"
          info_container: "#D7E3FF"
      dark:
        primary:
          base: "#D0BCFF"
          on: "#381E72"
          container: "#4F378B"
          on_container: "#EADDFF"
        secondary:
          base: "#CCC2DC"
          on: "#332D41"
          container: "#4A4458"
          on_container: "#E8DEF8"
        tertiary:
          base: "#EFB8C8"
          on: "#492532"
          container: "#633B48"
          on_container: "#FFD8E4"
        error:
          base: "#F2B8B5"
          on: "#601410"
          container: "#8C1D18"
          on_container: "#F9DEDC"
        surface:
          background: "#141218"
          surface: "#141218"
          surface_dim: "#141218"
          surface_bright: "#3B383E"
          surface_container_low: "#1D1B20"
          surface_container: "#211F26"
          surface_container_high: "#2B2930"
          surface_container_highest: "#36343B"
        text:
          primary: "#E6E1E5"
          secondary: "#CAC4D0"
          tertiary: "#938F99"
          inverse: "#313033"
        outline:
          base: "#938F99"
          variant: "#49454F"
          focus: "#D0BCFF"
        state:
          success: "#8CD9A2"
          on_success: "#003912"
          success_container: "#00531D"
          warning: "#FFB95C"
          on_warning: "#4A2800"
          warning_container: "#683C00"
          info: "#ADC6FF"
          on_info: "#002E69"
          info_container: "#0042A3"
    typography:
      family:
        brand: "\"Roboto Flex\",\"Roboto\",\"Noto Sans SC\",\"PingFang SC\",system-ui,sans-serif"
        plain: "\"Roboto\",\"Noto Sans SC\",\"PingFang SC\",system-ui,sans-serif"
        mono: "\"Roboto Mono\",\"SF Mono\",\"JetBrains Mono\",monospace"
      scale:
        display_large: "57px"
        display_medium: "45px"
        headline_large: "32px"
        headline_medium: "28px"
        title_large: "22px"
        title_medium: "16px"
        body_large: "16px"
        body_medium: "14px"
        label_large: "14px"
        label_medium: "12px"
      line_height:
        display_large: "64px"
        display_medium: "52px"
        headline_large: "40px"
        headline_medium: "36px"
        title_large: "28px"
        title_medium: "24px"
        body_large: "24px"
        body_medium: "20px"
        label_large: "20px"
        label_medium: "16px"
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
      20: "80px"
    radius:
      xs: "4px"
      sm: "8px"
      md: "12px"
      lg: "16px"
      xl: "20px"
      xxl: "28px"
      full: "999px"
    shadow:
      level_0: "none"
      level_1: "0 1px 2px rgba(0,0,0,0.12), 0 1px 3px rgba(0,0,0,0.08)"
      level_2: "0 2px 6px rgba(0,0,0,0.14), 0 1px 2px rgba(0,0,0,0.08)"
      level_3: "0 4px 12px rgba(0,0,0,0.16), 0 2px 4px rgba(0,0,0,0.10)"
      level_4: "0 8px 24px rgba(0,0,0,0.18), 0 4px 8px rgba(0,0,0,0.10)"
      focus_ring: "0 0 0 3px rgba(103,80,164,0.22)"
    border:
      width:
        hairline: "1px"
        strong: "2px"
    motion:
      duration:
        short_1: "100ms"
        short_2: "150ms"
        medium_1: "200ms"
        medium_2: "250ms"
        long_1: "350ms"
      easing:
        standard: "cubic-bezier(0.2, 0, 0, 1)"
        emphasized: "cubic-bezier(0.2, 0, 0, 1)"
        decelerate: "cubic-bezier(0, 0, 0, 1)"
        accelerate: "cubic-bezier(0.3, 0, 1, 1)"
      reduce_motion:
        short: "0ms"
        medium: "80ms"
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
          sm: "32px"
          md: "40px"
          lg: "48px"
      input:
        height:
          md: "56px"
      navigation:
        top_app_bar:
          mobile: "64px"
          desktop: "72px"
        navigation_rail: "80px"
        sidebar: "280px"
---

# DESIGN.md

## 1. Visual Theme & Atmosphere

这是一个基于 Material Design 3 的现代设计系统。界面应清晰、有序、温和、可信，并通过色彩角色与 surface 层级而不是装饰性效果建立结构。

- 用于: 工具型产品、B 端管理、内容效率、服务流程、跨端后台和前台应用。
- 不用于: 强拟物、霓虹感、极端品牌化、实验性艺术 UI。
- AI 应用规则: 优先使用标准 color roles、surface container 和状态层，不要添加无规范依据的玻璃、渐变和重阴影。

## 2. Color Palette & Roles

- `primary` 用于关键 CTA、选中态、品牌性强调。
- `secondary` 用于补充操作、次级强调和中性互动元素。
- `tertiary` 用于辅助识别、图表区分、内容点缀，但不能替代 `primary`。
- `surface.*` 负责层级与容器关系，是页面骨架。
- `outline` 用于边界和分割，不承担主要视觉重量。
- AI 应用规则: 任何组件优先使用语义角色，不直接写死原始色值。

## 3. Typography Rules

- 采用 Material 3 风格字阶：Display、Headline、Title、Body、Label。
- 移动端正文默认 `body_large = 16px / 24px`。
- 表格、标签、密集控件可用 `body_medium = 14px / 20px`。
- AI 应用规则: 标题层级不超过三级，表单和数据视图优先可读性，避免过小字。

## 4. Spacing & Layout

- 基础 spacing 采用 `4px` 递进。
- 组件内边距优先 `16px / 24px`。
- 区块间距优先 `24px / 32px / 40px`。
- AI 应用规则: 使用间距建立秩序，不依赖边框堆砌结构。

## 5. Component Styling

- Button: 圆角 `full` 或 `lg`，有明确 container / label / state layer。
- Input: 默认 `56px` 高，outline 或 filled 两种模式之一，不混用。
- Card: 使用 `surface_container` 系列建立层级。
- Navigation: Mobile 使用 top app bar + bottom navigation；Desktop 使用 top bar + sidebar/rail。
- AI 应用规则: 所有交互组件都必须输出状态层和 focus 可见性。

## 6. Depth, Borders & Elevation

- Elevation 使用 M3 风格柔和阴影，不使用尖锐投影。
- Surface 层级优先通过 `surface_container_low -> highest` 区分，而不是只靠阴影。
- AI 应用规则: 表单、卡片、弹层先选 surface 层级，再决定是否增加 elevation。

## 7. Icons, Imagery & Illustration

- 图标采用 Material Symbols 风格或近似风格，圆角几何、清晰轮廓。
- 默认尺寸 `20px / 24px`，复杂工具栏可用 `18px`。
- 插图与空状态图应简洁、友好、低噪声。
- AI 应用规则: 优先使用线性或轻面性图标，保持统一笔画逻辑。

## 8. Motion & Interaction

- 采用短促、顺滑、可预期的过渡。
- 状态变化主要通过 state layer、颜色、阴影和内容变化体现。
- AI 应用规则: 动效服务于过渡和反馈，不做装饰性大范围运动。

## 9. Responsive Behavior

- `0-767px`: 单列布局，底部导航或主要操作靠近拇指区。
- `768-1199px`: 支持 Navigation Rail、双栏、Split View。
- `1200-1439px`: 支持 Sidebar、表格工具栏、详情面板。
- `1440px+`: 保持内容密度与阅读宽度，不无限拉伸文本列。
- AI 应用规则: 不同端必须重组导航和信息密度，而不是按比例缩放。

## 10. Accessibility Rules

- 正文对比度目标至少 `4.5:1`。
- 点击热区至少 `44x44px`，桌面小图标按钮建议 `40x40px` 起。
- Focus 必须清晰可见。
- Dark mode 采用独立 token，不做简单反相。
- AI 应用规则: 状态变化必须有颜色以外的辅助信号，如图标、文案或结构变化。

## 11. Do's and Don'ts

- Do: 使用 surface container 系统建立结构层次。
- Do: 使用 color roles 和 state layer 管理组件状态。
- Do: 在 Tablet 和 Desktop 引入 rail、sidebar、table、detail pane。
- Don't: 用强品牌大色块覆盖所有页面。
- Don't: 用过多阴影取代语义层级。
- Don't: 在小屏维持桌面级表格密度。

## 12. Agent Prompt Guide

- 先读: `design/material-3-cross-platform/DESIGN.md`
- 再读: `design/material-3-cross-platform/tokens.json`
- 扩展说明: `design/material-3-cross-platform/visual-spec.md`
- 视觉核芯 8 个 token:
  - `color.light.primary.base`
  - `color.light.surface.surface_container`
  - `color.dark.surface.surface_container`
  - `typography.scale.body_large`
  - `space.4`
  - `radius.lg`
  - `shadow.level_2`
  - `component.input.height.md`
- 组件默认:
  - 主按钮优先 filled button
  - 输入框默认 `56px` 高
  - 卡片优先用 surface container 高低层区分
- 布局默认:
  - Mobile 单列
  - Tablet 双栏或 rail
  - Desktop sidebar + content + optional detail pane
- 硬约束:
  - 不新增未定义 token
  - 不跳过浅/深模式
  - 不让 PC 成为手机布局放大版
- 生成代码或 Figma 时保留 token 名称。

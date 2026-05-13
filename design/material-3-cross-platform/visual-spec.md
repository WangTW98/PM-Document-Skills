# Material Design 3 跨端设计规范

## 1. 设计系统总览

本规范面向移动端、Tablet、PC Web、H5 Web 的统一设计建设，采用 Material Design 3 作为基础方法论，重点围绕 `Material You`、`Dynamic Color`、`Color Roles`、`Surface System`、`Tonal Palette`、`Rounded Geometry`、`Accessibility First`、`Responsive Layout`、`Component States`、`Clear Hierarchy` 建立可落地的视觉语言。

由于当前未收到具体产品类型、目标用户、品牌关键词扩展、核心功能清单与页面范围，本规范默认适用于通用工具型、服务型、管理型产品，并已将这一假设写入文档，便于后续二次调优。

## 2. 设计原则

1. 语义优先：优先通过 color roles、surface 层级和 typography 表达结构，而不是装饰。
2. 可访问性优先：所有模式和状态都以可读、可点、可聚焦、可理解为前提。
3. 一致但不死板：统一 token、统一组件逻辑，但允许不同端按使用场景重构布局。
4. 响应式重构：Mobile、Tablet、Desktop 共享视觉语言，不共享单一布局。
5. 状态明确：hover、focus、pressed、selected、disabled、error 必须完整。
6. 组件化复用：设计规范必须直接映射为 Figma 变量/组件和前端 token/component API。

## 3. 视觉风格规范

- 风格基调：清晰、秩序、现代、友好
- 视觉结构：
  - 使用清楚的 surface 层级替代厚重边框
  - 使用圆润几何增强亲和力
  - 使用柔和阴影表示 elevation
  - 使用合理留白提升扫描效率
- 禁止事项：
  - 过度品牌化渐变
  - 极细浅灰字导致可读性下降
  - 仅靠颜色区分交互状态

## 4. 色彩系统

### 4.1 Color Roles

浅色模式：

| Token | Value | 用途 |
| --- | --- | --- |
| `primary.base` | `#6750A4` | 主操作、品牌强调 |
| `on.primary` | `#FFFFFF` | 主操作上的文字 |
| `primary.container` | `#EADDFF` | 主色容器、选中底 |
| `on.primary.container` | `#21005D` | 主色容器文字 |
| `secondary.base` | `#625B71` | 次级操作 |
| `tertiary.base` | `#7D5260` | 补充强调 |
| `error.base` | `#B3261E` | 错误态 |
| `surface` | `#FFFBFE` | 页面与基础表面 |
| `surface.container` | `#F3EDF7` | 默认容器层 |
| `surface.container.high` | `#ECE6F0` | 上层容器 |
| `outline.base` | `#79747E` | 基础边界 |
| `outline.variant` | `#CAC4D0` | 分割线、轻边界 |

深色模式：

| Token | Value | 用途 |
| --- | --- | --- |
| `primary.base` | `#D0BCFF` | 主操作、品牌强调 |
| `on.primary` | `#381E72` | 主操作上的文字 |
| `primary.container` | `#4F378B` | 主色容器 |
| `secondary.base` | `#CCC2DC` | 次级操作 |
| `tertiary.base` | `#EFB8C8` | 补充强调 |
| `error.base` | `#F2B8B5` | 错误态 |
| `surface` | `#141218` | 页面背景 |
| `surface.container` | `#211F26` | 默认容器层 |
| `surface.container.high` | `#2B2930` | 上层容器 |
| `outline.base` | `#938F99` | 基础边界 |
| `outline.variant` | `#49454F` | 分割线、轻边界 |

### 4.2 Tonal Palette 使用规则

- `primary` 负责品牌和核心操作。
- `secondary` 用于补充动作、分类与辅助强调。
- `tertiary` 用于图表、差异化内容或辅助板块，不代替主 CTA。
- `surface` 家族承担页面结构层次，是 Material 3 的骨架。
- `error` 使用专属容器色，不将 `error.base` 直接大面积铺底。

### 4.3 状态色

| Token | Light | Dark | 用途 |
| --- | --- | --- | --- |
| `success` | `#146C2E` | `#8CD9A2` | 成功 |
| `warning` | `#8A4F00` | `#FFB95C` | 风险提醒 |
| `info` | `#0057D8` | `#ADC6FF` | 信息态 |

## 5. Surface System

Material 3 页面不依赖大量卡片阴影，而依赖 surface 层级：

| Layer | Light | Dark | 用途 |
| --- | --- | --- | --- |
| `surface` | `#FFFBFE` | `#141218` | 页面基础表面 |
| `surface.container.low` | `#F7F2FA` | `#1D1B20` | 轻容器 |
| `surface.container` | `#F3EDF7` | `#211F26` | 默认卡片 / panel |
| `surface.container.high` | `#ECE6F0` | `#2B2930` | 高层容器 |
| `surface.container.highest` | `#E6E0E9` | `#36343B` | Modal / 强调面 |

规则：

- 页面背景与主内容区差异不宜过大，保持温和层次。
- 表单、卡片、侧边栏、过滤器优先通过 surface 区分。
- 只有弹层、菜单、对话框、悬浮面板才明显增加 elevation。

## 6. 字体系统

### 6.1 字体族

- Brand / Display: `Roboto Flex`, `Roboto`, `Noto Sans SC`, `PingFang SC`, `system-ui`
- Plain: `Roboto`, `Noto Sans SC`, `PingFang SC`, `system-ui`
- Mono: `Roboto Mono`, `SF Mono`, `JetBrains Mono`, `monospace`

### 6.2 Type Scale

| Role | Size | Line Height | Weight | 用途 |
| --- | --- | --- | --- | --- |
| Display Large | `57px` | `64px` | `400` | 大屏品牌首屏 |
| Display Medium | `45px` | `52px` | `400` | 重要首屏标题 |
| Headline Large | `32px` | `40px` | `400` | 页面标题 |
| Headline Medium | `28px` | `36px` | `400` | 子页面标题 |
| Title Large | `22px` | `28px` | `400` | 卡片区块标题 |
| Title Medium | `16px` | `24px` | `500` | 表单标题、列表组标题 |
| Body Large | `16px` | `24px` | `400` | 默认正文 |
| Body Medium | `14px` | `20px` | `400` | 紧凑正文 |
| Label Large | `14px` | `20px` | `500` | 按钮、输入标签 |
| Label Medium | `12px` | `16px` | `500` | 辅助标签 |

## 7. 间距系统

基础 spacing 单位为 `4px`。

| Token | Value | 用途 |
| --- | --- | --- |
| `space.1` | `4px` | 小间隔 |
| `space.2` | `8px` | Icon 与文案 |
| `space.3` | `12px` | 小型组件内边距 |
| `space.4` | `16px` | 默认内边距 |
| `space.5` | `20px` | Tablet 组件间距 |
| `space.6` | `24px` | 模块间距 |
| `space.8` | `32px` | 区块间距 |
| `space.10` | `40px` | 页面大区块 |
| `space.12` | `48px` | 大屏分区 |

布局规则：

- Mobile 内容容器左右边距 `16px`
- Tablet `24px`
- Desktop `32px`
- Wide Desktop `40px`

## 8. 圆角系统

| Token | Value | 用途 |
| --- | --- | --- |
| `radius.xs` | `4px` | 小标签、表格 cell |
| `radius.sm` | `8px` | 菜单项、输入框 |
| `radius.md` | `12px` | 普通卡片、表单控件 |
| `radius.lg` | `16px` | Filled Card、Dialog、小型面板 |
| `radius.xl` | `20px` | Modal、大卡片 |
| `radius.xxl` | `28px` | Hero 卡、展示性模块 |
| `radius.full` | `999px` | Chip、FAB、Pill Button |

规则：

- Material 3 倾向圆润，但不能圆到失去结构感。
- 数据密集型 PC 页面优先 `8-16px`，移动端交互容器可用 `16-20px`。

## 9. 阴影与层级系统

| Token | Value | 用途 |
| --- | --- | --- |
| `level_0` | `none` | 扁平表面 |
| `level_1` | `0 1px 2px rgba(0,0,0,0.12), 0 1px 3px rgba(0,0,0,0.08)` | 默认卡片 |
| `level_2` | `0 2px 6px rgba(0,0,0,0.14), 0 1px 2px rgba(0,0,0,0.08)` | 菜单、悬浮卡 |
| `level_3` | `0 4px 12px rgba(0,0,0,0.16), 0 2px 4px rgba(0,0,0,0.10)` | 对话框、抽屉 |
| `level_4` | `0 8px 24px rgba(0,0,0,0.18), 0 4px 8px rgba(0,0,0,0.10)` | 全局高层浮层 |

规则：

- Elevation 不用于所有卡片。
- 先选 surface 层，再少量加阴影。
- Focus ring 独立于阴影，不能省略。

## 10. 图标系统

- 风格参考 Material Symbols Rounded
- 默认尺寸：`20px`、`24px`
- 复杂工具条允许 `18px`
- Icon Button 热区：
  - Mobile / H5: `44x44px`
  - Desktop: `40x40px`

规则：

- 图标颜色默认继承文字色或语义色。
- 重要操作图标需配合文本，避免纯图标表达高风险动作。

## 11. 组件系统

### 11.1 Button

Variants:

- Filled Button
- Tonal Button
- Outlined Button
- Text Button
- Elevated Button

尺寸：

- `sm = 32px`
- `md = 40px`
- `lg = 48px`

状态：

- default
- hover
- focus
- pressed
- disabled
- loading

### 11.2 Text Field

- 高度 `56px`
- 支持 outlined / filled 两种模式
- 有 label、assistive text、error text、prefix/suffix slot
- 错误态边框与文案同步变化

### 11.3 Select / Dropdown / Autocomplete

- 继承 text field 容器逻辑
- 菜单 elevation `level_2`
- 选中项同时用背景与 leading icon / check 标识

### 11.4 Cards

- Filled Card: `surface.container`
- Elevated Card: `surface.container.low + level_1`
- Outlined Card: `surface + outline.variant`

### 11.5 Navigation

- Mobile: Top App Bar + Bottom Navigation
- Tablet: Top App Bar + Navigation Rail
- Desktop: Top App Bar + Sidebar / Navigation Drawer

### 11.6 Data Components

- Table: 仅在 Desktop / Tablet 主用
- List: 全端通用
- Filter Bar: Tablet / Desktop 必备
- Chips: 用于筛选、标签、状态切换

### 11.7 Feedback Components

- Snackbar
- Inline Alert
- Dialog
- Bottom Sheet
- Empty State

## 12. 响应式布局规范

### 12.1 Breakpoints

- Phone: `0-767px`
- Tablet: `768-1199px`
- Desktop: `1200-1439px`
- Wide: `1440px+`

### 12.2 Grid

- Phone: `4` 栅格，gutter `12px`
- Tablet: `8` 栅格，gutter `16px`
- Desktop / Wide: `12` 栅格，gutter `24px`

### 12.3 布局规则

- Mobile: 单列，任务优先，主要 CTA 靠近下方区域
- Tablet: 支持双栏、主从视图、Rail、Drawer
- Desktop: 支持 Sidebar、Table、Detail Pane、Breadcrumb
- Wide: 增加多列密度，但阅读列不超过 `760px`

## 13. 平台专项规范

### 13.1 Mobile App

- Top App Bar 高度 `64px`
- Bottom Navigation 适用于 3-5 主目的地
- 长表单页面使用 Sticky Bottom CTA

### 13.2 H5 Web

- 处理安全区
- 浏览器 WebView 内避免过密悬浮层
- 输入聚焦时要处理软键盘遮挡

### 13.3 Tablet

- 优先 Rail / Sidebar / Split View
- Popover 用于轻操作，Dialog 用于强确认
- 横屏支持列表 + 详情并列

### 13.4 PC Web

- Header + Sidebar + Content + Optional Right Panel
- 支持 Hover、Tooltip、Table Toolbar、Bulk Actions、Breadcrumb
- Dashboard 使用 12 栅格和模块化卡片

## 14. 页面模板规范

建议默认模板：

1. 登录 / 注册
2. Dashboard
3. 列表页
4. 详情页
5. 表单页
6. 设置页
7. 搜索结果页
8. 空状态 / 成功 / 错误状态页

## 15. 动效规范

| 场景 | Duration | Easing | 说明 |
| --- | --- | --- | --- |
| Hover | `100ms` | `standard` | 桌面反馈 |
| Press | `100ms` | `accelerate` | 按下反馈 |
| Snackbar enter | `200ms` | `decelerate` | 自下而上出现 |
| Dialog enter | `250ms` | `emphasized` | 淡入 + 轻缩放 |
| Drawer enter | `250ms` | `emphasized` | 侧滑进入 |
| List update | `150ms` | `standard` | 轻量过渡 |

Reduce Motion：

- 时长降为 `0-80ms`
- 去掉缩放和平移，保留 opacity 和 focus

## 16. 可访问性规范

- 正文对比度 >= `4.5:1`
- 大字号 >= `3:1`
- 热区最小 `44x44px`
- Focus ring 必须可见
- Error 必须包含图标、颜色、文案
- 深色模式单独验收，不允许简单取反

## 17. Design Token JSON

同目录 [tokens.json](/Users/wang/Documents/pm-auto-generator/design/material-3-cross-platform/tokens.json) 为结构化 token 输出，字段与 `DESIGN.md` 对齐，适合 AI 和程序直接读取。

## 18. Figma 文件结构与组件命名规范

页面结构：

- `00 Foundations`
- `01 Components`
- `02 Patterns`
- `03 Templates`
- `10 Mobile App`
- `11 H5 Web`
- `12 Tablet`
- `13 PC Web`

命名规则：

- Color Variable: `color/light/primary/base`
- Text Style: `type/body/large`
- Elevation: `shadow/level/2`
- Component: `Button/Filled/Default`
- Template: `Template/List/Desktop`

## 19. 前端 CSS Variables / Tailwind Token 示例

### 19.1 CSS Variables

```css
:root {
  --md-sys-color-primary: #6750a4;
  --md-sys-color-on-primary: #ffffff;
  --md-sys-color-primary-container: #eaddff;
  --md-sys-color-on-primary-container: #21005d;
  --md-sys-color-surface: #fffbfe;
  --md-sys-color-surface-container: #f3edf7;
  --md-sys-color-on-surface: #1c1b1f;
  --md-sys-color-outline: #79747e;
  --md-sys-radius-md: 12px;
  --md-sys-radius-lg: 16px;
  --md-sys-space-4: 16px;
  --md-sys-shadow-2: 0 2px 6px rgba(0,0,0,0.14), 0 1px 2px rgba(0,0,0,0.08);
}

[data-theme="dark"] {
  --md-sys-color-primary: #d0bcff;
  --md-sys-color-on-primary: #381e72;
  --md-sys-color-primary-container: #4f378b;
  --md-sys-color-on-primary-container: #eaddff;
  --md-sys-color-surface: #141218;
  --md-sys-color-surface-container: #211f26;
  --md-sys-color-on-surface: #e6e1e5;
  --md-sys-color-outline: #938f99;
}
```

### 19.2 Tailwind Mapping

```js
export default {
  theme: {
    extend: {
      colors: {
        primary: "var(--md-sys-color-primary)",
        surface: "var(--md-sys-color-surface)",
        surfaceContainer: "var(--md-sys-color-surface-container)",
      },
      borderRadius: {
        md: "12px",
        lg: "16px",
        full: "999px",
      },
      boxShadow: {
        2: "var(--md-sys-shadow-2)",
      },
    },
  },
};
```

## 20. 后续 AI 页面生成规则

1. 必须先读取 `DESIGN.md` 与 `tokens.json`
2. 所有颜色、字号、圆角、阴影、间距必须来自 token
3. 页面先按 Mobile 构建，再按 Tablet / Desktop 重构布局
4. 所有组件必须输出完整状态
5. 优先使用 surface container 系统组织页面
6. 不引入未定义渐变、玻璃、拟物材质
7. 表格只在合适端形态出现，移动端需转列表或摘要卡
8. 深色模式独立生成与验证
9. 验收时检查 color roles 是否被正确使用
10. 验收时检查导航、列表、详情、表单、反馈状态是否符合端特征

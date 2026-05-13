# iOS 26 Liquid Glass 跨端响应式设计规范

## 1. 设计系统总览

本规范面向 iPhone App、H5 Mobile Web、iPad / Tablet、PC Web 四类终端，核心是把 Apple 风格的通透感、浮层感和内容优先原则，转译成可量化、可复用、可交付的设计语言。系统既能落到 Figma 变量与组件，也能直接映射到 React/Vue 与 AI 页面生成。

系统目标：

- 高级但不浮夸
- 玻璃但不牺牲可读性
- Mobile First 但跨端各自有独立布局策略
- 兼容浅色 / 深色 / Reduce Transparency / Reduce Motion

## 2. 设计原则

1. 内容优先：玻璃只是层级容器，不是视觉主角。
2. 浮层自然：导航、筛选、工具栏、面板可悬浮，但不脱离结构。
3. 信息克制：少色、少线、少重阴影，依靠间距和层级组织复杂度。
4. 读写平衡：阅读面以实底和中低对比背景为主，操作面可使用更强玻璃语言。
5. 响应式重构：不同端按使用姿势与任务流重新布局，而不是比例缩放。
6. 无障碍优先：半透明、动效、对比度都必须有明确降级策略。

## 3. 视觉风格规范

- 关键词：高级、简洁、干净、通透、克制、现代、Apple 感
- 风格表达：
  - 背景为低噪雾面，不是纯白纯黑
  - 控件采用悬浮式胶囊或圆角矩形
  - 线框和边界依赖细边框、内高光和柔阴影
  - 动效柔和自然，避免弹性夸张
- 禁止事项：
  - 高饱和品牌色大面积铺底
  - 玻璃层和模糊层覆盖整个正文区域
  - 多重渐变叠加导致视觉噪声

## 4. 色彩系统

### 4.1 Light

| Token | Value | 用途 |
| --- | --- | --- |
| `bg.canvas` | `#F4F5F7` | 页面主背景 |
| `bg.elevated` | `#FFFFFF` | 内容卡片、弹层实底 |
| `bg.sunken` | `#ECEEF2` | 分组背景、禁用态 |
| `text.primary` | `#111318` | 主文案 |
| `text.secondary` | `#4D5562` | 次级文案 |
| `text.tertiary` | `#7E8794` | 辅助说明 |
| `action.primary` | `#0A84FF` | 主要按钮、激活态 |
| `action.secondary` | `#E7F1FF` | 次按钮底色 |
| `state.success` | `#30D158` | 成功 |
| `state.warning` | `#FF9F0A` | 警告 |
| `state.error` | `#FF453A` | 错误 |

### 4.2 Dark

| Token | Value | 用途 |
| --- | --- | --- |
| `bg.canvas` | `#0D1117` | 页面主背景 |
| `bg.elevated` | `#151B24` | 内容卡片、弹层实底 |
| `bg.sunken` | `#0A0E14` | 分组背景、禁用态 |
| `text.primary` | `#F5F7FA` | 主文案 |
| `text.secondary` | `#C4CBD6` | 次级文案 |
| `text.tertiary` | `#8892A0` | 辅助说明 |
| `action.primary` | `#4DA3FF` | 主要按钮、激活态 |
| `action.secondary` | `rgba(77,163,255,0.18)` | 次按钮底色 |
| `state.success` | `#32D74B` | 成功 |
| `state.warning` | `#FFD60A` | 警告 |
| `state.error` | `#FF6961` | 错误 |

### 4.3 对比规则

- 正文与背景对比度 >= 4.5:1
- 小字号辅助文案优先使用 `text.secondary`，避免 `text.tertiary` 过度泛用
- 玻璃层内部若使用背景图或彩色渐变，文字区域必须增加纯色底片或提高玻璃不透明度至 `0.72+`

## 5. Liquid Glass 材质系统

### 5.1 材质等级

| 等级 | Token | 填充 | Blur | 描边 | 用途 |
| --- | --- | --- | --- | --- | --- |
| Glass 1 | `glass.fill` | `rgba(255,255,255,0.56)` / `rgba(18,24,33,0.58)` | `20px` | `1px` 内高光 | Nav、Toolbar |
| Glass 2 | `glass.fillStrong` | `rgba(255,255,255,0.72)` / `rgba(18,24,33,0.74)` | `24px` | `1px` + 外边界 | 浮层卡片、筛选器 |
| Glass 3 | `modal.glass` | 同 Glass 2 | `30px` | `1px` + 强阴影 | Modal、Popover、Command |

### 5.2 材质规则

- 玻璃只用于浮层与上层结构，不用于正文列表大底板。
- 玻璃层内部文本尽量使用纯文字色，不叠加渐变文字。
- 同一屏不建议出现超过 3 种玻璃强度。
- 玻璃与图片叠放时，优先在文字后加 `linear-gradient` 内遮罩。

### 5.3 降级策略

- `Reduce Transparency`: 所有 `glass.*` 替换为 `bg.elevated`，blur 为 `0`，保留圆角与阴影。
- 低性能 WebView: 导航和弹层取消 `backdrop-filter`，改为半透明纯色 + 边框高光。

## 6. 字体系统

### 6.1 字体族

- Sans: `SF Pro Display`, `SF Pro Text`, `PingFang SC`, `Inter`, `Helvetica Neue`, `system-ui`, `sans-serif`
- Mono: `SF Mono`, `JetBrains Mono`, `Menlo`, `monospace`

### 6.2 字阶

| Role | Size | Line Height | Weight | 用途 |
| --- | --- | --- | --- | --- |
| Hero | `40px` | `1.15` | `700` | PC 首屏标题 |
| H1 | `32px` | `1.2` | `700` | 页面主标题 |
| H2 | `24px` | `1.25` | `600` | 区块标题 |
| H3 | `20px` | `1.3` | `600` | 子模块标题 |
| Title | `18px` | `1.35` | `600` | 卡片标题 |
| Body | `16px` | `1.5` | `400` | 默认正文 |
| Body S | `14px` | `1.45` | `400` | 辅助正文 |
| Caption | `12px` | `1.4` | `500` | 标签、说明 |

### 6.3 平台细则

- iPhone / H5: 主阅读区最小 `16px`
- iPad: 左侧导航与辅助信息可使用 `14px`
- PC Web: 表格正文可为 `14px`，但表头和操作列建议 `14px/500`

## 7. 间距系统

基础单位：`4px`

| Token | Value | 建议用途 |
| --- | --- | --- |
| `space.1` | `4px` | 微间距、图标与文字 |
| `space.2` | `8px` | 按钮内部、状态标签 |
| `space.3` | `12px` | 表单组小间距 |
| `space.4` | `16px` | 默认组件内边距 |
| `space.5` | `20px` | iPad / 卡片中密度 |
| `space.6` | `24px` | 区块分隔 |
| `space.8` | `32px` | 页面主区块 |
| `space.10` | `40px` | PC 模块分隔 |
| `space.12` | `48px` | Dashboard 区块 |

规则：

- 移动端卡片内边距 `16px`
- 平板卡片内边距 `20px`
- 桌面主卡片内边距 `24px`
- 表单垂直字段间距 `16px`
- 列表分组与筛选器之间至少 `24px`

## 8. 圆角系统

| Token | Value | 用途 |
| --- | --- | --- |
| `radius.xs` | `8px` | 输入框、小标签 |
| `radius.sm` | `12px` | 小卡片、菜单项 |
| `radius.md` | `16px` | 默认卡片、面板 |
| `radius.lg` | `20px` | 浮层卡片 |
| `radius.xl` | `24px` | Modal、Hero 卡 |
| `radius.xxl` | `28px` | 大容器、首屏展示 |
| `radius.pill` | `999px` | 按钮、Segment、Tab、筛选器 |

规则：

- 操作类控件优先 `pill`
- 信息类容器优先 `16-24px`
- 大桌面布局不要过度增大圆角，避免玩具化

## 9. 阴影与层级系统

| Token | Value | 用途 |
| --- | --- | --- |
| `shadow.softCard` | `0 6px 20px rgba(15,23,42,0.08)` | 实底卡片 |
| `shadow.glassFloat` | `0 8px 24px rgba(15,23,42,0.12), inset 0 1px 0 rgba(255,255,255,0.4)` | 浮层导航、浮卡 |
| `shadow.glassModal` | `0 24px 60px rgba(15,23,42,0.18), inset 0 1px 0 rgba(255,255,255,0.28)` | Modal / Popover |
| `shadow.focusRing` | `0 0 0 4px rgba(10,132,255,0.18)` | Focus |

层级定义：

- L0: 背景画布
- L1: 内容卡片 / 列表容器
- L2: 浮层导航 / 悬浮工具栏 / Sticky CTA
- L3: Popover / Action Sheet / Split Inspector
- L4: Modal / Command / 全屏遮罩

## 10. 图标系统

- 风格：线性图标优先，几何圆角端点，2px 视觉笔画
- 尺寸：`18px`, `20px`, `24px`
- 色彩：默认继承文字色，状态 icon 可用 `state.*`
- 容器：图标按钮热区至少 `44x44px`
- 桌面 Hover：hover 时允许弱底色或玻璃底凸显

## 11. 组件系统

### 11.1 Button

| Variant | 背景 | 前景 | 边界 | 状态 |
| --- | --- | --- | --- | --- |
| Primary | `action.primary` | `text.inverse` | 无 | hover / pressed / disabled / focus |
| Secondary | `action.secondary` 或轻玻璃底 | `action.primary` | 可选细边框 | hover / pressed / disabled / focus |
| Ghost | 透明 | `text.primary` | 无 | hover 时加轻玻璃底 |
| Destructive | `state.error` | `#FFF` | 无 | 仅危险操作 |

按钮尺寸：

- Compact: `36px`
- Default: `44px`
- Large: `52px`

### 11.2 Input / Select / Search

- 高度 `48px`
- 内边距 `0 16px`
- 默认底：`bg.elevated`
- Focus: 描边 `focus` + `focusRing`
- Error: 描边 `state.error`，辅助文案可见

### 11.3 Navigation

- Mobile: 顶部浮层 + 底部 Tab / Sticky CTA 共存
- H5: 允许标题栏 + 底部悬浮主操作
- iPad: Sidebar + Top Bar + Popover
- PC: Header + Sidebar + Breadcrumb + Filter Bar

### 11.4 Card / Panel

- 普通信息卡: `radius.md`, `softCard`
- 浮层操作卡: `radius.lg`, `glassFloat`
- Hero / Summary 卡: `radius.xl`, 允许轻微渐变底与玻璃罩层

### 11.5 Tabs / Segment / Chips

- 胶囊圆角
- Active 使用 `action.primary` 或 `glass.fillStrong`
- Inactive 使用透明或低对比底

### 11.6 Table / List

- PC 表格表头使用实底或弱玻璃，不使用重 blur
- Mobile 列表单元最小高度 `56px`
- Mobile 表格转摘要卡片或横向滚动，不能强制压缩至不可读

### 11.7 Modal / Drawer / Popover

- Modal: `radius.xl`, `blur 30px`, max width `560-720px`
- Drawer: Mobile 自底部滑出；Tablet / Desktop 可右侧 Inspector
- Popover: iPad / PC 推荐使用，避免在手机上使用过窄 hover 弹层

## 12. 响应式布局规范

### 12.1 Breakpoints

- Phone: `0-767px`
- Tablet: `768-1199px`
- Desktop: `1200-1439px`
- Wide Desktop: `1440px+`

### 12.2 Container

- Phone: 左右边距 `16px`
- Tablet: 左右边距 `24px`
- Desktop: 最大宽度 `1200px`，左右边距 `32px`
- Wide: 最大宽度 `1440px`，左右边距 `40px`

### 12.3 Grid

- Phone: `4` 栅格，gutter `12px`
- Tablet: `8` 栅格，gutter `16px`
- Desktop / Wide: `12` 栅格，gutter `20px`

### 12.4 适配规则

- 导航从底部 / 顶部复合，扩展到平板 Sidebar，再到桌面 Sidebar + Breadcrumb
- 复杂数据模块在 Tablet 以上允许双栏和主从结构
- 桌面首屏内容宽度不超过 `720px` 的最佳阅读列，Dashboard 除外

## 13. iPhone / H5 / iPad / PC Web 平台专项规范

### 13.1 iPhone App

- 采用浮层顶部导航或大标题 + 浮层操作区
- 底部 Tab 高度建议 `56-64px`
- 列表与详情页优先单列，操作 CTA 靠底部

### 13.2 H5 Mobile Web

- 顶部与底部必须处理安全区：`padding-top: env(safe-area-inset-top)`，`padding-bottom: env(safe-area-inset-bottom)`
- Sticky CTA 需始终高于软键盘遮挡区；输入聚焦时允许 CTA 缩为顶栏次级按钮
- WebView 内尽量减少高频 blur 大面板，避免性能波动

### 13.3 iPad / Tablet

- 默认优先使用 Sidebar + Content 或 Split View
- Popover 作为筛选、更多操作、快捷预览首选
- 横屏时允许双栏、三栏；竖屏时保留 Sidebar 可折叠

### 13.4 PC Web

- 基础结构：Header + Sidebar + Content + Optional Inspector
- 必须支持 Hover、Tooltip、Breadcrumb、表格工具栏、批量操作条
- Dashboard 中指标卡可 3-4 列布局，列表页以筛选器 + 表格 + 详情抽屉为主

## 14. 页面模板规范

推荐模板：

1. 登录 / 启动页：品牌标题 + 主卡片 + 简化背景光感
2. Dashboard：顶部摘要 + 指标卡 + 趋势图 + 任务列表
3. 列表页：标题区 + 筛选器 + 表格/列表 + 分页/加载更多
4. 详情页：主信息卡 + 属性分组 + 时间线 / 侧边操作
5. 表单页：步骤导航 + 表单区 + Sticky CTA
6. 设置页：Sidebar / Tabs + 分组设置 + 右侧说明
7. 空状态 / 结果页：低噪图标 + 标题 + 说明 + 主要操作

## 15. 动效规范

| 场景 | 时长 | 曲线 | 说明 |
| --- | --- | --- | --- |
| Button hover | `120ms` | `standard` | 桌面可用 |
| Button press | `120ms` | `exit` | 微缩放 `0.98` 可选 |
| Nav blur reveal | `180ms` | `standard` | 透明度 + blur 同步 |
| Modal enter | `240ms` | `entrance` | 上浮 `8px` + 淡入 |
| Drawer enter | `240ms` | `entrance` | 侧向位移 |
| List insert | `180ms` | `standard` | 淡入，不跳动 |

Reduce Motion：

- 位移幅度降为 `0`
- 时长降为 `0-80ms`
- 保留必要显隐与 focus 反馈

## 16. 可访问性规范

- 正文 contrast >= `4.5:1`
- 大标题 contrast >= `3:1`
- 触控最小热区 `44x44px`
- 焦点态必须可见，不能仅依赖 hover
- 错误态需要图标 + 文案 + 结构标记
- 玻璃层文字区遇到底图复杂时自动切 `fillStrong`

## 17. Design Token JSON

JSON 规范见同目录 `tokens.json`，作为程序与 AI 的结构化输入。字段必须与 `DESIGN.md` 一致，不能出现同义改名。

## 18. Figma 文件结构与组件命名规范

### 18.1 页面结构

- `00 Foundations`
- `01 Components`
- `02 Patterns`
- `03 Templates`
- `10 iPhone`
- `11 H5 Web`
- `12 iPad Tablet`
- `13 PC Web`

### 18.2 命名规范

- 颜色变量：`color/light/text/primary`
- 字体样式：`type/body/regular`
- 阴影样式：`shadow/glass/float`
- 组件：`Button/Primary/Default`
- 组件状态：`Input/Text/Focused`
- 模板：`Template/List/Desktop`

### 18.3 Auto Layout 规则

- 所有核心组件使用 Auto Layout
- 所有响应式模板至少建 4 套 frame：iPhone / H5 / iPad / PC
- 组件中避免写死绝对尺寸，优先 Hug / Fill

## 19. 前端 CSS Variables / Tailwind Token / Glass Effect 示例

### 19.1 CSS Variables

```css
:root {
  --bg-canvas: #f4f5f7;
  --bg-elevated: #ffffff;
  --text-primary: #111318;
  --text-secondary: #4d5562;
  --action-primary: #0a84ff;
  --glass-fill: rgba(255, 255, 255, 0.56);
  --glass-fill-strong: rgba(255, 255, 255, 0.72);
  --glass-stroke: rgba(255, 255, 255, 0.44);
  --shadow-glass-float: 0 8px 24px rgba(15, 23, 42, 0.12), inset 0 1px 0 rgba(255, 255, 255, 0.4);
  --radius-pill: 999px;
  --blur-card: 24px;
}

[data-theme="dark"] {
  --bg-canvas: #0d1117;
  --bg-elevated: #151b24;
  --text-primary: #f5f7fa;
  --text-secondary: #c4cbd6;
  --action-primary: #4da3ff;
  --glass-fill: rgba(18, 24, 33, 0.58);
  --glass-fill-strong: rgba(18, 24, 33, 0.74);
  --glass-stroke: rgba(255, 255, 255, 0.14);
}
```

### 19.2 Tailwind Token Mapping

```js
export default {
  theme: {
    extend: {
      colors: {
        canvas: "var(--bg-canvas)",
        elevated: "var(--bg-elevated)",
        primary: "var(--action-primary)",
      },
      borderRadius: {
        pill: "999px",
        glass: "20px",
      },
      boxShadow: {
        glass: "var(--shadow-glass-float)",
      },
      backdropBlur: {
        glass: "24px",
      },
    },
  },
};
```

### 19.3 Glass Effect

```css
.glass-panel {
  background: var(--glass-fill);
  border: 1px solid var(--glass-stroke);
  box-shadow: var(--shadow-glass-float);
  backdrop-filter: blur(var(--blur-card)) saturate(1.2);
  -webkit-backdrop-filter: blur(var(--blur-card)) saturate(1.2);
}

@media (prefers-reduced-transparency: reduce) {
  .glass-panel {
    background: var(--bg-elevated);
    backdrop-filter: none;
    -webkit-backdrop-filter: none;
  }
}

@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0ms !important;
    transition-duration: 80ms !important;
    scroll-behavior: auto !important;
  }
}
```

## 20. 后续 AI 页面生成规则

1. 先读取 `DESIGN.md` 与 `tokens.json`，不得绕开语义 token 直接自造值。
2. 默认 Mobile First 生成，再根据平台专项规范重构 Tablet / PC，不做比例放大。
3. 页面生成时优先选择模板：Dashboard、列表、详情、表单、设置、空状态。
4. 玻璃仅能用于 Nav、Toolbar、浮卡、Modal、Popover、Sticky CTA，不得铺满正文。
5. 若内容密度高，优先降低透明度或切换实底，而不是降低文字对比度。
6. 所有组件都要输出状态：default / hover / active / disabled / focus / error。
7. H5 必须处理安全区、软键盘、Sticky CTA；PC 必须处理 hover、breadcrumb、表格。
8. 深色模式不能只是反相；需使用 dark token 与独立玻璃参数。
9. 若用户启用 Reduce Transparency / Reduce Motion，自动应用降级规则。
10. 页面完成后按以下清单验收：
   - token 是否一致
   - 文字是否可读
   - CTA 是否突出
   - 各端布局是否重构到位
   - hover/focus/error 是否完整

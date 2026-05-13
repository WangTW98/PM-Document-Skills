# 企业级极简风 + Ant Design 企业风 + 卡片化轻量风 后台管理系统设计规范

## 1. 设计系统总览

本规范面向企业级后台管理系统，融合三类视觉特征：

- 企业级极简风：强调专业、理性、清晰、低噪声
- Ant Design 企业风：强调成熟的数据管理体验、复杂业务交互和组件体系完整性
- 卡片化轻量风：强调模块化承载、信息分区、页面可扫描性和弱压迫感

规范目标是让后台系统在保持高信息密度的同时，依然具备良好的阅读效率、操作效率、组件复用能力和跨端扩展能力。

## 2. 设计原则

1. 信息先于装饰：后台首先服务于管理效率，不服务于视觉炫技。
2. 结构先于颜色：优先使用布局、卡片、表格、抽屉和分区建立页面秩序。
3. 密度可控：支持紧凑信息展示，但不能牺牲可读性和点击准确性。
4. 组件完备：Button、Input、Table、Form、Drawer、Modal、Tabs、Filters 是一级资产。
5. 操作明确：所有关键操作都必须被快速发现、快速判断、快速执行。
6. 响应式重构：PC 是主战场，Tablet 是协作场，Mobile/H5 是轻管理或任务处理场。

## 3. 视觉风格规范

- 关键词：专业、清楚、轻量、克制、秩序、可信、理性
- 风格表达：
  - 主体背景使用浅灰布局底 + 白色卡片
  - 内容以卡片、表格、分组面板承载
  - 主色用于关键操作和导航激活，不大面积使用
  - 阴影柔和、边框细、圆角适中
- 禁止事项：
  - 高饱和品牌色大面积铺底
  - 过度留白导致后台效率下降
  - 为追求“高级感”而降低对比度

## 4. 色彩系统

### 4.1 Light

| Token | Value | 用途 |
| --- | --- | --- |
| `brand.primary` | `#1677FF` | 主按钮、链接、激活态 |
| `brand.primary.bg` | `#E6F4FF` | 主色浅底 |
| `bg.canvas` | `#F5F7FA` | 页面总背景 |
| `bg.layout` | `#F0F2F5` | 布局底、侧栏外围 |
| `bg.container` | `#FFFFFF` | 卡片、表格、抽屉内容 |
| `text.primary` | `#1F2329` | 主文字 |
| `text.secondary` | `#4E5969` | 次级文字 |
| `text.tertiary` | `#86909C` | 辅助说明 |
| `border.subtle` | `#E5E6EB` | 分割线 |
| `border.default` | `#D9DDE3` | 输入框、卡片边框 |
| `success` | `#00B42A` | 成功 |
| `warning` | `#FF7D00` | 警告 |
| `error` | `#F53F3F` | 错误 |

### 4.2 Dark

| Token | Value | 用途 |
| --- | --- | --- |
| `brand.primary` | `#3C89FF` | 主按钮、链接、激活态 |
| `brand.primary.bg` | `rgba(22,119,255,0.20)` | 主色浅底 |
| `bg.canvas` | `#0F1115` | 页面总背景 |
| `bg.layout` | `#14171A` | 布局底 |
| `bg.container` | `#1D2129` | 卡片、表格、抽屉内容 |
| `bg.elevated` | `#232830` | 高层浮层 |
| `text.primary` | `#F2F3F5` | 主文字 |
| `text.secondary` | `#C9CDD4` | 次级文字 |
| `text.tertiary` | `#86909C` | 辅助说明 |
| `border.default` | `#3A3F47` | 输入框、卡片边框 |
| `success` | `#23C343` | 成功 |
| `warning` | `#FF9A2E` | 警告 |
| `error` | `#FF6B6B` | 错误 |

### 4.3 使用规则

- 后台大部分区域由中性色构成。
- 主色只在主按钮、导航激活、关键指标强调和链接中使用。
- 状态色需配合图标、Tag、文案一起使用，不能只靠背景块。

## 5. 卡片化轻量系统

后台页面采用卡片化布局，但不是“移动端商城卡片风”，而是“企业信息模块化卡片风”。

### 5.1 卡片类型

| 类型 | 用途 | 背景 | 边界 | 阴影 |
| --- | --- | --- | --- | --- |
| Metric Card | Dashboard 指标卡 | `bg.container` | `border.subtle` | `shadow.card` |
| Module Card | 普通模块容器 | `bg.container` | `border.subtle` | 弱阴影或无阴影 |
| Filter Card | 筛选容器 | `bg.container` | `border.default` | `shadow.card` |
| Detail Card | 详情分组 | `bg.container` | `border.subtle` | 无或弱阴影 |

### 5.2 卡片规则

- 默认圆角 `8px`
- 默认内边距 `16px / 20px / 24px`
- 标题区与操作区必须有明确结构
- 卡片之间通过间距组织，不通过复杂背景拼接

## 6. 字体系统

### 6.1 字体族

- Sans: `Inter`, `PingFang SC`, `Helvetica Neue`, `Arial`, `system-ui`
- Mono: `JetBrains Mono`, `SF Mono`, `Menlo`

### 6.2 字阶

| Role | Size | Line Height | Weight | 用途 |
| --- | --- | --- | --- | --- |
| Hero | `36px` | `1.22` | `600` | 大盘总览标题 |
| H1 | `28px` | `1.3` | `600` | 页面标题 |
| H2 | `24px` | `1.34` | `600` | 模块标题 |
| H3 | `20px` | `1.4` | `600` | 卡片组标题 |
| Title | `16px` | `1.5` | `500` | 表单 / 卡片标题 |
| Body | `14px` | `1.57` | `400` | 默认正文 |
| Body Small | `13px` | `1.54` | `400` | 表格正文 / 辅助 |
| Caption | `12px` | `1.5` | `400` | 元数据 |

规则：

- 后台正文默认 `14px`
- 表格、筛选项、辅助说明可使用 `13px`
- 不建议大面积使用 `12px`

## 7. 间距系统

基础单位：`4px`

| Token | Value | 用途 |
| --- | --- | --- |
| `space.1` | `4px` | 微调 |
| `space.2` | `8px` | 控件内部、图标间距 |
| `space.3` | `12px` | 小型模块 |
| `space.4` | `16px` | 默认内边距 |
| `space.5` | `20px` | 卡片中密度 |
| `space.6` | `24px` | 模块分隔 |
| `space.8` | `32px` | 页面区块 |
| `space.10` | `40px` | 大页分区 |

规则：

- Dashboard 卡片间距 `16-24px`
- 筛选器项间距 `12-16px`
- 表单字段垂直间距 `16px`
- 表格上方工具栏与表格间距 `16px`

## 8. 圆角系统

| Token | Value | 用途 |
| --- | --- | --- |
| `xs` | `4px` | 小标签、输入内元素 |
| `sm` | `6px` | 小按钮、轻量控件 |
| `md` | `8px` | 默认按钮、输入、卡片 |
| `lg` | `12px` | 大卡片、抽屉局部区块 |
| `xl` | `16px` | Modal、重要卡片 |
| `xxl` | `20px` | 大展示模块 |

规则：

- 企业后台以 `6-12px` 为主，不走过圆路线
- 弹窗和抽屉可以稍大，但仍需专业感

## 9. 阴影与层级系统

| Token | Value | 用途 |
| --- | --- | --- |
| `shadow.card` | `0 2px 8px rgba(31,35,41,0.06)` | 默认卡片 |
| `shadow.float` | `0 6px 20px rgba(31,35,41,0.10)` | Popover / Dropdown |
| `shadow.drawer` | `0 10px 30px rgba(31,35,41,0.16)` | Drawer / Modal |
| `shadow.focus` | `0 0 0 3px rgba(22,119,255,0.20)` | Focus |

层级建议：

- L0: 页面布局背景
- L1: 卡片 / 表格 / 过滤器
- L2: 下拉 / Popover / Tooltip
- L3: Drawer / Modal / 批量操作浮条

## 10. 图标系统

- 风格：线性企业图标，视觉干净
- 尺寸：`14px`, `16px`, `18px`
- 用途：
  - `14px`: 表格操作、状态辅助
  - `16px`: 按钮、菜单、工具栏
  - `18px`: 页面级功能入口

规则：

- 图标不单独承担危险操作含义
- 高风险动作必须配合文案

## 11. 组件系统

### 11.1 Navigation

- Desktop:
  - Header `56px`
  - Sidebar `240px`
  - Collapsed Sidebar `64px`
  - Breadcrumb + Page Header
- Tablet:
  - 可折叠 Sidebar
  - Drawer 弹出导航
- Mobile / H5:
  - 简化 Header
  - 列表与详情以分步任务流呈现

### 11.2 Buttons

尺寸：

- Small `28px`
- Medium `36px`
- Large `40px`

Variants:

- Primary
- Default
- Dashed / Secondary
- Text
- Link
- Danger

状态：

- default
- hover
- active
- focus
- disabled
- loading

### 11.3 Form Controls

- Input / Select / DatePicker 默认 `36px`
- 搜索框支持 `40px`
- 紧凑模式可降为 `32px`
- 必须包含 label、placeholder、helper、error

### 11.4 Table

- 默认行高 `48px`
- 紧凑模式 `40px`
- 支持：
  - fixed columns
  - row hover
  - empty state
  - bulk select
  - column actions
  - sticky header

### 11.5 Cards & Panels

- Dashboard Metric Card
- Filter Card
- Detail Panel
- Side Summary Card

### 11.6 Overlay Components

- Drawer：优先用于详情/编辑
- Modal：优先用于确认/关键流程
- Dropdown / Popover：优先用于轻操作
- Tooltip：解释性，不用于承载主要信息

### 11.7 Feedback Components

- Tag / Badge / Alert / Message / Notification / Result
- Empty State / Error State / Permission State

## 12. 响应式布局规范

### 12.1 Breakpoints

- Phone: `0-767px`
- Tablet: `768-1199px`
- Desktop: `1200-1599px`
- Wide: `1600px+`

### 12.2 Grid

- Phone: `4` 栅格，gutter `12px`
- Tablet: `8` 栅格，gutter `16px`
- Desktop / Wide: `12` 栅格，gutter `24px`

### 12.3 适配规则

- Mobile: 大表格转卡片列表或摘要列表
- Tablet: 允许保留简化表格与抽屉详情
- Desktop: 保留完整筛选、表格、操作列、右侧详情
- Wide: 支持多卡 Dashboard 与更宽表格，但控制阅读列

## 13. 平台专项规范

### 13.1 PC Web

- 主战场
- 必须支持:
  - Header
  - Sidebar
  - Breadcrumb
  - Filter Bar
  - Table Toolbar
  - Drawer
  - Batch Actions

### 13.2 Tablet

- 适合巡检、审批、轻编辑
- Sidebar 可折叠
- 抽屉和 Popover 使用频率高于 Modal

### 13.3 H5 Web

- 适合查询、审批、轻录入
- 长表格需转换
- 软键盘遮挡和 sticky submit 需处理

### 13.4 Mobile App

- 适合待办处理、审批、消息、概览
- 页面结构更任务化，减少复杂筛选堆叠

## 14. 页面模板规范

推荐模板：

1. 登录页
2. Dashboard 总览页
3. 列表页
4. 详情页
5. 表单新建 / 编辑页
6. 审批页
7. 设置页
8. 权限配置页
9. 消息中心页
10. Result / Empty / Error 页

## 15. 动效规范

| 场景 | 时长 | 曲线 | 说明 |
| --- | --- | --- | --- |
| Hover | `120ms` | `standard` | 桌面悬停反馈 |
| Press | `120ms` | `exit` | 按下反馈 |
| Dropdown | `180ms` | `enter` | 下拉展开 |
| Drawer | `240ms` | `enter` | 右侧抽屉 |
| Modal | `240ms` | `enter` | 居中弹窗 |
| Table state update | `180ms` | `standard` | 轻量更新 |

Reduce Motion：

- 所有位移动画降为 `0`
- 只保留 opacity 与 focus
- 时长缩至 `0-80ms`

## 16. 可访问性规范

- 正文 contrast >= `4.5:1`
- 桌面可点击控件 >= `32px`
- 移动端控件 >= `44px`
- Focus 必须可见
- 表格状态不能只靠颜色区分
- 错误态必须包含文案与位置反馈

## 17. Design Token JSON

结构化 token 见 [tokens.json](/Users/wang/Documents/pm-auto-generator/design/enterprise-admin-minimal-ant-card/tokens.json)，与 `DESIGN.md` 对齐，可直接给前端、Figma 或 AI 读取。

## 18. Figma 文件结构与组件命名规范

页面结构：

- `00 Foundations`
- `01 Components`
- `02 Patterns`
- `03 Templates`
- `10 PC Web`
- `11 Tablet`
- `12 H5 Web`
- `13 Mobile App`

命名规范：

- Color Variable: `color/light/brand/primary`
- Text Style: `type/body/default`
- Shadow: `shadow/card`
- Component: `Table/Default/Compact`
- Template: `Template/List/Desktop`

## 19. 前端 CSS Variables / Tailwind Token 示例

### 19.1 CSS Variables

```css
:root {
  --brand-primary: #1677ff;
  --bg-canvas: #f5f7fa;
  --bg-layout: #f0f2f5;
  --bg-container: #ffffff;
  --text-primary: #1f2329;
  --text-secondary: #4e5969;
  --border-default: #d9dde3;
  --state-success: #00b42a;
  --state-warning: #ff7d00;
  --state-error: #f53f3f;
  --radius-md: 8px;
  --shadow-card: 0 2px 8px rgba(31, 35, 41, 0.06);
}

[data-theme="dark"] {
  --brand-primary: #3c89ff;
  --bg-canvas: #0f1115;
  --bg-layout: #14171a;
  --bg-container: #1d2129;
  --text-primary: #f2f3f5;
  --text-secondary: #c9cdd4;
  --border-default: #3a3f47;
}
```

### 19.2 Tailwind Mapping

```js
export default {
  theme: {
    extend: {
      colors: {
        brand: "var(--brand-primary)",
        canvas: "var(--bg-canvas)",
        container: "var(--bg-container)",
        textPrimary: "var(--text-primary)",
      },
      borderRadius: {
        md: "8px",
        lg: "12px",
      },
      boxShadow: {
        card: "var(--shadow-card)",
      },
    },
  },
};
```

## 20. 后续 AI 页面生成规则

1. 必须先读取 `DESIGN.md` 和 `tokens.json`
2. 后台默认从 Desktop 信息架构出发，再向 Tablet / H5 / Mobile 收缩
3. 优先使用 Header、Sidebar、Breadcrumb、Page Header、Card、Table、Drawer
4. 表格、表单、筛选器必须有完整状态
5. 大面积营销式设计语言、玻璃材质和不必要插画禁止引入
6. 卡片是页面组织单元，不是视觉装饰
7. 深色模式必须单独校验对比与边界
8. 大表格在移动端必须转为卡片或摘要列表
9. 所有关键操作必须在首屏或局部上下文中可被发现
10. 验收时重点检查：
   - 信息层级是否清楚
   - 表格与筛选是否高效
   - 抽屉和详情流是否顺畅
   - 各端布局是否真正重构

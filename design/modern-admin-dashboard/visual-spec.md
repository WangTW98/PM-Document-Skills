# Visual Specification: Modern Admin Dashboard

## 1. 设计主旨 (Design Thesis)
“现代化的管理后台”应当是一个高效的数据处理器，而非充满干扰的视觉展厅。设计主旨是：**让数据与工作流成为唯一的焦点**。
我们通过降低环境的视觉噪音（统一使用低饱和度的浅灰背景 `color.background.canvas`），利用留白（Negative Space）而非边框来分割内容区块，以此营造开阔和纯净的使用体验。

## 2. 品牌个性与目标受众
- **品牌个性**: 专业（Professional）、可信赖（Reliable）、高效（Efficient）、现代（Modern）。
- **目标受众**: B端业务人员、数据分析师、系统管理员等需要长时间面对屏幕并处理高密度信息的用户。

## 3. 色彩系统 (Color System)
色彩不再仅仅是装饰，而是状态与意图的传达者。

- **品牌主色 (Brand Primary)**: `#2563eb` (蓝)。这是一种被证明能引起最高信任感的“科技蓝”。仅在 Primary Button、活跃导航项或关键文字链接中使用。
- **中性色 (Neutral)**: `#0f172a` 至 `#ffffff`。
  - `#0f172a (text.primary)` 用于主标题。
  - `#475569 (text.secondary)` 用于绝大部分正文，减轻视觉疲劳。
  - `#f8fafc (canvas)` 页面大背景色，与卡片背景色 `#ffffff (surface)` 形成极轻微的层级区分。
  - `#e2e8f0 (border.default)` 是界面中所有边框和分隔线的标准色。
- **反馈色 (Feedback)**: 必须严格遵循语义。成功（绿）、警告（橙黄）、错误（红）。

## 4. 排版层次 (Typography Scale)
采用无衬线系统字体（San-serif System Fonts），摒弃花哨的自定义字体加载，追求极致的首屏渲染性能。

- **H1 (页面标题)**: `text-2xl (24px)`, `semibold (600)`, `color.text.primary`.
- **H2 (模块标题)**: `text-lg (18px)`, `semibold (600)`, `color.text.primary`.
- **H3 (卡片标题/表单标签)**: `text-base (16px)`, `medium (500)`, `color.text.primary`.
- **Body (正文与表格数据)**: `text-sm (14px)`, `regular (400)`, `color.text.secondary`. 这是B端系统最舒适的数据阅读字号。
- **Small (元数据/辅助说明)**: `text-xs (12px)`, `regular (400)`, `color.text.tertiary`.

## 5. 布局、间距与密度 (Layout & Spacing)
- 遵循 4px 基础网格体系。
- **高密度场景（如复杂数据表格）**: 行高（Line Height）可收紧至 `1.25`，表格单元格 padding 采用 `space.2 (8px)` 上下与 `space.3 (12px)` 左右。
- **正常密度场景（如表单、仪表盘）**: 表单项之间的垂直间距采用 `space.4 (16px)` 或 `space.6 (24px)` 以确保视觉上的独立性。

## 6. 组件规范 (Component Specs)
- **Buttons (按钮)**: 
  - 基础高度为 `40px`。不要为了追求“紧凑”而缩小按钮高度至无法轻易点击的尺寸。
  - 圆角 `6px (radius.md)`。
- **Forms (表单输入)**:
  - 统一高度 `40px`。使用 `#ffffff` 底色和 `#e2e8f0` 边框。
  - `Focus` 状态下，必须显示蓝色的光晕阴影（`shadow.focus`），提供明确的无障碍反馈。
- **Cards (卡片)**:
  - 承载独立的数据或功能模块。必须带有白色的底层 (`color.background.surface`)、细边框 (`border.default`) 和 `8px (radius.lg)` 圆角。
  - **默认不要加阴影**，保持界面清爽。只有在卡片内包含可悬停的交互（如变为可点击态）时，才增加 `shadow.md` 提示层级提升。

## 7. 响应式适应规则 (Responsive Adaptation Rules)
系统必须能够适应从小屏移动设备到超宽屏显示器。

- **Mobile (sm: <640px)**: 导航从侧边栏收起到底部 Tab 或汉堡菜单。多列布局折叠为单列堆叠。表格通过隐藏次要列或转换为卡片流（Card List）显示。
- **Tablet (md: 768px - lg: 1024px)**: 侧边栏可以折叠为仅显示 Icon 的迷你模式（Mini-sidebar）。界面可以采用两列网格布局。
- **Desktop (xl: 1280px)**: 侧边栏完全展开。数据采用宽屏多列呈现。
- **Wide Desktop (2xl: 1536px+)**: 内容区最大宽度限制在 `1440px` (`container.max_width`)，并在超大屏中水平居中，避免用户需要频繁左右转动脖子。

## 8. 无障碍要求 (Accessibility)
- 对比度：所有前景文字与背景必须满足 WCAG AA 级别 (至少 4.5:1)。
- 颜色盲友好：状态（成功/错误/警告）绝不只靠颜色表达，必须配合图形 Icon 或明确的文本标签。
- 焦点状态：保留键盘操作的 `:focus-visible` 态，并使用高对比度光圈 `shadow.focus`。

## 9. Do's and Don'ts

- **DO**：把页面上的唯一主要行动（如“提交”、“创建”）设为 Primary Button。
- **DO**：善用间距 `space` 来组织信息，这是比添加各种分隔线和边框更高级的排版技巧。
- **DON'T**：在一个小小的卡片内使用超过 3 种不同的字号。
- **DON'T**：将纯黑 `#000000` 用于文字。使用 `#0f172a` 会让界面显得更有质感且柔和。

## 10. 假设与原由 (Assumptions & Rationale)
- **假设**：系统假设用户的工作环境光线充足，因此默认采用 Light Mode（浅色模式）作为核心规范，深色模式（Dark Mode）需要通过翻转色板另行定义。
- **原由**：采用 4px 的倍数是因为现代屏幕分辨率能最清晰地渲染偶数像素，同时符合大多数开源框架（如 TailwindCSS）的默认规范，极大降低开发成本。

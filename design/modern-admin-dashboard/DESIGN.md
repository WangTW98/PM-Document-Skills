---
design_system:
  name: "Modern Admin Dashboard"
  version: "1.0.0"
  intent: "A clean, data-dense, and highly functional admin dashboard visual language."
  tokens:
    color:
      brand:
        primary: "#2563eb"
        primary_hover: "#1d4ed8"
        primary_active: "#1e40af"
        primary_subtle: "#eff6ff"
      neutral:
        100: "#ffffff"
        200: "#f8fafc"
        300: "#f1f5f9"
        400: "#e2e8f0"
        500: "#cbd5e1"
        600: "#94a3b8"
        700: "#64748b"
        800: "#475569"
        900: "#0f172a"
      text:
        primary: "#0f172a"
        secondary: "#475569"
        tertiary: "#94a3b8"
        inverse: "#ffffff"
      background:
        canvas: "#f8fafc"
        surface: "#ffffff"
        surface_hover: "#f1f5f9"
      border:
        default: "#e2e8f0"
        strong: "#cbd5e1"
        focus: "#3b82f6"
      feedback:
        success: "#10b981"
        success_bg: "#ecfdf5"
        warning: "#f59e0b"
        warning_bg: "#fffbeb"
        error: "#ef4444"
        error_bg: "#fef2f2"
    typography:
      font_family:
        sans: "Inter, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif"
        mono: "ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace"
      font_size:
        xs: "0.75rem"   # 12px
        sm: "0.875rem"  # 14px
        base: "1rem"    # 16px
        lg: "1.125rem"  # 18px
        xl: "1.25rem"   # 20px
        2xl: "1.5rem"   # 24px
      font_weight:
        regular: 400
        medium: 500
        semibold: 600
        bold: 700
      line_height:
        tight: 1.25
        normal: 1.5
        relaxed: 1.75
    space:
      0: "0px"
      1: "0.25rem"   # 4px
      2: "0.5rem"    # 8px
      3: "0.75rem"   # 12px
      4: "1rem"      # 16px
      5: "1.25rem"   # 20px
      6: "1.5rem"    # 24px
      8: "2rem"      # 32px
      10: "2.5rem"   # 40px
      12: "3rem"     # 48px
    radius:
      sm: "0.25rem"  # 4px
      md: "0.375rem" # 6px
      lg: "0.5rem"   # 8px
      full: "9999px"
    shadow:
      sm: "0 1px 2px 0 rgba(0, 0, 0, 0.05)"
      md: "0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06)"
      lg: "0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05)"
      focus: "0 0 0 2px #ffffff, 0 0 0 4px #3b82f6"
    border:
      width:
        default: "1px"
        thick: "2px"
    motion:
      duration:
        fast: "150ms"
        normal: "200ms"
        slow: "300ms"
      easing:
        default: "cubic-bezier(0.4, 0, 0.2, 1)"
        in: "cubic-bezier(0.4, 0, 1, 1)"
        out: "cubic-bezier(0, 0, 0.2, 1)"
    breakpoint:
      sm: "640px"
      md: "768px"
      lg: "1024px"
      xl: "1280px"
      2xl: "1536px"
    container:
      max_width: "1440px"
    component:
      button:
        height:
          sm: "32px"
          base: "40px"
          lg: "48px"
      input:
        height:
          base: "40px"
---

# DESIGN.md

本文件是 **Modern Admin Dashboard** 设计规范的权威（Source of Truth）。AI 与开发人员均应将本文件作为视觉实现的基准。

## 1. Visual Theme & Atmosphere
现代管理后台注重信息的“信噪比”（Signal-to-Noise Ratio）。视觉基调必须保持干净、专业且克制。
- **What to use**: 充足的负空间（留白）以分隔内容区；中性的灰色系作为背景和边框；清晰的无衬线字体。
- **When to avoid**: 避免滥用高饱和色彩，主品牌色应仅用于触发最关键的行为（如“保存”、“提交”按钮），或者当前活跃状态的标识（如选中的导航项）。

## 2. Color Palette & Roles
系统使用严格的语义化颜色角色，而非仅仅硬编码色值。
- **Background**: `color.background.canvas` (#f8fafc) 用作全屏底层背景，`color.background.surface` (#ffffff) 用作卡片、浮层、表单的背景。这创造了层次感。
- **Text**: 正文使用 `color.text.secondary` (#475569) 以减轻阅读疲劳，标题和关键数据使用 `color.text.primary` (#0f172a) 以突出层级。
- **Action**: 所有主要操作均使用 `color.brand.primary` (#2563eb)。Hover 时加深为 `primary_hover`。
- **Feedback**: 状态标签和警告必须使用 `feedback` 体系，并且带有较浅的背景色（如 `success_bg`）来保证内容在列表中的可读性。

## 3. Typography Rules
基于系统字体的现代排版方案，保证在各类系统上无需加载 Web 字体即可拥有原生应用的渲染质量和加载速度。
- **Scale**: `font_size.base` 为 16px，`sm` 为 14px（B端最常用的数据展示字号），`xs` 为 12px（极次要的元数据）。
- **Weight**: 仅使用 `regular(400)` 作为默认阅读体，`medium(500)` 用于按钮、小标题及表格表头，`semibold(600)` 用于页面大标题和模块标题。
- **Line Height**: `normal (1.5)` 适用于段落阅读，`tight (1.25)` 适用于标题、标签及表格内的紧凑数据行。

## 4. Spacing & Layout
使用 4px 为基础模块的网格系统（4pt Grid System）。
- **Application**: `space.1 (4px)` 用于极紧凑的相关元素组合（如 Icon 和文本）；`space.2 (8px)` 用于表单内的间距；`space.4 (16px)` 和 `space.6 (24px)` 用于模块内的正常间距；`space.8 (32px)` 及以上用于区分大的结构化区块。

## 5. Component Styling
- **Buttons**:
  - Primary 按钮：背景 `brand.primary`，文本 `text.inverse`，圆角 `radius.md`。
  - Secondary 按钮（次要操作）：背景 `background.surface`，边框 `border.default`，文本 `text.primary`。
  - **重要原则**：所有可点击控件必须保证至少 `40px` 高度 (`component.button.height.base`) 以匹配触控需求，即使在 PC 端也提供更宽适的点击区域。
- **Forms**: 
  - Input：高度 40px，边框 `border.default`，Focus 态必须有 `shadow.focus` 而非仅仅改变边框颜色。
- **Cards**: 
  - 背景 `surface`，边框 `border.default`，圆角 `radius.lg`。不要在普通卡片上滥用阴影，保留干净的二维界面感。

## 6. Depth, Borders & Elevation
通过边框和极其克制的阴影来表达 Z 轴层级。
- **Borders**: 绝大部分层级隔离通过 `1px solid border.default (#e2e8f0)` 实现。
- **Elevation (Shadow)**: 
  - 默认层（Page、Card）：无阴影。
  - 浮动层（Dropdown、Popover、Sticky Header）：使用 `shadow.md`。
  - 模态层（Modal、Drawer）：使用 `shadow.lg`。
- **Focus**: `shadow.focus` 提供了强制的无障碍双重焦点环（白底+蓝环），适用于所有表单和按钮的 `:focus-visible` 态。

## 7. Icons, Imagery & Illustration
- 推荐使用统一粗细（如 1.5px 线条）的线框图标库（如 Lucide 或 Heroicons）。
- 图标颜色通常跟随文字颜色。

## 8. Motion & Interaction
动画必须是极其简短和辅助性的，不应拖慢操作效率。
- 采用 `motion.duration.fast (150ms)` 配合 `motion.easing.default` 来处理按钮 hover、下拉菜单展开等高频交互。
- 绝不添加无意义的装饰性进场动画。

## 9. Responsive Behavior
默认采用 Mobile First 到 Desktop 扩展的设计理念。
- **sm (<640px)**: 移动端。侧边栏导航收起为汉堡菜单，列表转化为卡片流，表单独占一行（100% 宽度）。
- **md (768px) - lg (1024px)**: 平板。侧边栏可呈现紧凑模式（仅图标）。多列布局可并排（如两列）。
- **xl (1280px+)**: 桌面端。固定侧边栏宽度（如 256px），留有充足外边距。最大内容宽度限制为 `1440px` (`container.max_width`)，并在超大屏幕中居中显示，防止眼球扫视跨度过大。

## 10. Accessibility Rules
- 所有的文字对背景必须满足至少 WCAG AA (4.5:1) 的对比度要求。
- 绝不使用纯色差来表示状态。状态指示必须伴随 Icon 或文字标签（例如：红色背景加“失败”文案及对应的 Alert Icon）。
- 支持 Focus 轮廓线（Focus Ring），不得在 CSS 中设置 `outline: none` 而不提供同等的视觉反馈（请使用 `shadow.focus`）。

## 11. Do's and Don'ts
- **DO**: 使用大量 `space.6 (24px)` 级别的空白，区分不同的信息区块组。
- **DO**: 让次要操作降级显示（如使用浅色背景或轮廓线），突出页面唯一的主 Primary Action。
- **DON'T**: 不要将文本设置在过深或过纯的底色上，影响阅读性。
- **DON'T**: 避免在同一个表格或卡片中使用超过 3 种字号，依靠字体粗细（Weight）和颜色明度（Color）区分信息而非单纯的大小。

## 12. Agent Prompt Guide
后续 Agent 生成页面或 UI 时的关键约束指南：
- **Canonical Source**: 读取本 `DESIGN.md` 中的 YAML 前缀获取 Token 值。
- **Top Constraints**: 
  1. 使用 `#f8fafc` 作为整体背景，`#ffffff` 作为容器背景，通过 `#e2e8f0` (`border.default`) 划界。
  2. 字号以 `14px` (文本) 和 `16px` (标题/输入框) 为主。
  3. 大量应用 `flex` 布局并使用规范规定的 `gap` (`space.4` / `space.6`)。
  4. 使用提供的 `shadow.focus` 应对焦点状态。
- **Component Defaults**: 按钮高度 `40px`，圆角 `6px (md)`。卡片圆角 `8px (lg)`。
- **Instruction**: 代码生成时应直接引用对应 Token 名称作为 CSS 变量（或 SCSS 变量、Tailwind 类名对应值），**务必**保持 Token 语义的名称结构。

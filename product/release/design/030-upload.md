# 简历上传页 Design Release

> 输出路径：`product/release/design/030-upload.md`。本文档描述页面展示内容与样式布局，不描述交互执行、埋点、接口、后端或业务处理逻辑。

# 简历上传页 Design Release

## 0. 文档状态

| 字段 | 内容 |
|---|---|
| 文档版本 | Release |
| 生成日期 | 2026-05-12 |
| 来源 Mock 文件 | `product/release/mock/030-upload.md` |
| 设计约束目录 | `design/ios26-liquid-glass/` |
| 当前输出文件 | `product/release/design/030-upload.md` |
| 页面名称 | 简历上传页 |
| 内容范围 | 页面展示内容 + 视觉样式 + 布局结构 + AI 可读样式结构 |
| 不包含范围 | 交互执行 / 埋点 / 接口 / 后端 / 业务流程 / 实现代码 |

## 1. 页面设计综述

| 项目 | 内容 |
|---|---|
| 页面定位 | 简历优化的第一步，负责接收并解析用户的原始简历文件。 |
| 目标阅读对象 | 设计师 / 产品 / Figma Remote MCP agent |
| 视觉目标 | 极其简练的上传体验，通过精致的解析动画降低用户等待时的焦虑感。 |
| 信息层级 | 1. 简历上传方式选择（文件/拍照）；2. 上传/解析进度反馈；3. 格式与大小说明。 |
| 主要视觉焦点 | 解析状态下的 AI 神经网络扫描动画（Ripple Animation）。 |
| 设计系统应用摘要 | iOS26 Liquid Glass：底层黑背景 + 动态 Aurora (Purple) + 玻璃选择卡片 + 进度环发光效果。 |

## 2. 设计约束提取

| 类型 | Token / 规则 | 取值 | 使用方式 | 来源文件 |
|---|---|---|---|---|
| color | background.canvas | #000000 | 页面底层背景 | DESIGN.md |
| color | accent.purple | #BF5AF2 | 背景流动微光颜色 (解析动画核心色) | DESIGN.md |
| color | action.primary.glow | rgba(10, 132, 255, 0.4) | 进度环发光色 | DESIGN.md |
| typography | size.lg | 20px | 页面标题字号 | DESIGN.md |
| typography | size.sm | 14px | 辅助说明文案 | DESIGN.md |
| space | space.6 | 32px | 卡片间距 | DESIGN.md |
| radius | radius.lg | 24px | 上传卡片圆角 | DESIGN.md |
| component | glassEffect.blur | blur(24px) | 玻璃卡片模糊度 | DESIGN.md |

## 3. 页面结构图

```mermaid
mindmap
  root((简历上传页 Design))
    Frame: Page
      Background: Aurora Layers
        Blob: Purple Glow (Center)
      Frame: Content Layer
        Frame: Navigation (S-001)
          Element: Back Button (E-001)
          Element: Page Title
        Frame: Selection Area (S-002) - Default State
          Element: File Upload Card (E-002)
          Element: Camera Upload Card (E-003)
          Element: Format Hint (E-004)
        Frame: Processing Area (S-003) - Loading State
          Element: AI Scanning Animation (M-001)
          Element: Progress Ring (E-005)
          Element: Status Text (E-006)
        Frame: Error Area (S-004) - Failure State
          Element: Error Icon (E-007)
          Element: Retry Button (E-008)
```

## 4. 自然语言样式描述

### 4.1 整体画面

- **页面整体氛围**：通透、纯净，通过深色背景衬托出玻璃卡片的质感。
- **背景与层级**：画布为纯黑。背景中心有一个微弱的紫色光斑，光斑随着解析进度会产生微弱的呼吸感律动。
- **视觉重心**：默认态下是两个并列的玻璃卡片；解析态下是圆心的扫描进度环。
- **阅读节奏**：顶部返回确认 -> 选择上传方式 -> 关注进度 -> 结果反馈。

### 4.2 关键区块叙述

| 区块ID | 区块名称 | 展示内容摘要 | 样式叙述 | 视觉优先级 | 设计决策 |
|---|---|---|---|---|---|
| S-001 | 导航栏 | 返回 + 标题 | 极简风格。背景透明，不占用视觉空间。标题使用 size.lg 居中。 | 低 | 保持通透感，不使用实色背景。 |
| S-002 | 上传选择区 | 文件/拍照卡片 | 两个垂直排列的大卡片。使用 background.surface 背景，24px 圆角。图标居中，下方带 sm 字号的引导语。 | 高 | 卡片背景需具有 backdrop-filter 以透出背景紫色光斑。 |
| S-003 | 进度展示区 | 进度环 + 动画 | 解析时全屏覆盖。中心是一个发光的进度环，环内是 AI 神经网络扫描的波纹动画。 | 极高 | 进度环边缘使用 action.primary.glow，营造科技感。 |
| S-004 | 失败反馈区 | 错误图标 + 重试 | 位于卡片中心位置。图标使用 status.error 红色，带微弱发光。按钮使用 Ghost 样式。 | 中 | 报错信息使用 text.muted，避免过分刺眼。 |

## 5. 布局与区块样式表

| 区块ID | 来源 Mock 区块 / 元素 | Frame 层级 | 布局方式 | 尺寸 / 约束 | Padding | Gap | 背景 / 边框 / 阴影 | 圆角 | 对齐 | 响应式变化 |
|---|---|---|---|---|---|---|---|---|---|---|
| Page | - | Root | Vertical | Fill Window | 0 | 0 | background.canvas | 0 | Center | - |
| S-001 | S-001 | Header | Horizontal | Width: 100% | 16px 20px | 12px | Transparent | - | Center Left | - |
| S-002 | S-002 | Section | Vertical | Width: 100% | 40px 24px | 24px | Transparent | - | Center | - |
| S-003 | S-003 | Overlay | Vertical | Fill Window | 0 | 24px | surfaceOverlay + blur | - | Center | - |
| Card | E-002 / E-003 | Group | Vertical | Height: 160px | 32px | 12px | surface + glass | radius.lg | Center | - |

## 6. 元素级视觉定义

| 元素ID | 来源 Mock 元素 | 元素类型 | 展示内容 | 视觉角色 | 字体 / 字号 / 字重 | 颜色 Token | 背景 / 边框 | 尺寸 / 最小尺寸 | 状态样式摘要 | Figma 节点建议 |
|---|---|---|---|---|---|---|---|---|---|---|
| E-001 | E-001 | button | < 图标 | support | - | text.default | - | 44x44 | - | Circle Frame |
| E-002 | E-002 | card | 选择文档 | content | base / medium | text.default | surface / border.default | H: 160px | Hover: surfaceOverlay | Square Frame |
| E-003 | E-003 | card | 拍照/相册 | content | base / medium | text.default | surface / border.default | H: 160px | Hover: surfaceOverlay | Square Frame |
| E-004 | E-004 | text | 格式说明 | support | xs / regular | text.muted | - | - | - | Text |
| E-005 | E-005 | progress | [N]% | content | lg / bold | text.default | shadow.glowPrimary | 120x120 | 动态填充 | Ring Path |
| E-006 | E-006 | text | 正在通过 AI... | content | base / regular | text.default | - | - | - | Text |
| E-007 | E-007 | image | 错误图标 | warning | - | status.error | - | 64x64 | - | Illustration |
| E-008 | E-008 | button | 重新上传 | primary | base / semibold | text.default | Transparent / border.default | H: 48px | - | Ghost Button |

## 7. 内容与样式绑定表

| 内容对象ID | 来源 Mock 内容 | 展示文案 / 媒体描述 | 内容来源类型 | 样式 Token 绑定 | 布局位置 | 备注 |
|---|---|---|---|---|---|---|
| C-001 | E-002 | 选择文档 (PDF/DOCX) | 静态 | size.base, medium | Card 1 Bottom | |
| C-002 | E-003 | 拍照/相册上传 (OCR) | 静态 | size.base, medium | Card 2 Bottom | |
| C-003 | E-004 | 支持 PDF, DOCX... | 静态 | size.xs, text.muted | Below Cards | |
| C-004 | E-006 | 正在通过 AI 深度解析... | 静态 | size.base, regular | Below Progress | |
| C-005 | S-004 | 简历解析失败... | 静态 | size.base, status.error | Center | |

## 8. 状态展示样式

| 状态ID | 来源 Mock 状态 | 状态类型 | 展示内容 | 视觉样式 | 色彩 / 图标 / 媒体处理 | 空间占位 | 可访问性说明 |
|---|---|---|---|---|---|---|---|
| STATE-001 | STATE-001 | disabled | 卡片置灰 | opacity: 0.3 | text.disabled | 保持 E-002 占位 | 读屏应读出“不可点击” |
| STATE-002 | STATE-002 | loading | 进度环增长 | 环形渐变填充 | accent.blue | 居中显示 | 实时播报百分比 |
| STATE-003 | STATE-003 | error | 错误浮层 | 全屏玻璃覆盖 | status.error (微弱背景色) | 覆盖 S-002 | 强调重试操作 |

## 9. 响应式布局规则

| 断点 | 页面宽度范围 | Frame 布局 | 导航 / Header | 主内容布局 | 列表 / 表格 / 卡片变化 | 间距调整 | 优先隐藏或折叠内容 |
|---|---|---|---|---|---|---|---|
| mobile | < 768px | Vertical | 居左 | 单列堆叠 | 卡片全宽 (减去 Gutter) | space.6 (32px) | - |
| tablet | 768px - 1023px | Vertical | 居左 | 并列两列 | 卡片宽度固定 300px | space.7 (48px) | - |
| desktop | 1024px+ | Vertical | 居左 | 并列两列 | 卡片宽度固定 320px | space.8 (64px) | - |

## 10. AI 可读样式结构

```yaml
page:
  id: "U-020-010"
  name: "Resume Upload"
  source_mock: "product/release/mock/030-upload.md"
  design_system: "design/ios26-liquid-glass/"
  output: "product/release/design/030-upload.md"
  canvas:
    background_token: "color.background.canvas"
  background_effects:
    - type: "blur_blob"
      color: "accent.purple"
      position: "center"
      size: "600px"
      blur: "150px"
      opacity: 0.1
  frames:
    - id: "frame-root"
      type: "frame"
      layout: "vertical"
      children:
        - id: "nav-bar"
          type: "frame"
          layout: "horizontal"
          padding: 16
          children: ["E-001", "Title_Text"]
        - id: "selection-area"
          type: "frame"
          layout: "vertical"
          padding: 40
          gap: 24
          children: ["E-002", "E-003", "E-004"]
        - id: "loading-overlay"
          type: "frame"
          visibility: "hidden" # Default hidden
          background: "background.surfaceOverlay"
          backdrop_filter: "blur(24px)"
          children: ["E-005", "E-006", "M-001"]
  components:
    - id: "upload-card"
      type: "button"
      background: "background.surface"
      border: "border.default"
      radius: "radius.lg"
      backdrop_filter: "blur(24px)"
      states:
        hover:
          background: "background.surfaceOverlay"
```

## 11. Figma Remote MCP 生成提示

| 项目 | 指令 |
|---|---|
| Frame 创建顺序 | Canvas -> Nav -> Selection Area (Cards) -> Loading Overlay (Hidden by default) |
| Auto Layout 设置 | Selection Area 使用 Vertical Auto Layout, Padding 40, Gap 24. |
| Token 应用方式 | 卡片使用 radius.lg, border.default. 文字使用 size.base medium. |
| 组件分组 | 将文件上传和拍照上传定义为同一种 Component 的不同 Variant (Icon 不同). |
| 文本节点命名 | 卡片标题为 "Upload_Label", 说明文案为 "Format_Hint". |
| 响应式变体 | Mobile 保持单列. Tablet 设为 Horizontal 布局. |
| 生成时禁止事项 | 不生成文件选择对话框、不生成解析算法逻辑。 |

## 12. 设计决策记录

| 决策ID | 决策内容 | 依据 | 影响范围 |
|---|---|---|---|
| DD-001 | 采用两个大面积玻璃卡片作为入口。 | 强化“上传”这一物理动作的仪式感。 | 页面核心布局 |
| DD-002 | 解析时使用全屏覆盖的玻璃层。 | 让用户完全聚焦在解析进度上，排除其他干扰。 | 页面加载态 |
| DD-003 | 进度环内置 AI 波纹动画。 | 视觉化展示 AI 的“工作过程”，提升产品信任度。 | 解析动画设计 |

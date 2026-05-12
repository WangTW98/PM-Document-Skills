# 首页 Design Release

> 输出路径：`product/release/design/020-home.md`。本文档描述页面展示内容与样式布局，不描述交互执行、埋点、接口、后端或业务处理逻辑。

# 首页 Design Release

## 0. 文档状态

| 字段 | 内容 |
|---|---|
| 文档版本 | Release |
| 生成日期 | 2026-05-12 |
| 来源 Mock 文件 | `product/release/mock/020-home.md` |
| 设计约束目录 | `design/ios26-liquid-glass/` |
| 当前输出文件 | `product/release/design/020-home.md` |
| 页面名称 | 首页 |
| 内容范围 | 页面展示内容 + 视觉样式 + 布局结构 + AI 可读样式结构 |
| 不包含范围 | 交互执行 / 埋点 / 接口 / 后端 / 业务流程 / 实现代码 |

## 1. 页面设计综述

| 项目 | 内容 |
|---|---|
| 页面定位 | App 核心工作台，提供快速优化入口及近期历史查看。 |
| 目标阅读对象 | 设计师 / 产品 / Figma Remote MCP agent |
| 视觉目标 | 营造一种高效且具有 AI 智慧感的氛围，通过磨砂玻璃层级区分核心操作区与辅助记录区。 |
| 信息层级 | 1. 开启优化主入口（Action Card）；2. 剩余额度提醒；3. 近期优化记录列表；4. 底部 Tab 导航。 |
| 主要视觉焦点 | 页面中心的“开启 AI 简历优化”主按钮及其所属的悬浮玻璃卡片。 |
| 设计系统应用摘要 | iOS26 Liquid Glass：底层黑背景 + 动态 Aurora (Cyan/Blue) + 悬浮玻璃卡片 + 底部悬浮 Tab。 |

## 2. 设计约束提取

| 类型 | Token / 规则 | 取值 | 使用方式 | 来源文件 |
|---|---|---|---|---|
| color | background.canvas | #000000 | 页面底层背景 | DESIGN.md |
| color | background.surfaceOverlay | rgba(255, 255, 255, 0.12) | 卡片 Hover 或高亮态 | DESIGN.md |
| color | accent.cyan | #32ADE6 | 背景流动微光颜色 (核心操作区下方) | DESIGN.md |
| typography | size.lg | 20px | 模块标题字号 | DESIGN.md |
| typography | size.sm | 14px | 辅助信息 / 额度文案 | DESIGN.md |
| space | space.4 | 16px | 列表项内部间距 | DESIGN.md |
| radius | radius.md | 14px | 简历记录卡片圆角 | DESIGN.md |
| component | glassEffect.blur | blur(24px) | 全局玻璃效果 | DESIGN.md |

## 3. 页面结构图

```mermaid
mindmap
  root((首页 Design))
    Frame: Page
      Background: Aurora Layers
        Blob: Cyan Glow (Top Center)
        Blob: Blue Glow (Middle Right)
      Frame: Main Content (Scrollable)
        Frame: Header (S-001)
          Element: Greeting (E-001) + VIP Badge (E-002)
        Frame: Hero Action Card (S-002)
          Element: Quota Text (E-003)
          Element: Primary CTA Button (E-004)
        Frame: History Section (S-003)
          Element: Section Title (E-005)
          Frame: List Container
            Element: Resume Card 1 (E-006)
            Element: Resume Card 2
      Frame: Floating Navigation (S-005)
        Element: Tab Bar (E-008)
```

## 4. 自然语言样式描述

### 4.1 整体画面

- **页面整体氛围**：专业且充满科技感，深色背景下的青色（Cyan）微光暗示了 AI 的活跃状态。
- **背景与层级**：画布背景为纯黑。顶部欢迎语下方有一个巨大的、带有强模糊效果的玻璃卡片（Hero Card），承载核心功能。下方列表区则采用更为轻盈的 translucent 背景。
- **视觉重心**：页面上方的 Hero Card 具有最大的视觉权重，通过 `shadow.glowPrimary` 使其在深色背景中脱颖而出。
- **阅读节奏**：快速扫视欢迎语 -> 关注核心按钮 -> 下拉查看历史记录。

### 4.2 关键区块叙述

| 区块ID | 区块名称 | 展示内容摘要 | 样式叙述 | 视觉优先级 | 设计决策 |
|---|---|---|---|---|---|
| S-001 | 头部欢迎区 | 昵称 + VIP 勋章 | 左对齐。用户昵称使用 text.default，VIP 勋章紧随其后。背景通过一个微弱的顶部渐变光衔接。 | 中 | VIP 勋章使用 accent.blue 发光效果。 |
| S-002 | 额度与入口区 | 开启优化按钮 | 这是一个巨大的悬浮玻璃卡片。背景为 surfaceOverlay，带 24px 圆角。按钮位于中心，使用极大的 primary glow。 | 高 | 额度文案放在按钮上方，使用 text.muted。 |
| S-003 | 历史记录区 | 简历列表卡片 | 列表项使用 surfaceMuted 背景，14px 圆角。左右留白与 Hero Card 对齐。 | 中 | 采用横向拉通布局，单项高度约 80px。 |
| S-005 | 全局导航区 | 底部 Tab | 悬浮在页面底部的窄条玻璃栏，宽度约 90%，圆角 full。选中态图标带底部小圆点。 | 低 | 悬浮布局以增强 Liquid Glass 的层级感。 |

## 5. 布局与区块样式表

| 区块ID | 来源 Mock 区块 / 元素 | Frame 层级 | 布局方式 | 尺寸 / 约束 | Padding | Gap | 背景 / 边框 / 阴影 | 圆角 | 对齐 | 响应式变化 |
|---|---|---|---|---|---|---|---|---|---|---|
| Page | - | Root | Vertical | Fill Window | 0 | 0 | background.canvas | 0 | Center | - |
| S-001 | S-001 | Header | Horizontal | Width: 100% | 24px 20px | 8px | Transparent | - | Center Left | - |
| S-002 | S-002 | Section | Vertical | Width: 100% | 20px | 16px | surface + glass | radius.lg | Center | - |
| S-003 | S-003 | Section | Vertical | Width: 100% | 24px 20px | 12px | Transparent | - | Left | - |
| S-005 | S-005 | Nav | Horizontal | Width: 90% | 12px 32px | - | surfaceOverlay + blur | radius.full | Center | Fixed Bottom |

## 6. 元素级视觉定义

| 元素ID | 来源 Mock 元素 | 元素类型 | 展示内容 | 视觉角色 | 字体 / 字号 / 字重 | 颜色 Token | 背景 / 边框 | 尺寸 / 最小尺寸 | 状态样式摘要 | Figma 节点建议 |
|---|---|---|---|---|---|---|---|---|---|---|
| E-001 | E-001 | text | 你好，[昵称] | content | base / base / medium | text.default | - | - | - | Text |
| E-002 | E-002 | icon | 金色皇冠 | support | - | accent.blue | - | 20x20 | Glow effect | Icon w/ Effects |
| E-003 | E-003 | text | 剩余次数：[N] | support | sm / regular | text.muted | - | - | Critical: status.error | Text |
| E-004 | E-004 | button | 开启 AI 简历优化 | primary | lg / bold | action.primary.text | action.primary.background | H: 56px | shadow.glowPrimary | Filled Button |
| E-005 | E-005 | text | 近期优化记录 | content | base / lg / semibold | text.default | - | - | - | Text |
| E-006 | E-006 | card | 简历记录卡片 | content | - | - | surfaceMuted | H: 82px | Border: border.default | Glass Card Frame |
| E-008 | E-008 | nav | Tab 按钮 | support | sm / medium | text.muted | - | - | Active: text.default | Vertical Stack |

## 7. 内容与样式绑定表

| 内容对象ID | 来源 Mock 内容 | 展示文案 / 媒体描述 | 内容来源类型 | 样式 Token 绑定 | 布局位置 | 备注 |
|---|---|---|---|---|---|---|
| C-001 | E-001 | 你好，求职者 | 动态 | size.base, medium | Header Left | |
| C-002 | E-003 | 剩余免费次数：3 | 动态 | size.sm, text.muted | Above CTA Button | |
| C-003 | E-004 | 开启 AI 简历优化 | 静态 | size.lg, bold | Hero Card Center | |
| C-004 | DATA-001 | 互联网产品经理 | 动态 | size.base, medium | Resume Card Title | |
| C-005 | DATA-001 | 匹配度：92% | 动态 | size.sm, accent.blue | Resume Card Right | |

## 8. 状态展示样式

| 状态ID | 来源 Mock 状态 | 状态类型 | 展示内容 | 视觉样式 | 色彩 / 图标 / 媒体处理 | 空间占位 | 可访问性说明 |
|---|---|---|---|---|---|---|---|
| STATE-001 | STATE-001 | warning | 额度已用完 | 文字变红并显示升级按钮 | status.error | 保持 E-003 位置 | 红色背景降低饱和度以适配黑底 |
| STATE-002 | STATE-002 | loading | 骨架屏 | 灰色脉冲条 | surfaceMuted | 模拟 3 个 E-006 卡片 | 动画频率 1.5s |
| S-004 | S-004 | empty | 暂无记录插画 | 居中的玻璃质感插画 | opacity: 0.6 | 占据 S-003 空间 | |

## 9. 响应式布局规则

| 断点 | 页面宽度范围 | Frame 布局 | 导航 / Header | 主内容布局 | 列表 / 表格 / 卡片变化 | 间距调整 | 优先隐藏或折叠内容 |
|---|---|---|---|---|---|---|---|
| mobile | < 768px | Vertical | 居左欢迎语 | 单列垂直堆叠 | 卡片全宽 (335px) | gutterMobile: 20px | - |
| tablet | 768px - 1023px | Vertical | 居左欢迎语 | 单列居中 | 限制最大宽度 600px | space.7 (48px) | - |
| desktop | 1024px+ | Horizontal | 侧边栏 (可选) | 左右分栏 | 记录区可变为多列网格 | space.8 (64px) | - |

## 10. AI 可读样式结构

```yaml
page:
  id: "U-020"
  name: "Home Dashboard"
  source_mock: "product/release/mock/020-home.md"
  design_system: "design/ios26-liquid-glass/"
  output: "product/release/design/020-home.md"
  canvas:
    background_token: "color.background.canvas"
    max_width: "1200px"
  background_effects:
    - type: "blur_blob"
      color: "accent.cyan"
      position: "top_center"
      size: "500px"
      blur: "120px"
      opacity: 0.15
    - type: "blur_blob"
      color: "accent.blue"
      position: "middle_right"
      size: "400px"
      blur: "100px"
      opacity: 0.1
  frames:
    - id: "frame-root"
      name: "Dashboard"
      type: "frame"
      layout: "vertical"
      children:
        - id: "welcome-section"
          type: "frame"
          layout: "horizontal"
          padding: 24
          children: ["E-001", "E-002"]
        - id: "hero-section"
          type: "frame"
          background: "background.surface"
          backdrop_filter: "blur(24px)"
          radius: "radius.lg"
          margin: 20
          padding: 24
          children: ["E-003", "E-004"]
        - id: "history-section"
          type: "frame"
          padding: 24
          children:
            - id: "history-title"
              type: "text"
              content: "近期优化记录"
            - id: "history-list"
              type: "list"
              gap: 12
              children: ["E-006-items"]
  components:
    - id: "floating-tab-bar"
      source_element_id: "S-005"
      type: "nav"
      position: "fixed_bottom"
      background: "background.surfaceOverlay"
      backdrop_filter: "blur(24px)"
      radius: "radius.full"
      width: "90%"
      height: 64
```

## 11. Figma Remote MCP 生成提示

| 项目 | 指令 |
|---|---|
| Frame 创建顺序 | Canvas -> Aurora Blobs -> Scroll View -> Header -> Hero Card -> List -> Tab Bar (Floating) |
| Auto Layout 设置 | Hero Card 设为 Vertical, Padding 24, Alignment Center. 列表项 Gap 12. |
| Token 应用方式 | 顶部 Header 文字使用 size.base medium. 按钮使用 size.lg bold. |
| 组件分组 | 每一个 Resume Card 封装为独立 Component 实例。 |
| 文本节点命名 | 简历标题为 "Resume_Name", 匹配分为 "Match_Score", 日期为 "Date_Label"。 |
| 响应式变体 | Mobile 保持单列。Desktop 宽度拉伸至 600px。 |
| 生成时禁止事项 | 不生成埋点逻辑、不生成接口刷新逻辑。 |

## 12. 设计决策记录

| 决策ID | 决策内容 | 依据 | 影响范围 |
|---|---|---|---|
| DD-001 | 将“开启优化”按钮置于独立的显眼玻璃卡片中。 | 核心功能优先级最高，需最快被用户识别。 | 页面视觉重心 |
| DD-002 | 底部导航栏采用悬浮圆角长条样式。 | 增加页面的现代感和通透性，符合 Liquid Glass 的浮动感。 | 全局导航展示 |
| DD-003 | 简历列表卡片使用更薄的透明度（surfaceMuted）。 | 与 Hero Card 形成层级对比，减少视觉上的沉重感。 | 历史记录区 |

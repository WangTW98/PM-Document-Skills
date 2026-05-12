# 页面 Design Release 模板

> 输出路径：`product/release/design/<同名 release mock 文件>.md`。每次只生成一个页面。本文档描述页面展示内容与样式布局，不描述交互执行、埋点、接口、后端或业务处理逻辑。

# <页面名称> Design Release

## 0. 文档状态

| 字段 | 内容 |
|---|---|
| 文档版本 | Release |
| 生成日期 |  |
| 来源 Mock 文件 | `product/release/mock/<page>.md` |
| 设计约束目录 | `design/<design-system>/` |
| 当前输出文件 | `product/release/design/<page>.md` |
| 页面名称 |  |
| 内容范围 | 页面展示内容 + 视觉样式 + 布局结构 + AI 可读样式结构 |
| 不包含范围 | 交互执行 / 埋点 / 接口 / 后端 / 业务流程 / 实现代码 |

## 1. 页面设计综述

| 项目 | 内容 |
|---|---|
| 页面定位 |  |
| 目标阅读对象 | 设计师 / 产品 / 后续 AI 设计生成 agent / Figma Remote MCP agent |
| 视觉目标 |  |
| 信息层级 |  |
| 主要视觉焦点 |  |
| 设计系统应用摘要 |  |

## 2. 设计约束提取

| 类型 | Token / 规则 | 取值 | 使用方式 | 来源文件 |
|---|---|---|---|---|
| color |  |  |  | DESIGN.md / tokens.json |
| typography |  |  |  | DESIGN.md / tokens.json |
| space |  |  |  | DESIGN.md / tokens.json |
| radius |  |  |  | DESIGN.md / tokens.json |
| shadow / elevation |  |  |  | DESIGN.md / tokens.json |
| breakpoint |  |  |  | DESIGN.md / tokens.json |

## 3. 页面结构图

```mermaid
mindmap
  root((页面名称 Design))
    Frame
      Header
      Main
        SectionA
        SectionB
      FooterOrNavigation
    Responsive
      Mobile
      Tablet
      Desktop
```

## 4. 自然语言样式描述

### 4.1 整体画面

- 页面整体氛围：
- 背景与层级：
- 视觉重心：
- 阅读节奏：

### 4.2 关键区块叙述

| 区块ID | 区块名称 | 展示内容摘要 | 样式叙述 | 视觉优先级 | 设计决策 |
|---|---|---|---|---|---|
| S-001 |  |  |  | 高 / 中 / 低 |  |

## 5. 布局与区块样式表

| 区块ID | 来源 Mock 区块 / 元素 | Frame 层级 | 布局方式 | 尺寸 / 约束 | Padding | Gap | 背景 / 边框 / 阴影 | 圆角 | 对齐 | 响应式变化 |
|---|---|---|---|---|---|---|---|---|---|---|
| S-001 |  | Page / Header / Main / Section / Group | vertical / horizontal / grid / overlay |  |  |  |  |  |  |  |

## 6. 元素级视觉定义

| 元素ID | 来源 Mock 元素 | 元素类型 | 展示内容 | 视觉角色 | 字体 / 字号 / 字重 | 颜色 Token | 背景 / 边框 | 尺寸 / 最小尺寸 | 状态样式摘要 | Figma 节点建议 |
|---|---|---|---|---|---|---|---|---|---|---|
| E-001 |  | button / input / image / text / card / table / list / chart / nav / modal / toast / media / other |  | primary / secondary / content / support / warning |  |  |  |  |  |  |

## 7. 内容与样式绑定表

| 内容对象ID | 来源 Mock 内容 | 展示文案 / 媒体描述 | 内容来源类型 | 样式 Token 绑定 | 布局位置 | 备注 |
|---|---|---|---|---|---|---|
| C-001 |  |  | 静态 / 动态 | color / typography / space / radius / elevation |  |  |

## 8. 状态展示样式

> 仅描述状态下用户看到的内容与样式，不描述触发条件、执行动作、接口或埋点。

| 状态ID | 来源 Mock 状态 | 状态类型 | 展示内容 | 视觉样式 | 色彩 / 图标 / 媒体处理 | 空间占位 | 可访问性说明 |
|---|---|---|---|---|---|---|---|
| STATE-001 |  | loading / empty / error / disabled / success / warning / limited / media-failed |  |  |  |  |  |

## 9. 响应式布局规则

| 断点 | 页面宽度范围 | Frame 布局 | 导航 / Header | 主内容布局 | 列表 / 表格 / 卡片变化 | 字体 / 间距调整 | 优先隐藏或折叠内容 |
|---|---|---|---|---|---|---|---|
| mobile |  |  |  |  |  |  |  |
| tablet |  |  |  |  |  |  |  |
| desktop |  |  |  |  |  |  |  |
| wide |  |  |  |  |  |  |  |

## 10. AI 可读样式结构

```yaml
page:
  id: "<page-id>"
  name: "<page-name>"
  source_mock: "product/release/mock/<page>.md"
  design_system: "design/<design-system>/"
  output: "product/release/design/<page>.md"
  canvas:
    background_token: ""
    max_width: ""
    responsive_breakpoints: []
  frames:
    - id: "frame-root"
      name: "Page"
      type: "frame"
      layout: "vertical"
      padding: ""
      gap: ""
      fill_token: ""
      children: []
  components:
    - id: "component-001"
      source_element_id: ""
      type: ""
      content: ""
      tokens:
        typography: ""
        text_color: ""
        fill: ""
        radius: ""
        shadow: ""
      constraints:
        width: ""
        height: ""
        min_touch_target: ""
      states:
        default: {}
        loading: {}
        empty: {}
        error: {}
        disabled: {}
```

## 11. Figma Remote MCP 生成提示

| 项目 | 指令 |
|---|---|
| Frame 创建顺序 |  |
| Auto Layout 设置 |  |
| Token 应用方式 |  |
| 组件分组 |  |
| 文本节点命名 |  |
| 媒体占位 |  |
| 响应式变体 |  |
| 生成时禁止事项 | 不生成交互原型、不生成埋点、不生成接口逻辑、不生成业务流程 |

## 12. 设计决策记录

| 决策ID | 决策内容 | 依据 | 影响范围 |
|---|---|---|---|
| DD-001 |  | 来源 Mock + 设计系统约束 |  |

# 会员订阅页 Design Release

> 输出路径：`product/release/design/090-vip-subscription.md`。本文档描述页面展示内容与样式布局，不描述交互执行、埋点、接口、后端或业务处理逻辑。

# 会员订阅页 Design Release

## 0. 文档状态

| 字段 | 内容 |
|---|---|
| 文档版本 | Release |
| 生成日期 | 2026-05-12 |
| 来源 Mock 文件 | `product/release/mock/090-vip-subscription.md` |
| 设计约束目录 | `design/ios26-liquid-glass/` |
| 当前输出文件 | `product/release/design/090-vip-subscription.md` |
| 页面名称 | 会员订阅页 |
| 内容范围 | 页面展示内容 + 视觉样式 + 布局结构 + AI 可读样式结构 |
| 不包含范围 | 交互执行 / 埋点 / 接口 / 后端 / 业务流程 / 实现代码 |

## 1. 页面设计综述

| 项目 | 内容 |
|---|---|
| 页面定位 | 核心商业化页面，展示 VIP 权益并提供购买入口。 |
| 目标阅读对象 | 设计师 / 产品 / Figma Remote MCP agent |
| 视觉目标 | 通过极致的“黑金玻璃”质感营造稀缺性与尊贵感，利用流畅的 Aurora 动画吸引用户完成订阅转化。 |
| 信息层级 | 1. 会员特权头图（价值感知）；2. 订阅套餐选择（决策路径）；3. 支付方式选择；4. 协议与支付确认。 |
| 主要视觉焦点 | 顶部流光溢彩的“黑金会员”展示卡片。 |
| 设计系统应用摘要 | iOS26 Liquid Glass：底层黑背景 + 动态 Aurora (Gold/Purple) + 磨砂金边框 + 订阅卡片高亮。 |

## 2. 设计约束提取

| 类型 | Token / 规则 | 取值 | 使用方式 | 来源文件 |
|---|---|---|---|---|
| color | background.canvas | #000000 | 页面底层背景 | DESIGN.md |
| color | accent.gold | #FFD700 | VIP 卡片核心色 / 选中态边框 | - (扩展自 Liquid Glass) |
| color | accent.purple | #BF5AF2 | 装饰性背景光斑 | DESIGN.md |
| typography | size.lg | 20px | 权益标题与套餐名称 | DESIGN.md |
| typography | size.xxl | 40px | 套餐价格字号 | DESIGN.md |
| space | space.5 | 24px | 模块间边距 | DESIGN.md |
| radius | radius.lg | 24px | 订阅套餐卡片圆角 | DESIGN.md |
| shadow | glowAccent | 0 10px 40px rgba(191, 90, 242, 0.3) | 选中卡片的背光效果 | DESIGN.md |

## 3. 页面结构图

```mermaid
mindmap
  root((会员订阅页 Design))
    Frame: Page
      Background: Aurora Layers
        Blob: Gold Glow (Top)
        Blob: Purple Glow (Bottom)
      Frame: Content Layer
        Frame: Navigation (S-001)
          Element: Close Button
          Element: Page Title
        Frame: VIP Benefit Card (S-001)
          Element: Black Gold Texture
          Element: Benefit List (E-001)
        Frame: Pricing Section (S-002)
          Element: Monthly Plan (E-002)
          Element: Annual Plan (E-003) - Recommended
        Frame: Payment Section (S-003)
          Element: Payment Radio Group (E-004)
        Frame: Footer Action (S-004)
          Element: Checkout Button (E-005)
          Element: Agreement Checkbox (E-006)
```

## 4. 自然语言样式描述

### 4.1 整体画面

- **页面整体氛围**：奢华、沉浸、专业。通过深浅不一的黑色玻璃层级与流动的金色微光交织，建立高端订阅的品牌感知。
- **背景与层级**：画布为纯黑。背景顶部区域带有动态的金色 Aurora 效果，这种光影会随着手机陀螺仪（或动画模拟）产生微妙的偏移。
- **视觉重心**：选中的年度订阅卡片。该卡片具有最强的 `shadow.glowAccent`，并带有“立省 ¥70”的精致悬浮标签。
- **阅读节奏**：浏览顶部特权 -> 选择中间最划算的套餐 -> 确认底部支付。

### 4.2 关键区块叙述

| 区块ID | 区块名称 | 展示内容摘要 | 样式叙述 | 视觉优先级 | 设计决策 |
|---|---|---|---|---|---|
| S-001 | 权益展示区 | 会员卡片 + 列表 | 卡片采用“黑金玻璃”纹理，具有磨砂金边缘。特权列表项带金色 Checkmark 图标，文字为 White 0.9 透明度。 | 高 | 将最核心价值点（无限分析）置于卡片正中。 |
| S-002 | 套餐选择区 | 套餐卡片集合 | 并列布局。卡片背景为 `surfaceOverlay`。选中项增加 2px `accent.gold` 描边，且其对应的价格文字带有金色发光。 | 极高 | 通过视觉差（描边与发光）引导用户选择高客单价的年度套餐。 |
| S-003 | 支付方式区 | 渠道列表 | 扁平化设计。支付项被包裹在圆角 radius.md 的玻璃容器中。选中状态通过右侧的 Radio Button 颜色切换。 | 中 | 保持支付环节的简洁与信任感。 |
| S-004 | 底部操作区 | 支付按钮 + 协议 | 按钮使用 action.primary.background，文案字号 base bold。协议文字使用 sm 字号，text.muted 色。 | 极高 | 赋予支付按钮极强的视觉吸引力。 |

## 5. 布局与区块样式表

| 区块ID | 来源 Mock 区块 / 元素 | Frame 层级 | 布局方式 | 尺寸 / 约束 | Padding | Gap | 背景 / 边框 / 阴影 | 圆角 | 对齐 | 响应式变化 |
|---|---|---|---|---|---|---|---|---|---|---|
| Page | - | Root | Vertical | Fill Window | 0 | 0 | background.canvas | 0 | Center | - |
| S-001 | S-001 | Header | Vertical | Width: 335px | 24px | 16px | surface (Black Gold) | radius.lg | Top | Shadow: glass |
| S-002 | S-002 | Section | Horizontal | Width: 100% | 0 20px | 12px | Transparent | - | Center | - |
| Card | E-002 / E-003 | Group | Vertical | Width: 160px | 20px | 8px | surfaceOverlay | radius.md | Center | Active: border.gold |
| S-004 | S-004 | Footer | Vertical | Width: 100% | 24px 20px | 12px | surfaceOverlay + blur | - | Center | Fixed Bottom |

## 6. 元素级视觉定义

| 元素ID | 来源 Mock 元素 | 元素类型 | 展示内容 | 视觉角色 | 字体 / 字号 / 字重 | 颜色 Token | 背景 / 边框 | 尺寸 / 最小尺寸 | 状态样式摘要 | Figma 节点建议 |
|---|---|---|---|---|---|---|---|---|---|---|
| E-001 | E-001 | list | 权益列表 | content | base / medium | text.default | - | - | - | Icon Text List |
| E-002 | E-002 | card | 连续包月 | primary | lg / bold | text.default | surfaceOverlay | 160x180 | - | Glass Card |
| E-003 | E-003 | card | 年度会员 | primary | lg / bold | accent.gold | surfaceOverlay | 160x180 | Active: glow | Glass Card |
| E-004 | E-004 | radio_list | 支付方式 | support | sm / regular | text.default | - | - | - | Row List |
| E-005 | E-005 | button | 立即支付 | action | base / bold | action.primary.text | action.primary.background | H: 52px | shadow.glowPrimary | Filled Button |
| E-006 | E-006 | checkbox | 同意协议 | support | xs / regular | text.muted | - | - | - | Checkbox Row |

## 7. 内容与样式绑定表

| 内容对象ID | 来源 Mock 内容 | 展示文案 / 媒体描述 | 内容来源类型 | 样式 Token 绑定 | 布局位置 | 备注 |
|---|---|---|---|---|---|---|
| C-001 | E-001 | 无限次 AI 深度解析 | 静态 | size.base, medium | Benefit Item 1 | |
| C-002 | E-002 | ¥19.9 | 动态 | size.xxl, bold | Card 1 Center | |
| C-003 | E-003 | ¥168 | 动态 | size.xxl, bold, accent.gold | Card 2 Center | |
| C-004 | E-005 | 立即支付 ¥19.9 | 动态 | size.base, bold | Button Center | |
| C-005 | M-001 | VIP 会员 | 静态 | size.lg, gold | Card Header | |

## 8. 状态展示样式

| 状态ID | 来源 Mock 状态 | 状态类型 | 展示内容 | 视觉样式 | 色彩 / 图标 / 媒体处理 | 空间占位 | 可访问性说明 |
|---|---|---|---|---|---|---|---|
| STATE-001 | STATE-001 | loading | 支付中... | 按钮背景闪烁 | action.primary.background | 保持 E-005 占位 | 系统支付层弹出提示 |
| STATE-002 | STATE-002 | active | 套餐选中 | 金色边框 + 阴影发光 | accent.gold | 扩展卡片边缘 | 引导用户选择重点 |
| VIP_Badge | - | badge | 立省 ¥70 | 渐变背景胶囊 | #FF453A -> #BF5AF2 | 卡片右上角 | |

## 9. 响应式布局规则

| 断点 | 页面宽度范围 | Frame 布局 | 导航 / Header | 主内容布局 | 列表 / 表格 / 卡片变化 | 间距调整 | 优先隐藏或折叠内容 |
|---|---|---|---|---|---|---|---|
| mobile | < 768px | Vertical | 居中 | 堆叠布局 | 套餐双列排列 | space.5 (24px) | - |
| tablet | 768px - 1023px | Vertical | 居中 | 水平对齐 | 套餐三列排列 | space.7 (48px) | - |
| desktop | 1024px+ | Horizontal | 侧边栏 (可选) | 详情居左支付居右 | 权益与套餐侧重对齐 | space.8 (64px) | - |

## 10. AI 可读样式结构

```yaml
page:
  id: "U-030-010"
  name: "VIP Subscription"
  source_mock: "product/release/mock/090-vip-subscription.md"
  design_system: "design/ios26-liquid-glass/"
  output: "product/release/design/090-vip-subscription.md"
  canvas:
    background_token: "color.background.canvas"
  background_effects:
    - type: "blur_blob"
      color: "accent.gold"
      position: "top_center"
      size: "500px"
      blur: "120px"
      opacity: 0.08
    - type: "blur_blob"
      color: "accent.purple"
      position: "bottom_right"
      size: "400px"
      blur: "100px"
      opacity: 0.12
  frames:
    - id: "frame-root"
      type: "frame"
      layout: "vertical"
      children:
        - id: "benefit-header"
          type: "frame"
          background: "linear-gradient(135deg, rgba(30,30,30,0.8), rgba(10,10,10,0.9))"
          border: "1px solid accent.gold"
          padding: 24
          children: ["E-001-list"]
        - id: "pricing-options"
          type: "frame"
          layout: "horizontal"
          padding_vertical: 32
          gap: 16
          children: ["E-002-card", "E-003-card"]
        - id: "payment-methods"
          type: "frame"
          padding: 20
          children: ["E-004-list"]
        - id: "action-footer"
          type: "frame"
          background: "background.surfaceOverlay"
          backdrop_filter: "blur(24px)"
          children: ["E-005", "E-006"]
  components:
    - id: "pricing-card"
      type: "card"
      background: "background.surfaceOverlay"
      radius: "radius.lg"
      active_state:
        border: "accent.gold"
        shadow: "shadow.glowAccent"
```

## 11. Figma Remote MCP 生成提示

| 项目 | 指令 |
|---|---|
| Frame 创建顺序 | Canvas -> Background Gold Blob -> VIP Card Header -> Pricing Horizontal Section -> Payment List -> Footer CTA |
| Auto Layout 设置 | Pricing Section 使用 Horizontal Auto Layout, Gap 12, Children 均分宽度. |
| Token 应用方式 | 推荐套餐使用 accent.gold 描边. 价格大字使用 size.xxl bold. |
| 组件分组 | 将“年度会员”套餐定义为 Component Set 中的 "Highlighted" 变体. |
| 文本节点命名 | 价格文案为 "Price_Value", 权益项为 "Benefit_Text", 优惠标签为 "Promo_Badge". |
| 响应式变体 | Mobile 下套餐双列。Desktop 下可增加背景装饰（如流动的极光线条）. |
| 生成时禁止事项 | 不生成真实的内购（IAP）支付弹窗、不生成外部跳转链接. |

## 12. 设计决策记录

| 决策ID | 决策内容 | 依据 | 影响范围 |
|---|---|---|---|
| DD-001 | 使用黑金质感作为会员卡片唯一基调。 | 建立清晰的 VIP 视觉隔离，暗示服务的优质与高昂价值。 | 权益展示区 |
| DD-002 | 默认高亮“年度会员”套餐。 | 商业化策略：通过折扣差异引导用户进行长期订阅。 | 套餐选择交互 |
| DD-003 | 支付按钮背景使用品牌主色加外发光。 | 转化核心动作，需要在深色背景中保持极高的视觉显著性。 | 底部操作区 |
 Riverside, CA

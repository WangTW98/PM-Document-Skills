# Figma Remote MCP 自动建构指南

下游负责对接 Figma Remote MCP 接口以生成设计稿的 AI Agent，应严格遵循此指南，将“现代化的管理后台”系统转换为结构化的 Figma 文件。

## 1. 必读输入文件
- `design/modern-admin-dashboard/tokens.json` (主要使用此文件的数据结构)
- `design/modern-admin-dashboard/DESIGN.md` (用于理解约束)

## 2. Token 到 Figma Variables/Styles 的映射
Agent 应调用相应的 Figma 节点创建指令，建立基础的 Design System 骨架。

### 2.1 颜色系统 (Local Variables / Styles)
将 `tokens.color` 的内容逐一注册为 Figma Color Styles 或是 Variables。
**命名空间规范**:
- `Color/Brand/Primary` -> `#2563eb`
- `Color/Neutral/500` -> `#cbd5e1`
- `Color/Background/Canvas` -> `#f8fafc`
- `Color/Background/Surface` -> `#ffffff`
- `Color/Text/Secondary` -> `#475569`

### 2.2 排版样式 (Text Styles)
组合 `tokens.typography` 中的属性生成 Figma Text Styles。
**命名空间规范**:
- `Heading/1` -> Inter, Semibold, 24px, Line Height 1.25
- `Heading/2` -> Inter, Semibold, 18px, Line Height 1.25
- `Body/Base` -> Inter, Regular, 16px, Line Height 1.5
- `Body/Small` -> Inter, Regular, 14px, Line Height 1.5
- `Metadata` -> Inter, Regular, 12px, Line Height 1.5

### 2.3 效果样式 (Effect Styles)
创建 Figma Drop Shadow 效果。
- `Shadow/Focus` -> 两层阴影： 0 0 0 2px #fff, 0 0 0 4px #3b82f6

## 3. Auto-Layout 与组件构建

### 基础组件要求
在 Figma 中创建 Components 时，必须完全使用 Auto-Layout 替代绝对定位。

**Button Component**:
- 设置 Auto-Layout。
- 绑定左右 Padding (如 `16px`) 和固定高度 `40px`。
- 绑定 Fill 到 `Color/Brand/Primary`，绑定 Text 到 `Color/Text/Inverse`。
- 将 Corner Radius 设置为 `6px`。

**Card Component**:
- 设置 Auto-Layout，方向为 Vertical，Gap 设置为 `16px`。
- 绑定 Padding 为 `24px`。
- 绑定 Fill 到 `Color/Background/Surface`。
- 添加 Stroke 绑定到 `Color/Border/Default`，Stroke width 为 `1px`。
- 将 Corner Radius 设置为 `8px`。

## 4. 画板 (Frame) 的响应式设置
当利用此规范生成页面级设计草图时，应至少创建两种形态的 Frame 以覆盖响应式要求：

1. **Desktop Frame**:
   - 宽度 `1440px`。
   - 包含侧边栏（宽度 240-256px）。
   - 页面背景 Fill 为 `Color/Background/Canvas`。
2. **Mobile Frame**:
   - 宽度 `375px` 或 `393px`。
   - 不展示左侧边栏，顶部放置汉堡菜单 Header。
   - 所有内部宽幅组件（如表格）改为横向 Scroll 或垂直卡片。

## 5. 校验清单 (QA)
在 Remote MCP 执行完毕结束任务前，检查并确认：
- [ ] Figma 文件中是否含有 `Color/Background/Canvas` 和 `Color/Brand/Primary` 变量/样式？
- [ ] 组件的圆角是否被精确地设置为了 `6px` 和 `8px`，而非随意的整数？
- [ ] 文字节点是否应用了建立好的 Text Styles，还是依然属于未绑定的游离状态？

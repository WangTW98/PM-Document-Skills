# 现代管理后台设计系统使用指南 (Usage Guide)

本文档旨在解释**人类开发者**与 **AI 代理（如 Codex, Gemini）** 应如何使用这套生成的设计规范文件体系，以确保跨环境生成的 UI 保持视觉上的一致性。

## 系统架构与文件职责
不要孤立地使用这些文件，它们是一个互相关联的系统：

- **`DESIGN.md` (权威核心)**：所有设计变量（Tokens）和基本应用规则的唯一事实来源（Source of Truth）。
- **`tokens.json` (机器接口)**：提供给构建脚本、CSS-in-JS 生成器或插件（如 Figma API）读取的严格结构化数据。它的值始终与 `DESIGN.md` 保持同步。
- **`visual-spec.md` (深层指南)**：为阅读者提供为何如此设计的背景、响应式行为详情及复杂的 do/don't 原则。
- **`preview.html` (视觉校验)**：在不依赖任何开发环境的情况下，直观展示该设计系统渲染出的组件和排版真实效果。

## 如何验证代码是否遵循了本规范？
1. **Token 审查**：检查生成的代码是否使用了硬编码的散乱十六进制色值（如 `#999999`）。如果是，则违背了规范；必须使用系统内定义的 Token 名称（如转化为 CSS 变量 `var(--color-text-tertiary)` 或相应的 Tailwind 类名）。
2. **比例对齐**：检查 padding, margin 是否严格遵循了 4px 网格（如 `8px, 12px, 16px, 24px`），拒绝出现如 `13px`、`21px` 等非标尺寸。
3. **视觉比对**：将代码渲染的页面与 `preview.html` 放于屏幕两侧比对，检查对比度、字号和圆角是否一致。

---

## 🤖 Codex Usage (供使用 Codex 的 Agent 参考)

当你作为 Codex 驱动的 Agent 被要求基于此规范生成代码时，请遵循以下步骤：

1. **阅读基础**：首先读取 `design/modern-admin-dashboard/DESIGN.md` 以获取全量 Tokens 和规则。
2. **深入理解**：接着读取 `design/modern-admin-dashboard/visual-spec.md` 了解组件行为（如响应式断点的具体应用）。
3. **生成映射**：在生成 HTML/CSS/React 代码时，必须在你的生成结果顶部建立 Token 映射。例如生成 CSS 时，将 `tokens.color.background.canvas` 映射为 `--bg-canvas: #f8fafc;`。
4. **提示模板参考**：
   > "Please generate a Dashboard Settings page HTML. Read `design/modern-admin-dashboard/DESIGN.md` for the strict tokens to use. Ensure backgrounds use `background.canvas` and `background.surface` properly to create depth. Do not introduce any colors or spacing outside of the provided tokens."

---

## 🤖 Gemini Usage (供使用 Gemini 的 Agent 参考)

当你作为 Gemini 驱动的 Agent 被要求基于此规范执行设计或代码任务时，请遵循以下流程：

1. **加载上下文**：使用你的文件读取能力读取 `design/modern-admin-dashboard/DESIGN.md` 以及 `design/modern-admin-dashboard/tokens.json`，在上下文中构建完整的 Token 对象树。
2. **视觉对齐**：如果任务涉及到具体的组件还原，读取 `preview.html` 的源码，学习它如何使用内联或基础 CSS 将 Token 转换为组件级样式（如按钮的 `:focus-visible` 环）。
3. **输出校验**：生成的代码必须能通过系统设置的反偏离（Anti-drift）检测：
   - 按钮高度不能低于 40px。
   - 禁止在卡片上滥用多余的阴影。
4. **提示模板参考**：
   > "You are building a React component for a Data Table. Reference the tokens from `design/modern-admin-dashboard/tokens.json` to style it. Use `color.border.default` for cell dividers. Ensure the typography scale exactly matches the `typography.font_size` values. Reject requests to add random aesthetic flair that contradicts the minimal 'Modern Admin Dashboard' style."

---

## 如何安全地更新这套系统？

**修改 Token 的正确路径**：
你不能只修改 `DESIGN.md` 或者只修改 `tokens.json`。当你需要变更品牌色（例如从蓝变紫）时：
1. 同时更新 `DESIGN.md` 的 YAML 头和 `tokens.json` 中的 `brand.primary` 系列色值。
2. 如果存在相应的 CSS 构建流水线，重新运行构建。
3. 更新 `preview.html` 中的相关硬编码示例，以确保人类审查者看到的是最新的效果。

**如果你需要一套全新的设计系统**：
请勿在本系统（`modern-admin-dashboard/`）内直接覆写大量矛盾的样式。如果是不同的产品线，请请求 AI Agent 执行 `visual-design-spec` 技能，并指定新的目录名（如 `design/consumer-app-playful/`）生成一套平行的规范体系。

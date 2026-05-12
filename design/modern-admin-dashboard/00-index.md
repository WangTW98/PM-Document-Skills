# 现代化的管理后台设计规范 (Modern Admin Dashboard Design Spec)

## 1. 概述
本设计规范是一套平台中立（Platform-neutral）的视觉设计系统，专为 B 端（商业/企业级）产品、SaaS 平台以及数据密集型管理后台打造。

**生成目录**: `design/modern-admin-dashboard/`
**设计主旨**: 专业、清晰、克制。注重内容与数据的展示效率，控制视觉噪音，采用中性清爽的色调搭配高对比度交互元素，并建立严谨的网格和排版层级。

## 2. 设计意图与假设
- **输入来源**: 基于“现代化的管理后台”这一通用概念推断生成。
- **目标受众**: 职场专业人士、数据分析师、系统管理员。
- **设计风格**:
  - 极简、微质感的组件边缘（柔和的小阴影与1px的精致边框）。
  - 开阔的负空间以缓解长时间操作带来的视觉疲劳。
  - 大量使用中性灰阶（Gray scale）区分层级，主色仅用于关键行为（Primary Action）和状态传达。
- **开放性假设**: 
  - 默认使用品牌蓝（Blue）作为主色调（Primary Color），因为它在商业软件中最具信任感和普适性。
  - 默认使用系统无衬线字体栈（System Sans-serif font stack）以追求极致性能和原生体验。

## 3. 文件阅读指南

建议按以下顺序阅读和使用本系统文件：

1. **`DESIGN.md`**：【核心源文件】结合了机器可读的 Tokens（YAML格式）与人类可读的核心规则。这是 AI Agent 和开发者的 Source of Truth。
2. **`preview.html`**：【视觉演示】独立且纯净的 HTML 文件。无任何外部依赖，双击即可在浏览器中查看此规范在按钮、表单、排版和版式上的具体表现。
3. **`tokens.json`**：【结构化数据】与 `DESIGN.md` 保持完全同步的 JSON 格式 Tokens 数据，专为需要直接解析代码和自动生成组件的工具（如 Figma Plugin 或构建脚本）准备。
4. **`visual-spec.md`**：【深度规范】详尽的人类阅读设计指南。深入解释为什么这样设计、详细的响应式行为、无障碍说明和 Do's & Don'ts。
5. **`usage.md`**：【协同指南】指导人类开发者和 AI 助理如何安全、一致地应用本系统（涵盖 Codex 和 Gemini 的 Prompts 示例）。
6. **`handoff/html-generation-guide.md`**：【AI 生成指南】专门指导负责前端代码生成的 AI 如何映射 Tokens 到代码。
7. **`handoff/figma-remote-mcp-guide.md`**：【Figma 协同指南】指导 AI 通过 Remote MCP 将系统自动建构到 Figma。

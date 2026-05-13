# Material Design 3 Cross-Platform Design System

- 输出目录: `design/material-3-cross-platform/`
- 设计目标: 建立一套符合 Material Design 3 风格的跨端设计系统，支持 Mobile、Tablet、PC Web、H5 Web，并能直接用于 Figma、前端开发、AI 页面生成与验收。
- 输入来源: 本轮用户提供了风格方向、关键词和平台要求，但未提供具体产品类型、目标用户、品牌色、页面清单和核心流程。

## 已确认的设计决策

- 整体风格采用 Material Design 3 / Material You 语言，而不是 iOS 风格或强品牌个性化风格。
- 色彩体系采用 `Primary / Secondary / Tertiary / Error / Surface / Outline` 的 Color Roles 结构。
- 视觉基调强调清晰、秩序、友好、可访问性和组件化复用。
- 同时提供浅色和深色模式，并提供适用于 AI 生成与前端落地的 token。
- 响应式策略覆盖 Mobile、Tablet、Desktop、Wide Desktop，不使用“手机简单放大”方案。

## 假设与开放点

- 默认产品类型假设为工具型、企业服务型、内容效率型产品。
- 默认目标用户假设为需要高频浏览、录入、筛选、管理信息的普通用户与业务用户。
- 默认品牌策略采用偏蓝紫中性的 Material 基础调色，后续可换为品牌种子色重新生成 tonal palette。
- 默认页面范围包含: 登录、首页、Dashboard、列表、详情、表单、设置、空状态、反馈状态。

## 文件地图

- `DESIGN.md`: canonical source，含 YAML tokens 和 AI 约束。
- `visual-spec.md`: 中文完整版设计规范。
- `tokens.json`: 结构化 token 镜像。
- `preview.html`: 可视化预览页。
- `usage.md`: 使用说明。
- `handoff/html-generation-guide.md`: HTML / React / Vue / AI 页面生成指南。
- `handoff/figma-remote-mcp-guide.md`: Figma Remote MCP 建模指南。

## 推荐阅读顺序

1. 人类读者: `visual-spec.md` -> `preview.html` -> `DESIGN.md`
2. AI 页面生成: `DESIGN.md` -> `tokens.json` -> `handoff/html-generation-guide.md`
3. Figma 执行: `DESIGN.md` -> `tokens.json` -> `handoff/figma-remote-mcp-guide.md`

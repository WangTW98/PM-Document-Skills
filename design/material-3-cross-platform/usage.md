# Usage

`DESIGN.md` 是这套 Material Design 3 设计系统的唯一 canonical source。`tokens.json` 提供结构化 token，`visual-spec.md` 提供完整中文设计手册，`preview.html` 用于视觉校验。

## 如何使用

1. 先读取 `DESIGN.md`
2. 需要程序消费时读取 `tokens.json`
3. 需要检查风格时打开 `preview.html`
4. 需要端到端规范时查看 `visual-spec.md`

## Codex Usage

```text
请读取 design/material-3-cross-platform/DESIGN.md 作为唯一设计规范源，
同时参考 tokens.json、visual-spec.md 和 preview.html。
基于这套 Material Design 3 设计系统生成页面或组件，
严格使用既有 color roles、surface、typography、spacing、radius、elevation 和 breakpoints，
不要额外引入未定义的风格语言。
```

## Gemini Usage

```text
Read design/material-3-cross-platform/DESIGN.md as the canonical source.
Use tokens.json for structured tokens, visual-spec.md for rationale, and preview.html for visual QA.
Generate UI in a Material Design 3 style with correct color roles, surface hierarchy,
component states, accessibility rules, and responsive layout behavior.
```

## Figma Remote MCP 使用

- 先把 `tokens.json` 映射为颜色变量、文字样式、圆角、阴影和间距变量
- 按 `Foundations / Components / Templates / Platforms` 结构建文件
- 每个关键模板至少产出 Mobile / H5 / Tablet / PC 四套 frame

## 安全更新方式

- 先改 `DESIGN.md`
- 再同步 `tokens.json`
- 再同步 `preview.html`
- 最后同步 `visual-spec.md` 与 handoff 文档

## 验证是否符合系统

- 是否正确使用 color roles
- 是否正确使用 surface 层级
- 是否完整支持浅色 / 深色
- 是否组件状态齐全
- 是否不同端做了布局重构

## 后续新增设计系统

如需新增品牌化版本，请在 `design/` 目录下创建新的 slug 目录，不覆盖当前 Material 3 基线系统。

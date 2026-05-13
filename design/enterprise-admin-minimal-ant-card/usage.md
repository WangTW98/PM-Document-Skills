# Usage

`DESIGN.md` 是这套后台管理设计系统的唯一 canonical source。`tokens.json` 提供结构化 token，`visual-spec.md` 提供完整中文规范，`preview.html` 用于视觉校验。

## 如何使用

1. 先读取 `DESIGN.md`
2. 需要程序消费时读取 `tokens.json`
3. 需要判断视觉和布局是否正确时打开 `preview.html`
4. 需要业务后台层面的完整规则时查看 `visual-spec.md`

## Codex Usage

```text
请读取 design/enterprise-admin-minimal-ant-card/DESIGN.md 作为唯一设计规范源，
并参考 tokens.json、visual-spec.md 与 preview.html。
基于这套企业级后台设计系统生成页面或组件，
严格使用既有 token、布局骨架和组件状态，
保持企业级极简风、Ant Design 企业风和卡片化轻量风的一致性。
```

## Gemini Usage

```text
Read design/enterprise-admin-minimal-ant-card/DESIGN.md as the canonical source.
Use tokens.json for structured tokens, visual-spec.md for expanded rationale,
and preview.html for visual QA.
Generate enterprise admin UI with clear hierarchy, tables, filters, cards, drawers, and robust component states.
```

## Figma Remote MCP 使用

- 先把 `tokens.json` 映射为颜色、文字、间距、阴影、圆角变量
- 按 `Foundations / Components / Templates / Platforms` 组织文件
- 桌面后台模板至少先建 Dashboard、List、Detail、Form、Approval 五类

## 安全更新方式

- 先改 `DESIGN.md`
- 再同步 `tokens.json`
- 再同步 `preview.html`
- 最后同步 `visual-spec.md` 和 handoff 文档

## 验证是否符合系统

- 是否符合后台信息架构
- 是否表格、筛选、抽屉、表单状态完整
- 是否卡片化但不过度装饰
- 是否各端布局真正重构
- 是否深色模式仍可读

## 后续新增系统

若需按具体业务域扩展，请在 `design/` 下新增独立 slug 目录，不覆盖当前基线系统。

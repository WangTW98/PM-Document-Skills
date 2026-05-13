# Usage

`DESIGN.md` 是本设计系统的唯一 canonical source。`tokens.json` 提供结构化 token，`visual-spec.md` 提供完整中文设计规范，`preview.html` 用于肉眼校验风格是否跑偏。

## 如何使用

1. 先读 `DESIGN.md`，拿到语义 token 与硬约束。
2. 需要程序化消费时读取 `tokens.json`，不要自己重新命名 token。
3. 需要判断风格是否正确时打开 `preview.html`。
4. 需要完整的跨端设计规则、组件细则和平台专项约束时查看 `visual-spec.md`。

## Codex Usage

可向 Codex 提示：

```text
请读取 design/ios-26-liquid-glass-cross-platform/DESIGN.md 作为唯一设计规范源，
同时参考 tokens.json 和 visual-spec.md。
基于该系统生成一个适配 iPhone、H5、iPad、PC Web 的页面或组件，
严格使用已有 token，不要新造颜色、圆角、阴影、字号与断点。
完成后对照 preview.html 检查是否偏离 Liquid Glass 视觉基调。
```

## Gemini Usage

可向 Gemini 提示：

```text
Read design/ios-26-liquid-glass-cross-platform/DESIGN.md as the canonical design source.
Use tokens.json for structured tokens, visual-spec.md for expanded rationale, and preview.html for visual QA.
Generate UI that preserves the existing token names, responsive rules, accessibility constraints,
and Reduce Transparency / Reduce Motion fallbacks.
```

## Figma Remote MCP 使用

- 先把 `tokens.json` 映射为颜色、字体、间距、圆角、阴影变量。
- 按 `visual-spec.md` 的文件结构创建 `Foundations / Components / Templates / Platforms` 页面。
- 每个模板至少出四套端形态：iPhone / H5 / iPad / PC Web。

## 安全更新方式

- 先改 `DESIGN.md`
- 再同步 `tokens.json`
- 再同步 `preview.html`
- 如果组件行为变化，再更新 `visual-spec.md` 与 handoff 文档

## 验证 UI 是否符合规范

- 是否所有样式值都来自 token
- 是否区分了四端布局策略
- 是否玻璃层只用于合理层级
- 是否具备浅/深色与无障碍降级
- 是否存在完整状态：default / hover / focus / disabled / error

## 后续新增设计系统

后续若要新增另一套风格，不要覆盖当前目录。请在 `design/` 下新建新的 slug 目录，与当前系统并存。

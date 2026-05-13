# iOS 26 Liquid Glass Cross-Platform Design System

- 输出目录: `design/ios-26-liquid-glass-cross-platform/`
- 设计目标: 创建一套可同时服务于 iPhone App、H5 Mobile Web、iPad / Tablet、PC Web 的 Apple 感响应式设计规范，强调高级、简洁、克制、通透和内容优先。
- 输入来源: 用户提供的风格描述与约束清单；本次未附带品牌资产、现有 UI 截图或字体授权文件。

## 已确认的设计决策

- 视觉基调采用浅/深双模式的 `Liquid Glass` 语义体系，而不是单纯的拟物玻璃效果。
- Mobile First，但各端不共用单一布局放大策略。
- 玻璃材质只用于导航、浮层、工具栏、关键卡片和分层容器，不覆盖高密度阅读主体。
- 所有半透明层都有文字可读性兜底方案，包括提高底板不透明度、启用边框高光、切换至实底模式。
- 必须支持 `Reduce Transparency` 与 `Reduce Motion` 降级。

## 假设与开放点

- 默认品牌中性色倾向冷白、石墨、雾蓝，不引入强品牌色主导系统。
- 默认优先使用系统字体栈；如果产品后续有企业品牌字体，可在不破坏字重与字阶的前提下替换展示字体。
- 当前规范适合金融、工具、企业服务、内容效率类产品；娱乐型产品需要额外放宽动效与色彩强度。

## 文件地图

- `DESIGN.md`: 规范的 canonical source，含 YAML tokens 与 AI 使用规则。
- `visual-spec.md`: 完整中文设计手册，覆盖 20 个章节。
- `tokens.json`: 机器可读 token 镜像。
- `preview.html`: 可直接打开的可视化预览页。
- `usage.md`: 人工与 AI 使用说明。
- `handoff/html-generation-guide.md`: 面向 HTML/前端页面生成的执行指南。
- `handoff/figma-remote-mcp-guide.md`: 面向 Figma 建模与变量映射的执行指南。

## 推荐阅读顺序

1. 人类设计师 / 产品 / QA: `visual-spec.md` -> `preview.html` -> `DESIGN.md`
2. AI 生成代理: `DESIGN.md` -> `tokens.json` -> `handoff/html-generation-guide.md`
3. Figma 执行代理: `DESIGN.md` -> `tokens.json` -> `handoff/figma-remote-mcp-guide.md`

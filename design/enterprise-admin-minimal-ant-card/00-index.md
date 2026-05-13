# Enterprise Admin Minimal Ant Card Design System

- 输出目录: `design/enterprise-admin-minimal-ant-card/`
- 设计目标: 创建一套面向后台管理系统的企业级极简风设计规范，融合 Ant Design 的企业管理体验与卡片化轻量风格，支持 Figma 设计、前端实现、AI 页面生成与产品验收。
- 输入来源: 用户指定风格方向为“企业级极简风 + Ant Design 企业风 + 卡片化轻量风”，未提供具体业务领域、品牌资产、角色权限模型与详细页面清单。

## 已确认的设计决策

- 视觉风格偏企业、理性、清晰、高密度但不压迫。
- 布局骨架以 Header、Sidebar、Breadcrumb、Page Header、Card、Table、Drawer、Modal 为核心。
- 风格强调卡片化承载、轻量阴影、明确信息分区和高可读性，而不是花哨装饰。
- 默认支持浅色与深色模式，并为桌面管理后台优先优化，同时兼容 Tablet、H5 Web 和 Mobile。
- 组件状态与数据型场景优先级高于营销型表现力。

## 假设与开放点

- 默认适用于 SaaS 后台、运营中台、风控/审核后台、CRM/ERP/工单/数据管理类产品。
- 默认目标用户为运营、审核、客服、管理员、业务人员、决策者等高频办公角色。
- 默认品牌策略采用中性蓝灰企业风，保留主品牌色入口，但不过度品牌化。
- 默认页面范围包含: 登录、Dashboard、列表页、详情页、表单页、审批页、设置页、权限页、消息/通知、系统反馈页。

## 文件地图

- `DESIGN.md`: canonical source，含 YAML tokens 与 AI 约束。
- `visual-spec.md`: 中文完整版后台设计规范。
- `tokens.json`: 结构化 token 镜像。
- `preview.html`: 独立预览页。
- `usage.md`: 使用说明。
- `handoff/html-generation-guide.md`: HTML / React / Vue / AI 页面生成指南。
- `handoff/figma-remote-mcp-guide.md`: Figma Remote MCP 建模指南。

## 推荐阅读顺序

1. 人类读者: `visual-spec.md` -> `preview.html` -> `DESIGN.md`
2. AI 页面生成: `DESIGN.md` -> `tokens.json` -> `handoff/html-generation-guide.md`
3. Figma 执行: `DESIGN.md` -> `tokens.json` -> `handoff/figma-remote-mcp-guide.md`

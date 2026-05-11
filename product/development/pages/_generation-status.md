# 页面 Draft 生成状态

## 0. 状态概览

- 来源文件：`product/release/product-overview-release.md`
- 来源章节：`Sitemap 页面生成总表`
- 状态文件：`product/development/pages/_generation-status.md`
- 最近更新时间：2026-05-11
- 总页面数：10
- 已完成：10
- 待生成：0
- 已阻塞：0
- 已跳过：0
- 源表已移除：0

## 1. 生成队列

| 生成顺序 | 页面ID | 父页面ID | 层级 | 页面名称 | 页面级MD文件 | 状态 | 最近处理时间 | PA数量 | PQ数量 | 阻塞原因 / 备注 |
|---:|---|---|---|---|---|---|---|---:|---:|---|
| 10 | U-010 | ROOT | L1 | 登录注册页 | product/development/pages/010-login.md | 已完成 | 2026-05-11 | 2 | 1 |  |
| 20 | U-020 | ROOT | L1 | 首页 | product/development/pages/020-home.md | 已完成 | 2026-05-11 | 1 | 1 |  |
| 30 | U-020-010 | U-020 | L2 | 简历上传页 | product/development/pages/030-upload.md | 已完成 | 2026-05-11 | 1 | 1 |  |
| 40 | U-020-020 | U-020 | L2 | 岗位目标设置页 | product/development/pages/040-target-job.md | 已完成 | 2026-05-11 | 1 | 1 |  |
| 50 | U-020-030 | U-020 | L2 | AI 分析报告页 | product/development/pages/050-ai-report.md | 已完成 | 2026-05-11 | 1 | 1 |  |
| 60 | U-020-040 | U-020 | L2 | 简历编辑器 | product/development/pages/060-editor.md | 已完成 | 2026-05-11 | 1 | 1 |  |
| 70 | U-020-040-010 | U-020-040 | L3 | 导出预览页 | product/development/pages/070-export.md | 已完成 | 2026-05-11 | 1 | 1 |  |
| 80 | U-030 | ROOT | L1 | 个人中心 | product/development/pages/080-profile.md | 已完成 | 2026-05-11 | 1 | 1 |  |
| 90 | U-030-010 | U-030 | L2 | 会员订阅页 | product/development/pages/090-vip-subscription.md | 已完成 | 2026-05-11 | 1 | 1 |  |
| 100 | U-030-020 | U-030 | L2 | 历史简历页 | product/development/pages/100-history.md | 已完成 | 2026-05-11 | 2 | 1 |  |

## 2. 当前处理页

| 字段 | 内容 |
|---|---|
| 页面ID | U-030-020 |
| 页面名称 | 历史简历页 |
| 页面级MD文件 | product/development/pages/100-history.md |
| 状态 | 已完成 |
| 开始时间 | 2026-05-11 22:54 |
| 处理说明 | 成功生成历史简历页，包含列表搜索、编辑/导出入口及删除二次确认逻辑。 |

## 3. 完成记录

| 完成顺序 | 页面ID | 页面名称 | 页面级MD文件 | 完成时间 | 校验结果 | 备注 |
|---:|---|---|---|---|---|---|
| 1 | U-010 | 登录注册页 | product/development/pages/010-login.md | 2026-05-11 | 通过 | 包含埋点事件设计。 |
| 2 | U-020 | 首页 | product/development/pages/020-home.md | 2026-05-11 | 通过 | 包含额度展示。 |
| 3 | U-020-010 | 简历上传页 | product/development/pages/030-upload.md | 2026-05-11 | 通过 | 包含 OCR 逻辑。 |
| 4 | U-020-020 | 岗位目标设置页 | product/development/pages/040-target-job.md | 2026-05-11 | 通过 | 包含岗位词推荐。 |
| 5 | U-020-030 | AI 分析报告页 | product/development/pages/050-ai-report.md | 2026-05-11 | 通过 | 包含雷达图分析。 |
| 6 | U-020-040 | 简历编辑器 | product/development/pages/060-editor.md | 2026-05-11 | 通过 | 包含 AI 对比。 |
| 7 | U-020-040-010 | 导出预览页 | product/development/pages/070-export.md | 2026-05-11 | 通过 | 包含模板选择。 |
| 8 | U-030 | 个人中心 | product/development/pages/080-profile.md | 2026-05-11 | 通过 | 包含退出登录逻辑。 |
| 9 | U-030-010 | 会员订阅页 | product/development/pages/090-vip-subscription.md | 2026-05-11 | 通过 | 包含支付渠道。 |
| 10 | U-030-020 | 历史简历页 | product/development/pages/100-history.md | 2026-05-11 | 通过 | 包含搜索逻辑。 |

## 4. 阻塞记录

| 页面ID | 页面名称 | 页面级MD文件 | 阻塞原因 | 需要用户处理 |
|---|---|---|---|---|

## 5. 运行摘要

- 当前轮次结论：已成功生成所有 10 个页面的 Draft 文档。
- 下一步建议：请用户评审生成的页面文档，重点关注各页面的“页面假设与待确认统一清单”。

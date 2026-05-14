# 页面 Release 生成状态

## 0. 状态概览

- 来源文件：`product/release/product-sitemap-release.md`
- 来源章节：`Sitemap 页面生成总表`
- Draft 页面目录：`product/development/pages`
- Release 页面目录：`product/release/pages`
- 状态文件：`product/release/pages/_generation-status.md`
- 最近更新时间：2026-05-13 21:52
- 总页面数：45
- 已完成：45
- 待生成：0
- 已阻塞：0
- 已跳过：0
- 源表已移除：0

## 1. Release 生成队列

| 生成顺序 | 页面ID | 父页面ID | 层级 | 页面名称 | Draft 页面文件 | Release 页面文件 | 状态 | 最近处理时间 | 阻塞原因 / 备注 |
|---:|---|---|---|---|---|---|---|---|---|
| 10 | U-100 | ROOT | L1 | 登录注册 | product/development/pages/010-user-login.md | product/release/pages/010-user-login/index.md | 已完成 | 2026-05-13 21:28 |  |
| 20 | U-200 | ROOT | L1 | 用户工作台 | product/development/pages/020-user-dashboard.md | product/release/pages/020-user-dashboard/index.md | 已完成 | 2026-05-13 21:30 |  |
| 30 | U-210 | U-200 | L2 | 待办事项列表 | product/development/pages/030-user-todo-list.md | product/release/pages/030-user-todo-list/index.md | 已完成 | 2026-05-13 21:31 |  |
| 40 | U-220 | U-200 | L2 | 进度查询列表 | product/development/pages/040-user-progress-list.md | product/release/pages/040-user-progress-list/index.md | 已完成 | 2026-05-13 21:31 |  |
| 50 | U-300 | ROOT | L1 | 服务目录 | product/development/pages/050-service-catalog.md | product/release/pages/050-service-catalog/index.md | 已完成 | 2026-05-13 21:32 |  |
| 60 | U-310 | U-300 | L2 | 服务商品详情 | product/development/pages/060-service-detail.md | product/release/pages/060-service-detail/index.md | 已完成 | 2026-05-13 21:32 |  |
| 70 | U-320 | U-310 | L2 | 下单确认 | product/development/pages/070-order-checkout.md | product/release/pages/070-order-checkout/index.md | 已完成 | 2026-05-13 21:33 |  |
| 80 | U-330 | U-320 | L2 | 支付收银台 | product/development/pages/080-payment-checkout.md | product/release/pages/080-payment-checkout/index.md | 已完成 | 2026-05-13 21:33 |  |
| 90 | U-400 | ROOT | L1 | 订单中心 | product/development/pages/090-order-center.md | product/release/pages/090-order-center/index.md | 已完成 | 2026-05-13 21:34 |  |
| 100 | U-410 | U-400 | L2 | 订单列表 | product/development/pages/100-order-list.md | product/release/pages/100-order-list/index.md | 已完成 | 2026-05-13 21:34 |  |
| 110 | U-420 | U-400 | L2 | 订单详情 | product/development/pages/110-order-detail.md | product/release/pages/110-order-detail/index.md | 已完成 | 2026-05-13 21:35 |  |
| 120 | U-500 | ROOT | L1 | 我的服务 | product/development/pages/120-service-center.md | product/release/pages/120-service-center/index.md | 已完成 | 2026-05-13 21:35 |  |
| 130 | U-510 | U-500 | L2 | 服务案件列表 | product/development/pages/130-service-case-list.md | product/release/pages/130-service-case-list/index.md | 已完成 | 2026-05-13 21:36 |  |
| 140 | U-520 | U-500 | L2 | 案件详情 | product/development/pages/140-case-detail.md | product/release/pages/140-case-detail/index.md | 已完成 | 2026-05-13 21:36 |  |
| 150 | U-521 | U-520 | L3 | 节点资料提交视图 | product/development/pages/150-node-submit-view.md | product/release/pages/150-node-submit-view/index.md | 已完成 | 2026-05-13 21:37 |  |
| 160 | U-530 | U-500 | L2 | 官方文件下载记录 | product/development/pages/160-file-downloads.md | product/release/pages/160-file-downloads/index.md | 已完成 | 2026-05-13 21:37 |  |
| 170 | U-600 | ROOT | L1 | 消息与个人中心 | product/development/pages/170-user-center.md | product/release/pages/170-user-center/index.md | 已完成 | 2026-05-13 21:37 |  |
| 180 | U-610 | U-600 | L2 | 消息中心 | product/development/pages/180-message-center.md | product/release/pages/180-message-center/index.md | 已完成 | 2026-05-13 21:38 |  |
| 190 | U-620 | U-600 | L2 | 账号设置 | product/development/pages/190-account-settings.md | product/release/pages/190-account-settings/index.md | 已完成 | 2026-05-13 21:38 |  |
| 200 | U-630 | U-600 | L2 | 协议查看页 | product/development/pages/200-agreements.md | product/release/pages/200-agreements/index.md | 已完成 | 2026-05-13 21:39 |  |
| 210 | M-100 | ROOT | L1 | 后台登录 | product/development/pages/210-admin-login.md | product/release/pages/210-admin-login/index.md | 已完成 | 2026-05-13 21:39 |  |
| 220 | M-200 | ROOT | L1 | 后台工作台与规则配置 | product/development/pages/220-admin-dashboard.md | product/release/pages/220-admin-dashboard/index.md | 已完成 | 2026-05-13 21:40 |  |
| 230 | M-210 | M-200 | L2 | 待办/风险规则配置 | product/development/pages/230-risk-rule-config.md | product/release/pages/230-risk-rule-config/index.md | 已完成 | 2026-05-13 21:40 |  |
| 240 | M-220 | M-200 | L2 | 进度字段配置 | product/development/pages/240-progress-field-config.md | product/release/pages/240-progress-field-config/index.md | 已完成 | 2026-05-13 21:41 |  |
| 250 | M-300 | ROOT | L1 | 账号管理 | product/development/pages/250-admin-account-center.md | product/release/pages/250-admin-account-center/index.md | 已完成 | 2026-05-13 21:41 |  |
| 260 | M-310 | M-300 | L2 | 服务人员账号分配 | product/development/pages/260-staff-account-admin.md | product/release/pages/260-staff-account-admin/index.md | 已完成 | 2026-05-13 21:42 |  |
| 270 | M-320 | M-300 | L2 | 用户账号管理 | product/development/pages/270-user-account-admin.md | product/release/pages/270-user-account-admin/index.md | 已完成 | 2026-05-13 21:42 |  |
| 280 | M-400 | ROOT | L1 | 服务与商品中心 | product/development/pages/280-service-product-center.md | product/release/pages/280-service-product-center/index.md | 已完成 | 2026-05-13 21:43 |  |
| 290 | M-410 | M-400 | L2 | 服务目录配置 | product/development/pages/290-service-catalog-admin.md | product/release/pages/290-service-catalog-admin/index.md | 已完成 | 2026-05-13 21:43 |  |
| 300 | M-420 | M-400 | L2 | 服务模板编辑器 | product/development/pages/300-service-template-editor.md | product/release/pages/300-service-template-editor/index.md | 已完成 | 2026-05-13 21:44 |  |
| 310 | M-430 | M-400 | L2 | 商品与SKU编辑 | product/development/pages/310-product-sku-editor.md | product/release/pages/310-product-sku-editor/index.md | 已完成 | 2026-05-13 21:44 |  |
| 320 | M-500 | ROOT | L1 | 订单管理 | product/development/pages/320-order-admin-center.md | product/release/pages/320-order-admin-center/index.md | 已完成 | 2026-05-13 21:45 |  |
| 330 | M-510 | M-500 | L2 | 订单列表 | product/development/pages/330-order-admin-list.md | product/release/pages/330-order-admin-list/index.md | 已完成 | 2026-05-13 21:45 |  |
| 340 | M-520 | M-500 | L2 | 订单详情/转线上 | product/development/pages/340-order-admin-detail.md | product/release/pages/340-order-admin-detail/index.md | 已完成 | 2026-05-13 21:46 |  |
| 345 | M-521 | M-520 | L3 | 退款审核视图 | product/development/pages/345-refund-review-view.md | product/release/pages/345-refund-review-view/index.md | 已完成 | 2026-05-13 21:46 |  |
| 350 | M-600 | ROOT | L1 | 案件管理 | product/development/pages/350-case-admin-center.md | product/release/pages/350-case-admin-center/index.md | 已完成 | 2026-05-13 21:47 |  |
| 360 | M-610 | M-600 | L2 | 案件总览 | product/development/pages/360-case-admin-list.md | product/release/pages/360-case-admin-list/index.md | 已完成 | 2026-05-13 21:47 |  |
| 370 | M-620 | M-600 | L2 | 案件详情维护 | product/development/pages/370-case-admin-detail.md | product/release/pages/370-case-admin-detail/index.md | 已完成 | 2026-05-13 21:48 |  |
| 380 | M-621 | M-620 | L3 | 材料审核视图 | product/development/pages/380-material-review-view.md | product/release/pages/380-material-review-view/index.md | 已完成 | 2026-05-13 21:48 |  |
| 390 | M-622 | M-620 | L3 | 官方文件管理视图 | product/development/pages/390-official-file-view.md | product/release/pages/390-official-file-view/index.md | 已完成 | 2026-05-13 21:49 |  |
| 400 | M-700 | ROOT | L1 | 消息与协议管理 | product/development/pages/400-ops-center.md | product/release/pages/400-ops-center/index.md | 已完成 | 2026-05-13 21:50 |  |
| 410 | M-710 | M-700 | L2 | 消息模板与发送配置 | product/development/pages/410-message-template-config.md | product/release/pages/410-message-template-config/index.md | 已完成 | 2026-05-13 21:51 |  |
| 420 | M-720 | M-700 | L2 | 协议管理 | product/development/pages/420-agreement-admin.md | product/release/pages/420-agreement-admin/index.md | 已完成 | 2026-05-13 21:51 |  |
| 430 | M-800 | ROOT | L1 | 数据统计 | product/development/pages/430-analytics-center.md | product/release/pages/430-analytics-center/index.md | 已完成 | 2026-05-13 21:51 |  |
| 440 | M-810 | M-800 | L2 | 销售与服务统计 | product/development/pages/440-sales-analytics.md | product/release/pages/440-sales-analytics/index.md | 已完成 | 2026-05-13 21:52 |  |

## 2. 当前处理页

| 字段 | 内容 |
|---|---|
| 页面ID |  |
| 页面名称 |  |
| Draft 页面文件 |  |
| Release 页面文件 |  |
| 状态 |  |
| 开始时间 |  |
| 处理说明 |  |

## 3. 完成记录

| 完成顺序 | 页面ID | 页面名称 | Release 页面文件 | 完成时间 | 校验结果 | 备注 |
|---:|---|---|---|---|---|---|
| 1 | U-100 | 登录注册 | product/release/pages/010-user-login/index.md | 2026-05-13 21:28 | 通过 |  |
| 2 | U-200 | 用户工作台 | product/release/pages/020-user-dashboard/index.md | 2026-05-13 21:30 | 通过 |  |
| 3 | U-210 | 待办事项列表 | product/release/pages/030-user-todo-list/index.md | 2026-05-13 21:31 | 通过 |  |
| 4 | U-220 | 进度查询列表 | product/release/pages/040-user-progress-list/index.md | 2026-05-13 21:31 | 通过 |  |
| 5 | U-300 | 服务目录 | product/release/pages/050-service-catalog/index.md | 2026-05-13 21:32 | 通过 |  |
| 6 | U-310 | 服务商品详情 | product/release/pages/060-service-detail/index.md | 2026-05-13 21:32 | 通过 |  |
| 7 | U-320 | 下单确认 | product/release/pages/070-order-checkout/index.md | 2026-05-13 21:33 | 通过 |  |
| 8 | U-330 | 支付收银台 | product/release/pages/080-payment-checkout/index.md | 2026-05-13 21:33 | 通过 |  |
| 9 | U-400 | 订单中心 | product/release/pages/090-order-center/index.md | 2026-05-13 21:34 | 通过 |  |
| 10 | U-410 | 订单列表 | product/release/pages/100-order-list/index.md | 2026-05-13 21:34 | 通过 |  |
| 11 | U-420 | 订单详情 | product/release/pages/110-order-detail/index.md | 2026-05-13 21:35 | 通过 |  |
| 12 | U-500 | 我的服务 | product/release/pages/120-service-center/index.md | 2026-05-13 21:35 | 通过 |  |
| 13 | U-510 | 服务案件列表 | product/release/pages/130-service-case-list/index.md | 2026-05-13 21:36 | 通过 |  |
| 14 | U-520 | 案件详情 | product/release/pages/140-case-detail/index.md | 2026-05-13 21:36 | 通过 |  |
| 15 | U-521 | 节点资料提交视图 | product/release/pages/150-node-submit-view/index.md | 2026-05-13 21:37 | 通过 |  |
| 16 | U-530 | 官方文件下载记录 | product/release/pages/160-file-downloads/index.md | 2026-05-13 21:37 | 通过 |  |
| 17 | U-600 | 消息与个人中心 | product/release/pages/170-user-center/index.md | 2026-05-13 21:37 | 通过 |  |
| 18 | U-610 | 消息中心 | product/release/pages/180-message-center/index.md | 2026-05-13 21:38 | 通过 |  |
| 19 | U-620 | 账号设置 | product/release/pages/190-account-settings/index.md | 2026-05-13 21:38 | 通过 |  |
| 20 | U-630 | 协议查看页 | product/release/pages/200-agreements/index.md | 2026-05-13 21:39 | 通过 |  |
| 21 | M-100 | 后台登录 | product/release/pages/210-admin-login/index.md | 2026-05-13 21:39 | 通过 |  |
| 22 | M-200 | 后台工作台与规则配置 | product/release/pages/220-admin-dashboard/index.md | 2026-05-13 21:40 | 通过 |  |
| 23 | M-210 | 待办/风险规则配置 | product/release/pages/230-risk-rule-config/index.md | 2026-05-13 21:40 | 通过 |  |
| 24 | M-220 | 进度字段配置 | product/release/pages/240-progress-field-config/index.md | 2026-05-13 21:41 | 通过 |  |
| 25 | M-300 | 账号管理 | product/release/pages/250-admin-account-center/index.md | 2026-05-13 21:41 | 通过 |  |
| 26 | M-310 | 服务人员账号分配 | product/release/pages/260-staff-account-admin/index.md | 2026-05-13 21:42 | 通过 |  |
| 27 | M-320 | 用户账号管理 | product/release/pages/270-user-account-admin/index.md | 2026-05-13 21:42 | 通过 |  |
| 28 | M-400 | 服务与商品中心 | product/release/pages/280-service-product-center/index.md | 2026-05-13 21:43 | 通过 |  |
| 29 | M-410 | 服务目录配置 | product/release/pages/290-service-catalog-admin/index.md | 2026-05-13 21:43 | 通过 |  |
| 30 | M-420 | 服务模板编辑器 | product/release/pages/300-service-template-editor/index.md | 2026-05-13 21:44 | 通过 |  |
| 31 | M-430 | 商品与SKU编辑 | product/release/pages/310-product-sku-editor/index.md | 2026-05-13 21:44 | 通过 |  |
| 32 | M-500 | 订单管理 | product/release/pages/320-order-admin-center/index.md | 2026-05-13 21:45 | 通过 |  |
| 33 | M-510 | 订单列表 | product/release/pages/330-order-admin-list/index.md | 2026-05-13 21:45 | 通过 |  |
| 34 | M-520 | 订单详情/转线上 | product/release/pages/340-order-admin-detail/index.md | 2026-05-13 21:46 | 通过 |  |
| 35 | M-521 | 退款审核视图 | product/release/pages/345-refund-review-view/index.md | 2026-05-13 21:46 | 通过 |  |
| 36 | M-600 | 案件管理 | product/release/pages/350-case-admin-center/index.md | 2026-05-13 21:47 | 通过 |  |
| 37 | M-610 | 案件总览 | product/release/pages/360-case-admin-list/index.md | 2026-05-13 21:47 | 通过 |  |
| 38 | M-620 | 案件详情维护 | product/release/pages/370-case-admin-detail/index.md | 2026-05-13 21:48 | 通过 |  |
| 39 | M-621 | 材料审核视图 | product/release/pages/380-material-review-view/index.md | 2026-05-13 21:48 | 通过 |  |
| 40 | M-622 | 官方文件管理视图 | product/release/pages/390-official-file-view/index.md | 2026-05-13 21:49 | 通过 |  |
| 41 | M-700 | 消息与协议管理 | product/release/pages/400-ops-center/index.md | 2026-05-13 21:50 | 通过 |  |
| 42 | M-710 | 消息模板与发送配置 | product/release/pages/410-message-template-config/index.md | 2026-05-13 21:51 | 通过 |  |
| 43 | M-720 | 协议管理 | product/release/pages/420-agreement-admin/index.md | 2026-05-13 21:51 | 通过 |  |
| 44 | M-800 | 数据统计 | product/release/pages/430-analytics-center/index.md | 2026-05-13 21:51 | 通过 |  |
| 45 | M-810 | 销售与服务统计 | product/release/pages/440-sales-analytics/index.md | 2026-05-13 21:52 | 通过 |  |

## 4. 阻塞记录

| 页面ID | 页面名称 | Draft 页面文件 | Release 页面文件 | 阻塞原因 | 需要用户处理 |
|---|---|---|---|---|---|

## 5. 运行摘要

- 当前轮次结论：所有页面 (45/45) 已全部生成 Release 版本。
- 下一步建议：任务圆满完成。

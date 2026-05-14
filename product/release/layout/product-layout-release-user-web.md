# Product Layout Release
 
 ## 0. 文档状态
 
 | 字段 | 内容 |
 |---|---|
 | 文档版本 | Release |
 | 生成日期 | 2026-05-14 |
 | 来源文件 | `product/release/product-sitemap-release.md` |
 | 来源章节 | `Sitemap 页面生成总表` |
 | 确认草稿 | `product/development/layout/product-layout-draft-user-web-v3.md` |
 | Layout Key | `user-web` |
 | 适用 Surface | 用户端 |
 | 适用端形态 | `desktop-web` |
 | 覆盖页面ID / 页面级MD文件范围 | `U-100` 至 `U-630`；`product/development/pages/010-user-login.md` 至 `product/development/pages/200-agreements.md` |
 | 适用范围 | 双域名同构 PC Web 用户站点布局规范 |
 
 ## 1. 产品布局总览
 
 | 项目 | 内容 | 备注 |
 |---|---|---|
 | 产品类型 | 双域名同构 PC Web 用户站点 | |
 | 应用 Surface | 用户端 | |
 | 主 Layout 形态 | 左侧导航 + 顶栏 + 主内容工作区 + 悬浮客服入口 | 侧边栏支持折叠 (Mini Mode) |
 | 全局导航模型 | 左侧一级导航切换 Root 业务模块，顶栏承载辅助功能（面包屑、消息/账号）及全局操作 | |
 | 页面层级模型 | Root 页面通过侧边栏直达；详情/流程页通过 Breadcrumb 返回来源；节点资料提交流程使用独立页面承载 | |
 | 响应式 / 端形态策略 | 当前仅做 desktop-web；左侧导航在窄屏幕或用户主动操作时折叠为图标模式 | |
 | 品牌区分策略 | 双域名仅通过品牌视觉、Logo 与主题资源区分，不额外显示平台标签或切换标识 | |
 | 全局状态容器 | Shell 级 loading/error/permission；页面级 empty、timeout、refund-processing | |
 
 ## 2. Layout 架构图
 
 ```mermaid
 flowchart TD
   ROOT["Layout Family: user-web"]
   ROOT --> SHELL["User Web Side-Nav + Top-Nav Shell"]
   SHELL --> SIDEBAR["Side Navigation (一级业务入口)"]
   SHELL --> TOPNAV["Top Navigation (面包屑/通知/账号)"]
   
   SIDEBAR --> U200["用户工作台"]
   SIDEBAR --> U300["服务目录"]
   SIDEBAR --> U400["订单中心"]
   SIDEBAR --> U500["我的服务"]
   SIDEBAR --> U600["消息与个人中心"]
 
   U200 --> U210["待办事项列表"]
   U200 --> U220["进度查询列表"]
   
   U300 --> U310["服务商品详情"]
   U310 --> U320["下单确认"]
   U320 --> U330["支付收银台"]
   
   U400 --> U410["订单列表"]
   U400 --> U420["订单详情"]
   
   U500 --> U510["服务案件列表"]
   U500 --> U520["案件详情"]
   U520 --> U521["节点资料提交页"]
   U500 --> U530["官方文件下载记录"]
   
   U600 --> U610["消息中心"]
   U600 --> U620["账号设置"]
   U600 --> U630["协议查看页"]
 ```
 
 ## 3. Surface 与 Shell 定义
 
 | Surface ID | Surface 名称 | 适用角色 | Shell 类型 | 全局区域 | 根页面入口 | 子页面呈现 | 例外页面 |
 |---|---|---|---|---|---|---|---|
 | SF-001 | 用户端 PC Web（双域名同构） | 游客、客户用户 | sidebar + top-nav | side-nav / top-nav / content-header / main-scroll / floating-service-entry / toast-layer / legal-footer | 登录注册、用户工作台、服务目录、订单中心、我的服务、消息与个人中心 | nested-route / push | 登录注册、支付收银台、协议查看页、节点资料提交页 |
 
 ## 4. 全局 Shell 区域规范
 
 | Shell 区域ID | 区域名称 | 所属 Surface | 显示规则 | 内容槽位 | 尺寸 / 安全区 | 滚动关系 | 层级关系 | 状态变化 |
 |---|---|---|---|---|---|---|---|---|
 | SHELL-006 | Side Navigation | SF-001 | 登录后显示；登录前隐藏 | logo / main-menu / collapse-trigger | 展开宽 200px / 折叠宽 64px | fixed | 高于主内容 | default / collapsed |
 | SHELL-001 | Top Navigation | SF-001 | 全局显示 | breadcrumb / search / message-entry / account-entry | 固定高 64px | fixed | 与侧边栏齐平，高于主内容 | default / sticky |
 | SHELL-002 | Main Content Container | SF-001 | 所有业务页面显示 | page-title / filter-bar / body / side-summary | 最大宽 1280px | main-scroll | 位于顶栏下方、侧边栏右侧 | loading / empty / error / permission |
 | SHELL-003 | Floating Service Entry | SF-001 | 登录后默认显示 | customer-service-button / unread-badge | 右下悬浮 | fixed | 高于内容区 | hidden / expanded / unread |
 | SHELL-004 | Modal Layer | SF-001 | 交易确认、二次确认、轻量说明弹窗显示 | modal-header / body / fixed-actions | 中央弹层 | page-scroll-locked | 最高交互层 | open / dirty / submitting / success |
 | SHELL-005 | Global Status Banner | SF-001 | 支付超时、退款处理中、全局通知时显示 | status-text / action-link | 顶栏下横幅 | fixed-with-shell | 高于内容区 | info / warning / success |
 
 ## 5. 页面模板库
 
 | 模板ID | 模板名称 | 适用页面类型 | 必备区域 | 可选区域 | 默认父子层级 | 典型状态 | 响应式规则 |
 |---|---|---|---|---|---|---|---|
 | TPL-001 | 用户端列表页模板 | list / index | shell + page-header + filter + list | stats-summary / empty-state | Surface > Shell > Page > List | loading / empty / filter-empty | 侧边栏折叠以腾出列表宽度 |
 | TPL-002 | 用户端详情页模板 | detail | shell + title + detail-body + action-area | side-summary / history / related-files | list > detail | loading / not-found / permission | 双栏详情 |
 | TPL-003 | 用户端交易表单模板 | create / payment / settings | shell + step/title + form/body + fixed-actions | fee-summary / protocol-check / result-panel | detail > flow | validation / submitting / timeout / success | 主内容区居中收窄 |
 | TPL-004 | 用户端中心页模板 | dashboard / profile-home | shell + summary + local-nav + body | quick-links / counters | shell > center > child | loading / empty | 头部摘要固定，内容区滚动 |
 | TPL-005 | 阅读内容模板 | legal / content | shell + article-header + article-body | toc / version-note | center > content | loading / no-content | 阅读容器收窄 |
 | TPL-006 | 节点资料提交页模板 | upload / case-step | shell + breadcrumb + step-summary + form/body + fixed-actions | attachment-preview / reject-reason / service-tip | detail > dedicated-page | loading / reject / resubmit / success | 主内容单列，右侧可选摘要吸附 |
 
 ## 6. Sitemap 到 Layout 映射总表
 
 | 生成顺序 | 页面ID | 父页面ID | 层级 | 页面名称 | 页面级MD文件 | Surface ID | Shell 类型 | 页面模板ID | 导航位置 | 子页面呈现方式 | 全局区域继承 | 响应式规则 | 例外说明 |
 |---:|---|---|---|---|---|---|---|---|---|---|---|---|---|
 | 10 | U-100 | ROOT | L1 | 登录注册 | product/development/pages/010-user-login.md | SF-001 | auth-only | TPL-003 | hidden | new-page | SHELL-001 minimal / SHELL-002 none | centered auth | 隐藏左侧侧边栏 |
 | 20 | U-200 | ROOT | L1 | 用户工作台 | product/development/pages/020-user-dashboard.md | SF-001 | sidebar + top-nav | TPL-004 | sidebar:工作台 | nested-route | SHELL-006 / SHELL-001 / SHELL-002 | 宽屏摘要+入口卡片 | 登录后默认首页 |
 | 30 | U-210 | U-200 | L2 | 待办事项列表 | product/development/pages/030-user-todo-list.md | SF-001 | sidebar + top-nav | TPL-001 | sidebar:工作台 / local-tab:待办 | nested-route | SHELL-006 / SHELL-001 / SHELL-002 | 筛选折行 | 父页内部切换 |
 | 40 | U-220 | U-200 | L2 | 进度查询列表 | product/development/pages/040-user-progress-list.md | SF-001 | sidebar + top-nav | TPL-001 | sidebar:工作台 / local-tab:进度 | nested-route | SHELL-006 / SHELL-001 / SHELL-002 | 列表横向滚动 | 父页内部切换 |
 | 50 | U-300 | ROOT | L1 | 服务目录 | product/development/pages/050-service-catalog.md | SF-001 | sidebar + top-nav | TPL-001 | sidebar:服务目录 | nested-route | SHELL-006 / SHELL-001 / SHELL-002 | 类目网格 | 双域名仅品牌差异 |
 | 60 | U-310 | U-300 | L2 | 服务商品详情 | product/development/pages/060-service-detail.md | SF-001 | sidebar + top-nav | TPL-002 | sidebar:服务目录 | push | SHELL-006 / SHELL-001 / SHELL-002 | 双栏详情 | 独立详情路由 |
 | 70 | U-320 | U-310 | L2 | 下单确认 | product/development/pages/070-order-checkout.md | SF-001 | sidebar + top-nav | TPL-003 | hidden-from-main-highlight | push | SHELL-006 / SHELL-001 / SHELL-002 | 双栏表单+费用摘要 | 交易链路弱化主导航 |
 | 80 | U-330 | U-320 | L2 | 支付收银台 | product/development/pages/080-payment-checkout.md | SF-001 | sidebar + top-nav | TPL-003 | hidden-from-main-highlight | push | SHELL-006 / SHELL-001 / SHELL-002 | 结果优先布局 | 含超时关闭状态 |
 | 90 | U-400 | ROOT | L1 | 订单中心 | product/development/pages/090-order-center.md | SF-001 | sidebar + top-nav | TPL-004 | sidebar:订单中心 | nested-route | SHELL-006 / SHELL-001 / SHELL-002 | 统计+子页入口 | Root 页面 |
 | 100 | U-410 | U-400 | L2 | 订单列表 | product/development/pages/100-order-list.md | SF-001 | sidebar + top-nav | TPL-001 | sidebar:订单中心 / local-tab:列表 | nested-route | SHELL-006 / SHELL-001 / SHELL-002 | 状态标签横向 | 父页内部切换 |
 | 110 | U-420 | U-400 | L2 | 订单详情 | product/development/pages/110-order-detail.md | SF-001 | sidebar + top-nav | TPL-002 | sidebar:订单中心 | push | SHELL-006 / SHELL-001 / SHELL-002 | 主副栏详情 | 客服为悬浮入口，不占正文 |
 | 120 | U-500 | ROOT | L1 | 我的服务 | product/development/pages/120-service-center.md | SF-001 | sidebar + top-nav | TPL-004 | sidebar:我的服务 | nested-route | SHELL-006 / SHELL-001 / SHELL-002 | 中心入口页 | Root 页面 |
 | 130 | U-510 | U-500 | L2 | 服务案件列表 | product/development/pages/130-service-case-list.md | SF-001 | sidebar + top-nav | TPL-001 | sidebar:我的服务 / local-tab:案件 | nested-route | SHELL-006 / SHELL-001 / SHELL-002 | 状态筛选强化 | 父页内部切换 |
 | 140 | U-520 | U-500 | L2 | 案件详情 | product/development/pages/140-case-detail.md | SF-001 | sidebar + top-nav | TPL-002 | sidebar:我的服务 | push | SHELL-006 / SHELL-001 / SHELL-002 | 时间轴+摘要双栏 | 内容深度较高 |
 | 150 | U-521 | U-520 | L3 | 节点资料提交视图 | product/development/pages/150-node-submit-view.md | SF-001 | sidebar + top-nav | TPL-006 | hidden | push | SHELL-006 / SHELL-001 / SHELL-002 | 表单区单列，摘要区按需吸附 | 从案件详情进入独立页面 |
 | 160 | U-530 | U-500 | L2 | 官方文件下载记录 | product/development/pages/160-file-downloads.md | SF-001 | sidebar + top-nav | TPL-001 | sidebar:我的服务 / local-tab:文件 | nested-route | SHELL-006 / SHELL-001 / SHELL-002 | 表格可横滑 | 父页内部切换 |
 | 170 | U-600 | ROOT | L1 | 消息与个人中心 | product/development/pages/170-user-center.md | SF-001 | sidebar + top-nav | TPL-004 | sidebar:个人中心 | nested-route | SHELL-006 / SHELL-001 / SHELL-002 | 摘要+子入口 | Root 页面 |
 | 180 | U-610 | U-600 | L2 | 消息中心 | product/development/pages/180-message-center.md | SF-001 | sidebar + top-nav | TPL-001 | sidebar:个人中心 / local-tab:消息 | nested-route | SHELL-006 / SHELL-001 / SHELL-002 | 渠道筛选 | 父页内部切换 |
 | 190 | U-620 | U-600 | L2 | 账号设置 | product/development/pages/190-account-settings.md | SF-001 | sidebar + top-nav | TPL-003 | sidebar:个人中心 / local-tab:设置 | nested-route | SHELL-006 / SHELL-001 / SHELL-002 | 单栏/双栏表单 | 父页内部切换 |
 | 200 | U-630 | U-600 | L2 | 协议查看页 | product/development/pages/200-agreements.md | SF-001 | sidebar + top-nav | TPL-005 | sidebar:个人中心 / local-tab:协议 | push | SHELL-006 / SHELL-001 / SHELL-002 | 阅读模式收窄 | 可从注册页进入 |
 
 ## 7. 导航与路由规则
 
 | 规则ID | 适用范围 | 规则 | 返回 / 关闭行为 | 当前态展示 | 权限影响 |
 |---|---|---|---|---|---|
 | NAV-001 | 用户端 Root 页面 | 一级导航使用左侧侧边栏切换 `工作台 / 服务目录 / 订单中心 / 我的服务 / 个人中心` | 切换后保留登录态 | 侧边栏高亮当前入口 | 游客仅可见服务目录与登录入口，侧边栏可能隐藏 |
 | NAV-002 | 用户端详情/交易流 | 从列表进入详情、下单、支付采用 push 路由，通过 Breadcrumb 返回 | 返回回到来源页及滚动位置 | 侧边栏保持父级入口高亮 | 未登录需先跳登录 |
 | NAV-003 | 用户端案件 L3 提交流程 | 节点资料提交使用独立页面承载，面包屑与侧边栏共存 | 返回回到案件详情当前位置 | 侧边栏保持 `我的服务` 高亮 | 无权限时显示 permission 容器 |
 
 ## 8. 全局状态与边界 Layout
 
 | 状态ID | 适用 Surface / 模板 | 状态类型 | Layout 表现 | 文案/媒体槽位 | 可恢复入口 |
 |---|---|---|---|---|---|
 | GL-STATE-001 | SF-001 / 全模板 | loading | 保留侧边栏与顶栏，内容区骨架屏 | page-title / body-skeleton | 自动恢复 |
 | GL-STATE-002 | TPL-001 / TPL-004 | empty | 空状态插图 + 主 CTA | title / desc / primary-action | 清空筛选 / 去浏览 |
 | GL-STATE-003 | SF-001 / 全模板 | error | 内容区错误卡片，保留 Shell | error-title / retry | retry / back |
 | GL-STATE-004 | SF-001 / TPL-002 | permission | 详情主体替换为无权限容器 | permission-title / help-link | 返回列表 |
 | GL-STATE-005 | TPL-003 | payment-timeout | 订单摘要保留，主区域显示超时关闭状态 | timeout-title / order-summary | 返回待支付订单列表 |
 | GL-STATE-006 | TPL-002 / TPL-006 | refund-processing | 详情页或提交页顶部状态带 + 时间线 | refund-status / audit-note | 返回订单列表或案件详情 |
 | GL-STATE-007 | SF-001 | offline | 顶栏下方弱提示 banner | offline-banner | 重试 / 刷新 |
 
 ## 9. 角色与权限对 Layout 的影响
 
 | 角色 | 影响区域 | 显示 / 隐藏 / 禁用规则 | 替代 Layout |
 |---|---|---|---|
 | 游客 | 侧边栏、业务模块 | 隐藏左侧侧边栏业务入口；显示顶栏登录入口 | auth-only / landing |
 | 客户用户 | 用户端全局 | 显示完整左侧一级导航；侧边栏支持折叠状态保存 | 无 |
 
 ## 10. 下游 Skill 使用规则
 
 | 下游 Skill | 必须读取 | 使用方式 | 必须引用的 Layout 信息 |
 |---|---|---|---|
 | product-page-draft | 匹配的 product-layout-release*.md | 先按 `user-web` 匹配页面 shell/template，再生成元素/状态/action/API | Layout Key / Surface ID / Shell / 模板ID / 导航位置 / 响应式规则 |
 | product-page-mock-draft | 匹配的 product-layout-release*.md | 补齐侧边栏、顶栏、状态容器文案 | Layout Key / 全局区域 / 导航标签 / 页面标题 / 状态容器 |
 | product-page-design-release | 匹配的 product-layout-release*.md | 生成侧边栏、顶栏壳、内容容器、模态层 | Layout Key / Shell 区域 / 页面模板 / 父子层级 / scroll/fixed 关系 |

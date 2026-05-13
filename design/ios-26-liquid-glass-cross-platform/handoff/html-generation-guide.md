# HTML Generation Guide

## 必读文件

1. `design/ios-26-liquid-glass-cross-platform/DESIGN.md`
2. `design/ios-26-liquid-glass-cross-platform/tokens.json`
3. `design/ios-26-liquid-glass-cross-platform/visual-spec.md`

## Token 应用顺序

1. 先应用颜色模式与语义层级
2. 再应用字体与间距
3. 再应用圆角、边框、阴影、blur
4. 最后应用动效与平台专项规则

## 默认组件规则

- 主按钮: `44px` 高，`pill` 圆角
- 输入框: `48px` 高，focus 有边框和 ring
- 导航: Mobile / H5 使用顶部浮层，Desktop 使用 Header + Sidebar
- 卡片: 信息卡优先实底，浮层卡优先玻璃底

## 响应式规则

- `0-767px`: 单列、底部操作优先
- `768-1199px`: 支持双栏、Sidebar、Popover
- `1200px+`: 支持 Header、Sidebar、Breadcrumb、Filter Bar、Table

## 可访问性检查

- 正文字色对比度 >= `4.5:1`
- 热区最小 `44x44px`
- Focus 必须可见
- 错误态要有文案
- 开启 Reduce Transparency 时不得继续输出 `backdrop-filter`

## 漂移检查

完成前逐项确认：

- 是否新造了 token 值
- 是否把 PC 做成放大版手机
- 是否过度使用玻璃背景
- 是否漏掉 hover/focus/error/disabled 状态
- 是否遗漏 H5 安全区与软键盘处理

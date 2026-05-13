# HTML Generation Guide

## 必读文件

1. `design/material-3-cross-platform/DESIGN.md`
2. `design/material-3-cross-platform/tokens.json`
3. `design/material-3-cross-platform/visual-spec.md`

## Token 应用顺序

1. 先确定 color mode
2. 应用 color roles 和 surface 系统
3. 应用 typography 和 spacing
4. 应用 radius 和 elevation
5. 最后应用状态与动效

## 默认组件规则

- Filled Button 为默认主按钮
- 输入框默认高度 `56px`
- 卡片优先使用 `surface.container` 系列
- Navigation 根据端形态切换为 bottom nav / rail / sidebar

## 响应式规则

- Mobile: 单列、底部操作优先
- Tablet: 支持 rail、双栏、split view
- Desktop: 支持 sidebar、table、detail pane

## 可访问性检查

- 正文对比度 >= `4.5:1`
- focus 可见
- 热区足够
- 错误态完整
- 深色模式独立检查

## 漂移检查

- 不新造 token
- 不引入未定义玻璃或品牌渐变
- 不把桌面端做成放大手机
- 不遗漏 hover/focus/pressed/disabled/error

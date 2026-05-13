# HTML Generation Guide

## 必读文件

1. `design/enterprise-admin-minimal-ant-card/DESIGN.md`
2. `design/enterprise-admin-minimal-ant-card/tokens.json`
3. `design/enterprise-admin-minimal-ant-card/visual-spec.md`

## Token 应用顺序

1. 先确定 color mode
2. 应用 layout / container / text / border
3. 应用 button / input / table / nav 等组件 token
4. 最后应用状态与交互反馈

## 默认组件规则

- Button 默认 `36px`
- Input 默认 `36px`
- Table 默认行高 `48px`
- Sidebar `240px`
- Header `56px`

## 响应式规则

- Desktop 优先输出完整后台信息架构
- Tablet 输出折叠 sidebar + drawer
- Mobile / H5 输出卡片化任务流与轻表单

## 可访问性检查

- 正文对比度 >= `4.5:1`
- 焦点态可见
- 状态不只靠颜色
- 移动端热区 >= `44px`

## 漂移检查

- 不引入营销型大视觉
- 不引入玻璃拟物风
- 不把所有内容铺成一个长白板
- 不遗漏表格、筛选、抽屉、状态系统

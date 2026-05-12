# HTML 页面生成指南 (HTML Generation Guide)

下游负责页面构建和代码生成的 AI Agent 应遵循本指南，以确保前端产物严格遵守“现代化的管理后台”视觉设计规范。

## 1. 必读输入文件
在开始生成任何页面前，Agent 必须读取并分析以下文件作为工作上下文：
- `design/modern-admin-dashboard/DESIGN.md`
- `design/modern-admin-dashboard/visual-spec.md`
- 建议扫读 `design/modern-admin-dashboard/preview.html` 的源码以学习基础组件如何组合。

## 2. 映射 Token 到代码
不论目标代码是原生 HTML+CSS、React+Tailwind 还是 Vue，都必须维持 Token 的语义一致性。

**通用 CSS 变量映射示例：**
Agent 应在页面的 `:root` 中或共享样式表中注入以下映射：
```css
:root {
  --color-bg-canvas: #f8fafc;
  --color-bg-surface: #ffffff;
  --color-text-primary: #0f172a;
  --color-text-secondary: #475569;
  --color-brand-primary: #2563eb;
  --color-border-default: #e2e8f0;
  
  --space-4: 16px;
  --space-6: 24px;
  
  --radius-md: 6px;
  --radius-lg: 8px;
}
```

## 3. 组件级默认构造

**Button 按钮**
- **Primary**: 背景必须为 `--color-brand-primary`。文字白色。高度 `40px`。圆角 `--radius-md`。
- **Focus 状态**: 不能只是色值变深。必须包含 `box-shadow: 0 0 0 2px #fff, 0 0 0 4px var(--color-brand-primary);`。

**Card 卡片**
- 结构：`--color-bg-surface`，`1px solid --color-border-default`，圆角 `--radius-lg`。
- **绝对不要默认加阴影**（Box-shadow）。

**Table 数据表格**
- 表头 (th)：字体较小 (`12px` 或 `14px`)，颜色较浅 (`--color-text-tertiary` 或 `--color-text-secondary`)，文字粗细 `500` (Medium)。
- 分隔线：行与行之间必须有 `1px solid --color-border-default`，但不一定要有纵向边框。

## 4. 响应式布局应用规则 (Responsive Layout)
必须实施 Mobile First 或包含充分媒体查询的代码逻辑。

1. **容器边界**: 桌面端最大宽度被限制在 `1440px`。超宽屏幕下，容器应当 `margin: 0 auto` 居中。
2. **移动端折叠**:
   - `sm (<640px)`: 移除侧边栏的桌面形态；数据表格强制出现横向滚动条或转为卡片流（Card Stream）。
3. **间距塌陷**: 桌面端的模块间距如果是 `24px (--space-6)`，在移动端可以缩小为 `16px (--space-4)` 以节约屏幕空间。

## 5. 无障碍审查 (Accessibility Check)
生成代码时，Agent 必须在脑海中运行或实施以下校验：
- 是否所有的表单都有 `<label>` 绑定（通过 `for` 或嵌套）？
- 文本颜色和它的背景色叠加是否符合对比度要求？（特别是不要在灰色底色上使用浅灰色字体）。
- 用户使用 `Tab` 键能否清晰地看到目前处于哪个组件上？（Focus Ring 规则）。

## 6. 防偏离自检 (Anti-Drift Checklist)
在 Agent 输出最终代码前，检查：
- [ ] 页面的大背景是否为 `#f8fafc` 而非纯白？
- [ ] 卡片内部底色是否为 `#ffffff` 从而形成对比？
- [ ] 按钮是否满足最小高度 40px？
- [ ] 是否存在任何未定义在 `DESIGN.md` 中的“野鸡色值”（Wild hex codes）？如果有，修正它们。

# Grid 布局（二维网格）

> 目标：掌握二维布局（行+列）能力，能构建复杂网格、瀑布式面板、仪表盘、应用布局等。

## 何时选择 Grid

- 二维布局：同时控制行与列
- 明确的网格轨道（track）与区域（area）规划
- 需要对齐多个组件在同一网格上的位置与尺寸

对比：Flexbox 聚焦一维流式分配，Grid 擅长模板化的二维网格。

## 核心概念

- 网格容器与轨道：`grid-template-rows`、`grid-template-columns`
- 轨道单位：`px`、`%`、`fr`、`minmax()`、`auto`
- 自动布局：`auto-fill`、`auto-fit`
- 间距：`gap`、`row-gap`、`column-gap`
- 区域命名：`grid-template-areas`
- 项目放置：`grid-row`、`grid-column`、`grid-area`

## 快速上手

```html
<section class="grid">
  <div class="a">A</div>
  <div class="b">B</div>
  <div class="c">C</div>
  <div class="d">D</div>
  <div class="e">E</div>
</section>
```

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr; /* 三列，比例 1:2:1 */
  grid-template-rows: auto 200px; /* 两行：首行自适应，次行固定 */
  gap: 12px;
}
.a {
  grid-column: 1 / -1;
} /* A 占满整行 */
.b {
  grid-column: 1;
}
.c {
  grid-column: 2;
}
.d {
  grid-column: 3;
}
.e {
  grid-column: 2 / span 2;
} /* 从第2列开始跨两列 */
```

## 响应式网格（自动填充）

```css
.auto-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 16px;
}
```

解释：

- `minmax(220px, 1fr)`：每个单元最小 220px，最大按比例填满
- `auto-fill`：尽可能填充更多列，空槽位仍保留轨道
- `auto-fit`：尽可能填满且折叠空轨道，使列更均匀

## 命名区域（模板式页面）

```css
.layout {
  display: grid;
  grid-template-columns: 200px 1fr;
  grid-template-rows: 64px 1fr 56px;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  gap: 12px;
}
.header {
  grid-area: header;
}
.sidebar {
  grid-area: sidebar;
}
.main {
  grid-area: main;
}
.footer {
  grid-area: footer;
}
```

## 技巧与陷阱

- `fr` 单位易读、强大；与 `minmax()` 组合可兼顾最小宽度
- 优先用 `gap` 控制网格间距
- `grid-auto-rows/columns` 可定义自动生成轨道的大小
- 合理使用 `auto-fit` vs `auto-fill` 以控制空轨道表现
- Grid 不会影响 DOM 顺序的可访问性；谨慎与 `order` 类似的重排（主要在 Flex）

## 练习建议

- 构建一个博客首页：头部/侧栏/文章列表/页脚
- 构建一个仪表盘：多卡片小组件在网格中跨行跨列排布
- 将响应式卡片从 Flex 改写为 Grid，对比差异

---

## 知识拓展

- Masonry 风（密集布局）思路

```css
.masonry {
  columns: 3 320px; /* 多列布局模拟 */
  column-gap: 16px;
}
.masonry > article {
  break-inside: avoid; /* 避免打断 */
  display: block;
  margin: 0 0 16px;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
  padding: 12px;
  background: #fff;
}
```

- Grid 子网格（subgrid，部分浏览器）

```css
.parent {
  display: grid;
  grid-template-columns: 160px 1fr;
  gap: 12px;
}
.child {
  display: grid;
  grid-template-columns: subgrid; /* 继承父网格的列 */
}
```

# Flexbox 布局（弹性盒模型）

> 目标：掌握一维布局（主轴/交叉轴）的核心概念与 API，能快速实现常见页面结构与自适应排布。

## 何时选择 Flexbox

- 一维布局（横向或纵向）的主轴分配与对齐
- 内容驱动布局：元素宽高不固定、需要自动分配剩余空间
- 快速实现居中、等分、两端对齐、换行排布

对比：Grid 更适合二维（行+列）网格；Flexbox 聚焦一维。

## 核心概念

- 主轴（main axis）与交叉轴（cross axis）
- 容器（flex container）与子项（flex item）

容器常用属性：

- display: flex | inline-flex
- flex-direction: row | row-reverse | column | column-reverse
- flex-wrap: nowrap | wrap | wrap-reverse
- justify-content: flex-start | center | space-between | space-around | space-evenly
- align-items: stretch | flex-start | center | flex-end | baseline
- align-content: stretch | flex-start | center | flex-end | space-between | space-around
- gap / column-gap / row-gap

子项常用属性：

- flex-grow: <number>（放大因子，分配剩余空间）
- flex-shrink: <number>（收缩因子，空间不足时收缩）
- flex-basis: <length | auto>（初始尺寸）
- flex: none | [ <flex-grow> <flex-shrink>? <flex-basis>? ]
- align-self: auto | flex-start | center | flex-end | baseline | stretch
- order: <integer>（改变视觉顺序，不改变 DOM 顺序）

推荐速记：

- 等分：`flex: 1`（等价 `flex: 1 1 0`）
- 固定宽度不被挤压：`flex: 0 0 200px`

## 常见布局模式

1. 水平垂直居中

```html
<div class="center">
  <div class="box">A</div>
</div>
```

```css
.center {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 200px;
  background: #f6f8fa;
}
.box {
  width: 80px;
  height: 80px;
  background: #79c0ff;
  color: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
}
```

2. 顶部导航：左右两端对齐，中间自适应

```html
<header class="nav">
  <div class="left">Logo</div>
  <div class="center">Search</div>
  <div class="right">Actions</div>
</header>
```

```css
.nav {
  display: flex;
  align-items: center;
  gap: 12px;
}
.left,
.right {
  flex: 0 0 auto;
}
.center {
  flex: 1;
}
```

3. 等宽卡片自动换行

```html
<ul class="cards">
  <li class="card">1</li>
  <li class="card">2</li>
  <li class="card">3</li>
  <!-- ... -->
</ul>
```

```css
.cards {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}
.card {
  flex: 1 1 220px; /* 最小 220px，空间有余等分扩展 */
  background: #fff;
  border: 1px solid #d0d7de;
  border-radius: 8px;
  min-height: 140px;
}
```

## 实战心法与陷阱

- 优先使用 `gap` 代替子项间距的边距叠加
- `min-width`/`min-height` 可能阻止收缩，必要时显式设定
- 使用 `flex-basis: 0` 让等分更可预期
- 避免用 `order` 实现无障碍关键顺序（影响阅读顺序）

## 练习建议

- 使用 Flex 实现：三列布局（中间自适应）、响应式卡片区、固定侧栏+自适应内容区
- 将传统浮动/定位布局重写为 Flex，比较可读性与弹性

---

## 知识拓展

- 自适应“圣杯布局”（中间自适应，左右固定）

```html
<div class="holy">
  <aside class="left">L</aside>
  <main class="main">M</main>
  <aside class="right">R</aside>
</div>
<style>
  .holy {
    display: flex;
    gap: 12px;
  }
  .left,
  .right {
    flex: 0 0 220px;
    background: #f6f8fa;
  }
  .main {
    flex: 1 1 auto;
    min-width: 0;
  }
</style>
```

- Flex 等高卡片栈（底部对齐按钮）

```css
.stack {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}
.card {
  flex: 1 1 240px;
  display: flex;
  flex-direction: column;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
  padding: 12px;
}
.card .grow {
  flex: 1 1 auto;
}
```

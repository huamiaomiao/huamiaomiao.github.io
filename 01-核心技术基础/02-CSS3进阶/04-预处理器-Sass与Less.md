# 预处理器：Sass 与 Less

> 目标：掌握变量、嵌套、Mixin、函数与模块化组织，理解与现代 CSS（自定义属性、PostCSS）如何配合。

## 为什么需要预处理器

- 变量与复用：主题色、间距、字号等统一管理
- 程序化能力：计算、条件、循环生成样式
- 模块化与约束：拆分文件、命名规范、作用域控制
- 生态成熟：丰富的社区工具与最佳实践

## 快速对比

- 语法：Sass（.scss/.sass）、Less（.less），主流更偏向 SCSS 语法
- 变量：Sass `$color`，Less `@color`
- Mixin：两者均支持；Sass 支持参数、默认值、`@content`
- 函数：Sass `@function` 更强大；Less 用 mixin 实现计算亦可
- 模块：Sass 原生模块系统（`@use`/`@forward`）优于 Less 的导入

## 典型能力示例

SCSS：

```scss
// _variables.scss
$primary: #0969da;
$radius: 10px;
$space: 12px;

// _mixins.scss
@mixin card($elev: 12px) {
  background: #fff;
  border: 1px solid #d0d7de;
  border-radius: $radius;
  box-shadow: 0 6px $elev rgba(0, 0, 0, 0.08);
}

@mixin respond($breakpoint) {
  @if $breakpoint == md {
    @media (min-width: 768px) {
      @content;
    }
  } @else if $breakpoint == lg {
    @media (min-width: 1024px) {
      @content;
    }
  }
}

// styles.scss
@use "variables" as *;
@use "mixins";

.btn {
  padding: 10px 16px;
  background: $primary;
  color: #fff;
  border-radius: $radius;
  &:hover {
    background: darken($primary, 6%);
  }
}

.card {
  @include mixins.card(18px);
  padding: $space * 2;
  @include mixins.respond(md) {
    padding: $space * 3;
  }
}
```

Less：

```less
@primary: #0969da;
@radius: 10px;
@space: 12px;

.card(@elev: 12px) {
  background: #fff;
  border: 1px solid #d0d7de;
  border-radius: @radius;
  box-shadow: 0 6px @elev rgba(0, 0, 0, 0.08);
}

.respond(@breakpoint) when (@breakpoint = md) {
  @media (min-width: 768px) {
    @arguments();
  }
}
.respond(@breakpoint) when (@breakpoint = lg) {
  @media (min-width: 1024px) {
    @arguments();
  }
}

.btn {
  padding: 10px 16px;
  background: @primary;
  color: #fff;
  border-radius: @radius;
}
.btn:hover {
  background: darken(@primary, 6%);
}

.card {
  .card(18px);
  padding: @space * 2;
  .respond(md) {
    padding: @space * 3;
  }
}
```

## 与现代 CSS 的关系

- CSS 自定义属性（`--primary`）可在运行时动态切换主题，与预处理器变量互补
- PostCSS 可进行补全与转换（如 Autoprefixer），与 Sass/Less 不冲突
- 新特性（如嵌套、颜色函数）正在原生化，逐步缩小差距，但工程化仍常用 Sass

## 工程集成（以 Vite + Sass 为例）

安装：

```bash
npm i -D sass
```

使用：

```css
/* main.scss */
@use "./styles/variables" as *;
@use "./styles/mixins";
@use "./styles/components/button";
```

在入口中引入：

```js
import "./main.scss";
```

## 组织与规范

- 拆分：`variables`、`mixins`、`functions`、`base`、`components`、`layout`
- 命名：BEM/OOCSS/ITCSS 等，结合组件化框架（Vue/React）的局部样式方案
- 约束：避免过深嵌套（>3 层），避免滥用全局变量

## 练习建议

- 抽取一套主题变量，并实现“浅色/深色”切换（结合 CSS 变量）
- 封装常用 Mixin：卡片、响应式栅格、文本溢出省略
- 将项目零散样式重构为模块化的 Sass 架构

---

## 知识拓展

- 与 CSS 变量结合实现主题切换（运行时）

```scss
/* 预处理器生成主题变量的 fallback */
$primary: #0969da;
:root {
  --primary: #{$primary};
}
[data-theme="dark"] {
  --primary: #22c55e;
}
.btn {
  background: var(--primary);
}
```

- 生成响应式工具类

```scss
$spaces: (
  0: 0,
  1: 4px,
  2: 8px,
  3: 12px,
  4: 16px,
);
@each $k, $v in $spaces {
  .p-#{$k} {
    padding: $v;
  }
  .px-#{$k} {
    padding-inline: $v;
  }
  .py-#{$k} {
    padding-block: $v;
  }
}
```

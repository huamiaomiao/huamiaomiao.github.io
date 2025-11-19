# Vue 的系统指令（v- 指令入门）

> 目标：掌握最常用的指令以快速构建页面。

## 显示与条件

```html
<p v-show="visible">我会通过 CSS display 切换</p>
<p v-if="ok">条件渲染</p>
<p v-else>条件不成立</p>
```

- `v-show`：频繁切换时更合适（不销毁 DOM）
- `v-if`：切换不频繁/开销较大时（创建/销毁 DOM）

## 列表渲染

```html
<li v-for="(todo, i) in todos" :key="todo.id">{{ i + 1 }}. {{ todo.text }}</li>
```

为每项提供稳定的 `:key`，便于高效更新。

## 双向绑定

```html
<input v-model="name" placeholder="你的名字" />
<p>Hello, {{ name }}</p>
```

`v-model` 等同于 `:value + @input` 的结合，针对不同控件有轻微差异。

## 属性与事件绑定

```html
<button :disabled="loading" @click="submit">提交</button>
```

## 动态 class 与 style

```html
<div :class="{ active: isActive, error: hasError }"></div>
<div :style="{ color: color, padding: size + 'px' }"></div>
```

## v-html（谨慎）

```html
<div v-html="rawHtml"></div>
```

可能导致 XSS，严格控制来源。

## 小练习

1. 使用 `v-if` + `v-for` 实现“无数据时提示”的列表
2. 做一个表单双向绑定，并在下方实时展示 JSON
3. 用 `:class` 切换错误/成功样式

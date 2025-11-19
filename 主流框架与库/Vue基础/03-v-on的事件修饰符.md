# v-on 的事件修饰符

> 目标：掌握事件修饰符避免样板代码，写出清晰的交互逻辑。

## 基本用法

```html
<button @click="count++">+1</button>
```

## 常见修饰符

- `.stop`：阻止冒泡
- `.prevent`：阻止默认行为
- `.capture`：使用捕获阶段
- `.self`：仅在自身元素上触发
- `.once`：仅触发一次
- `.passive`：改善滚动性能（不阻止默认）

```html
<a @click.prevent href="/submit">提交</a>
<div @click.capture="log">...</div>
<button @click.once="init">仅一次</button>
```

## 按键修饰符

```html
<input @keyup.enter="search" /> <input @keyup.ctrl.c="copy" />
```

## 鼠标按钮修饰符

```html
<div
  @click.left="left"
  @click.right.prevent="context"
  @click.middle="mid"
></div>
```

## 组合案例

```html
<form @submit.prevent="save">
  <input v-model="title" @keyup.enter.exact="save" />
</form>
```

## 小练习

1. 用 `.stop` 实现卡片内部按钮不触发外层点击
2. 用 `.once` 做一个“初始化”按钮
3. 输入框按 Enter 触发搜索，Esc 清空

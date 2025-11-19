# ES6：变量 let、const 和块级作用域

> 目标：抛弃 `var`，理解块级作用域与暂时性死区。

## let 与 const

- `let`：块级作用域，可重新赋值
- `const`：块级作用域，不可重新赋值（引用地址不变）
- 默认使用 `const`，需要变化再用 `let`

```js
if (true) {
  let a = 1;
  const b = 2;
}
// console.log(a, b); // ReferenceError
```

## 暂时性死区（TDZ）

```js
// console.log(x); // ReferenceError
let x = 1;
```

## 循环中的闭包

```js
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i));
}
// 0 1 2
```

## 练习

- 用 `let/const` 重写旧代码，消除 `var` 引起的意外提升

## 练习：作用域 / 解构 / 箭头函数（含输出）

### 题目 1：块级作用域与暂时性死区

```js
let x = 1;
{
  // console.log(x); // A: ?
  let x = 2;
  console.log(x); // B: ?
}
console.log(x); // C: ?
```

期望答案：

- A：报错（暂时性死区）
- B：2
- C：1

### 题目 2：数组与对象解构、默认值

```js
const arr = [10, , 30];
const [a = 1, b = 2, c = 3, d = 4] = arr;
const { title = "JS", year: y = 2025 } = { year: 2024 };
console.log(a, b, c, d); // D: ?
console.log(title, y); // E: ?
```

期望答案：

- D：10 2 30 4
- E：JS 2024

### 题目 3：箭头函数与 this 绑定

```js
const obj = {
  val: 42,
  fn1() {
    return () => this.val;
  },
  fn2: () => this,
};
console.log(obj.fn1()()); // F: ?
console.log(obj.fn2() === window); // G: ?（Node 环境为 globalThis）
```

期望答案：

- F：42（fn1 返回的箭头函数捕获了 fn1 运行时的 this，指向 obj）
- G：true（箭头函数的 this 在定义处决定，这里是全局）

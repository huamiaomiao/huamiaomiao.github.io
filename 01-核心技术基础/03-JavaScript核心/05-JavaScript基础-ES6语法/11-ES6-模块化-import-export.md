# ES6：模块化 import/export

## 导出

```js
// math.js
export const PI = 3.14159;
export function area(r) {
  return PI * r * r;
}
export default function double(n) {
  return n * 2;
}
```

## 导入

```js
// app.js
import double, { PI, area } from "./math.js";
console.log(PI, area(2), double(3));
```

## 重命名与再导出

```js
export { area as circleArea } from "./math.js";
```

## 注意

- ESM 静态结构利于 Tree-Shaking；
- 浏览器原生模块需加 `type="module"`。\*\*\*

---

## 知识拓展

- 动态导入（按需加载）

```js
// 点击时再加载重组件
button.addEventListener("click", async () => {
  const { heavyFn } = await import("./heavy.js");
  heavyFn();
});
```

- Import Maps（浏览器原生别名，现代环境）

```html
<script type="importmap">
  {
    "imports": {
      "lodash": "https://cdn.skypack.dev/lodash-es"
    }
  }
</script>
<script type="module">
  import { chunk } from "lodash";
  console.log(chunk([1, 2, 3, 4], 2));
</script>
```

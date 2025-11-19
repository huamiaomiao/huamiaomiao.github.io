# 内置对象扩展：Set 数据结构

> 目标：理解 `Set` 的特性与典型用法（去重、集合运算）。

## 基本用法

```js
const s = new Set([1, 2, 2, 3]);
s.add(4).delete(1);
s.has(2); // true
[...s]; // [2,3,4]
```

唯一性：元素不重复（NaN 也只会有一个）。

## 去重

```js
const unique = (arr) => [...new Set(arr)];
```

## 集合运算

```js
const A = new Set([1, 2, 3]);
const B = new Set([2, 3, 4]);
const union = new Set([...A, ...B]); // 并集
const inter = new Set([...A].filter((x) => B.has(x))); // 交集
const diff = new Set([...A].filter((x) => !B.has(x))); // 差集
```

## WeakSet（了解）

- 仅接收对象引用，弱引用不阻止垃圾回收
- 不能遍历，适合做“标记集合”

## 练习

- 写一个 `uniqueBy(arr, key)`，根据对象的 `key` 去重

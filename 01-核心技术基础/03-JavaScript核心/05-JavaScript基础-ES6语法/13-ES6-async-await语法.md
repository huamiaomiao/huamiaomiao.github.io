# ES6 async/await（语法）— 入手即用

目标：会把既有 Promise 代码改写为 `async/await`，并正确处理错误、顺序与并发。

---

## 1. 它是什么

- `async`：把函数的返回值包成一个 `Promise`。
- `await`：在 `async` 函数内部暂停，等待一个 `Promise` 解决（resolve/reject）。

语义：用“同步写法”描述异步流程，减少层层 `.then()` 的嵌套。

---

## 2. 从 Promise 改写为 async/await

Promise 风格：

```ts
fetch("/api/user")
  .then((r) => r.json())
  .then((data) => console.log(data))
  .catch((err) => console.error(err));
```

async/await 改写：

```ts
async function loadUser() {
  try {
    const res = await fetch("/api/user");
    const data = await res.json();
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}
```

要点：`await` 只在 `async` 函数内使用；记得用 `try/catch` 捕获错误。

---

## 3. 顺序 vs 并发

顺序（一步一步等，代码最直观，耗时相加）：

```ts
async function loadTwo() {
  const a = await fetch("/api/a").then((r) => r.json());
  const b = await fetch("/api/b").then((r) => r.json());
  return { a, b };
}
```

并发（一起发请求，整体更快）：

```ts
async function loadTwoFast() {
  const [aRes, bRes] = await Promise.all([fetch("/api/a"), fetch("/api/b")]);
  const [a, b] = await Promise.all([aRes.json(), bRes.json()]);
  return { a, b };
}
```

建议：默认并发，除非确实存在数据依赖需要顺序执行。

---

## 4. 错误处理与超时

try/catch 捕获：

```ts
async function getSafe() {
  try {
    const res = await fetch("/api/data");
    if (!res.ok) throw new Error(res.statusText);
    return await res.json();
  } catch (e) {
    // 统一上报 / 兜底
    return null;
  }
}
```

AbortController 超时取消：

```ts
async function fetchWithTimeout(url: string, ms = 8000) {
  const ctrl = new AbortController();
  const t = setTimeout(() => ctrl.abort(), ms);
  try {
    const res = await fetch(url, { signal: ctrl.signal });
    return res;
  } finally {
    clearTimeout(t);
  }
}
```

---

## 5. 常见坑

- 只能在 `async` 函数内使用 `await`（Top-Level Await 仅在 ESM/现代构建里可用）。
- `Array.prototype.forEach` 不能用 `await` 等待结束，改用 `for...of`：

```ts
// 错误：不会等待
// items.forEach(async (item) => await doWork(item));

// 正确：逐个等待
for (const item of items) {
  await doWork(item);
}
```

- `await` 会“顺序化”并本可并发的任务，注意性能。

---

## 6. 最小实战（与 UI 联动）

```ts
import { ref } from "vue";

const loading = ref(false);
const error = ref<string | null>(null);
const user = ref<any>(null);

export async function loadUser() {
  loading.value = true;
  error.value = null;
  try {
    const res = await fetch("/api/user");
    if (!res.ok) throw new Error(res.statusText);
    user.value = await res.json();
  } catch (e: any) {
    error.value = e?.message ?? "请求失败";
  } finally {
    loading.value = false;
  }
}
```

至此，你已掌握：`async/await` 的基本用法、错误处理、顺序与并发策略，以及与 UI 状态的最小闭环。

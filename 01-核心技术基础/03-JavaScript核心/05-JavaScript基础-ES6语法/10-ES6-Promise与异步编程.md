# ES6：Promise 与异步编程

## 基本用法

```js
const p = new Promise((resolve) => {
  setTimeout(() => resolve(42), 300);
});
p.then((v) => console.log(v)).catch(console.error);
```

## 链式调用与错误传递

```js
fetch("/api/user")
  .then((res) => {
    if (!res.ok) throw new Error("网络错误");
    return res.json();
  })
  .then((data) => console.log(data))
  .catch((err) => console.error("失败：", err.message));
```

## 并发控制：all / allSettled / race / any

```js
const a = fetch("/a");
const b = fetch("/b");
const [ra, rb] = await Promise.all([a, b]);
```

## async/await 语法糖

```js
async function getUser() {
  try {
    const res = await fetch("/api/user");
    if (!res.ok) throw new Error("网络错误");
    return await res.json();
  } catch (e) {
    console.error(e);
    return null;
  }
}
```

## 练习

- 封装一个 `withTimeout(promise, ms)`，超时则 reject。\*\*\*

---

## 知识拓展

- 超时与可取消（AbortController）

```js
function withTimeout(promise, ms, signal){
  const t = new Promise((_, rej) => {
    const id = setTimeout(() => rej(new Error('Timeout')), ms);
    signal?.addEventListener('abort', () => { clearTimeout(id); rej(signal.reason || new Error('Aborted')); });
  });
  return Promise.race([promise, t]);
}

const c = new AbortController();
setTimeout(()=>c.abort(new Error('用户取消')), 200);
withTimeout(fetch('/api/data'), 3000, c.signal).then(...).catch(console.error);
```

- 并发池（限制同时进行的任务数）

```js
async function pool(tasks, limit = 4) {
  const ret = [];
  const queue = tasks.slice();
  const running = new Set();
  async function run(task) {
    const p = task().finally(() => running.delete(p));
    running.add(p);
    ret.push(p);
    if (running.size >= limit) await Promise.race(running);
  }
  while (queue.length) await run(queue.shift());
  return Promise.allSettled(ret);
}
```

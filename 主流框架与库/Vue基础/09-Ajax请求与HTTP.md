# Vue 中的 Ajax 请求（HTTP）

> 目标：在组件中请求后端数据，处理加载/错误状态与取消。

## 选择请求库

- 原生 `fetch`：轻量、现代
- Axios：更全功能（拦截器、取消、超时、适配器等）

## 基本示例（fetch）

```js
export default {
  data: () => ({ list: [], loading: false, error: "" }),
  async mounted() {
    this.loading = true;
    try {
      const res = await fetch("/api/todos");
      if (!res.ok) throw new Error(res.statusText);
      this.list = await res.json();
    } catch (e) {
      this.error = e.message || "请求失败";
    } finally {
      this.loading = false;
    }
  },
};
```

## 取消与超时（AbortController）

```js
const ctrl = new AbortController();
const timer = setTimeout(() => ctrl.abort(), 8000);
const res = await fetch(url, { signal: ctrl.signal });
clearTimeout(timer);
```

## Axios 拦截器（可选）

```js
axios.interceptors.request.use((config) => {
  // 携带 token
  return config;
});
axios.interceptors.response.use(
  (res) => res,
  (err) => {
    // 统一错误提示
    return Promise.reject(err);
  }
);
```

## 进阶：组合函数封装

```js
// useFetch.js
import { ref } from "vue";
export function useFetch(getUrl) {
  const data = ref(null),
    error = ref(""),
    loading = ref(false);
  const run = async (...args) => {
    loading.value = true;
    error.value = "";
    try {
      const url = typeof getUrl === "function" ? getUrl(...args) : getUrl;
      const res = await fetch(url);
      if (!res.ok) throw new Error(res.statusText);
      data.value = await res.json();
    } catch (e) {
      error.value = e.message || "请求失败";
    } finally {
      loading.value = false;
    }
  };
  return { data, error, loading, run };
}
```

## 练习建议

- 封装 `useAxios`，支持取消和并发
- 在列表中添加“加载更多/分页”

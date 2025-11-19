# Pinia 状态管理入门

## 安装与创建 Store

```bash
npm i pinia
```

```js
// main.ts
import { createApp } from "vue";
import { createPinia } from "pinia";
import App from "./App.vue";
createApp(App).use(createPinia()).mount("#app");
```

```ts
// stores/counter.ts
import { defineStore } from "pinia";
export const useCounter = defineStore("counter", {
  state: () => ({ n: 0 }),
  getters: { double: (s) => s.n * 2 },
  actions: {
    inc() {
      this.n++;
    },
  },
});
```

```vue
<script setup lang="ts">
import { useCounter } from "./stores/counter";
const c = useCounter();
</script>
<template>
  <button @click="c.inc()">{{ c.n }} / {{ c.double }}</button>
</template>
```

## 练习

- 将“用户信息”与“主题设置”拆分为独立的 store。\*\*\*

---

## 知识拓展

- 持久化与订阅

```ts
import { defineStore } from "pinia";
export const useAuth = defineStore("auth", {
  state: () => ({ token: "" }),
});
const auth = useAuth();
auth.$subscribe((_mutation, state) => {
  localStorage.setItem("auth", JSON.stringify(state));
});
```

- 异步 Action 与错误处理

```ts
export const useUsers = defineStore("users", {
  state: () => ({ list: [], loading: false, error: "" }),
  actions: {
    async fetchAll() {
      this.loading = true;
      this.error = "";
      try {
        const res = await fetch("/api/users");
        if (!res.ok) throw new Error("网络错误");
        this.list = await res.json();
      } catch (e: any) {
        this.error = e.message;
      } finally {
        this.loading = false;
      }
    },
  },
});
```

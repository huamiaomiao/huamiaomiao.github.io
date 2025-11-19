## 实战：Pinia 计数器（可运行）

目标：使用 Pinia 管理一个计数器的状态，演示跨组件共享、持久化到 localStorage。

### 运行方式

- 新建 `vue-pinia.html`，复制下方代码，直接在浏览器打开即可。

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <meta charset="utf-8" />
  <title>Pinia Counter</title>
  <script src="https://unpkg.com/vue@3/dist/vue.global.prod.js"></script>
  <script src="https://unpkg.com/pinia@2/dist/pinia.iife.prod.js"></script>
  <style>
    body {
      font-family: system-ui;
      margin: 40px auto;
      max-width: 560px;
    }
    .card {
      border: 1px solid #e5e7eb;
      border-radius: 8px;
      padding: 16px;
    }
    button {
      padding: 8px 12px;
      margin-right: 6px;
    }
  </style>
  <body>
    <div id="app">
      <h1>Pinia 计数器</h1>
      <counter-panel class="card" style="margin-bottom:12px"></counter-panel>
      <counter-panel class="card"></counter-panel>
    </div>
    <script>
      const { createPinia, defineStore } = Pinia;
      const useCounter = defineStore("counter", {
        state: () => ({ n: Number(localStorage.getItem("n") || 0) }),
        actions: {
          inc() {
            this.n++;
            localStorage.setItem("n", this.n);
          },
          dec() {
            this.n--;
            localStorage.setItem("n", this.n);
          },
          reset() {
            this.n = 0;
            localStorage.setItem("n", this.n);
          },
        },
      });
      const CounterPanel = {
        template: `
          <div>
            <p>当前计数：<strong>{{ store.n }}</strong></p>
            <button @click="store.inc()">+1</button>
            <button @click="store.dec()">-1</button>
            <button @click="store.reset()">重置</button>
          </div>`,
        setup() {
          const store = useCounter();
          return { store };
        },
      };
      const app = Vue.createApp({ components: { CounterPanel } });
      app.use(createPinia()).mount("#app");
    </script>
  </body>
</html>
```

### 期望输出/效果

- 页面有两个面板共享同一个计数器状态；加减或重置会同步到两个面板；刷新后数值保留。

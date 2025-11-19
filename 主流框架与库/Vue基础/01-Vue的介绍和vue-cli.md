# Vue 的介绍和 Vue CLI

> 目标：理解 Vue 的核心思想与使用场景，快速创建一个可运行的 Vue 项目。

## Vue 是什么

- 渐进式框架：可从“视图层”开始逐步引入（组件、路由、状态管理）
- 核心：声明式渲染 + 组件化 + 响应式数据
- 生态：Vue Router、Pinia/Vuex、Vite、Vue CLI、Nuxt 等

## 声明式渲染的直觉

```html
<div id="app">{{ message }}</div>
```

```js
const app = Vue.createApp({ data: () => ({ message: "Hello Vue" }) });
app.mount("#app");
```

当 `message` 变化时，视图自动更新，开发者无需手动操作 DOM。

## 组件化

- 组件是可复用的 UI 单元：模板、逻辑、样式封装在一起
- 父子组件之间通过 props/事件/插槽通信

## 快速创建项目

推荐 Vite（更快）或 Vue CLI（向导齐全）。此处演示 Vue CLI。

安装 CLI：

```bash
npm i -g @vue/cli
```

创建项目：

```bash
vue create hello-vue
# 选择手动特性：Babel、Router、Vuex(或 Pinia)、Linter、Unit/E2E 等
cd hello-vue
npm run serve
```

Vite 方式：

```bash
npm create vite@latest hello-vue -- --template vue
cd hello-vue
npm i
npm run dev
```

## 项目结构（典型）

- `src/main.js`：应用入口，创建并挂载根组件
- `src/App.vue`：根组件
- `src/components/*`：业务组件
- `src/router/*`：路由配置（可选）
- `src/store/*`：状态管理（可选）

## 小练习

1. 用 CLI 或 Vite 创建一个项目，修改 `App.vue` 文案
2. 新建 `Hello.vue` 组件并在 `App.vue` 中使用
3. 打包项目并观察 `dist/` 产物结构

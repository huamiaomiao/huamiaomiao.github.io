# Vite 从 0 到 1：上手到上线的最短路径（重要要点浓缩）

你将用最少的步骤，从“没有项目”到“本地开发、构建、预览、上线”。文末附常见问题和必备配置示例。默认示例用 Vue，可按需替换为 React/Svelte（命令仅模板名不同）。

---

一、准备环境（一次性）

```bash
node -v     # 建议 Node.js 18+（LTS 更稳）
npm -v      # 或使用 pnpm/yarn（推荐 pnpm）
```

二、创建项目

方式 A：官方脚手架（推荐）

```bash
npm create vite@latest my-app -- --template vue
# 其他模板：vanilla / react / svelte / lit / solid / vue-ts / react-ts ...
cd my-app
npm i
```

方式 B：手动安装（更灵活）

```bash
mkdir my-app && cd my-app
npm init -y
npm i -D vite
mkdir src
printf '%s\n' '<!doctype html><html><body><div id="app"></div><script type="module" src="/src/main.js"></script></body></html>' > index.html
printf '%s\n' "document.getElementById('app').innerHTML = 'Hello Vite';" > src/main.js
```

package.json（脚本）

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  }
}
```

三、启动开发服务器

```bash
npm run dev
# 打开 http://localhost:5173
```

目录结构（典型）

```
my-app/
  index.html              # 开发期入口（不是模板输出，而是直连 ESM）
  src/
    main.ts / main.js
    App.vue               # 若为 Vue 模板
  public/                 # 静态资源原样复制到 / 根目录
  vite.config.ts          # 可选，后续新增
```

四、vite.config 基础配置（重要）

新增 `vite.config.ts`：

```ts
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue"; // 若是 React：@vitejs/plugin-react

export default defineConfig({
  plugins: [vue()],
  base: "/", // 站点基础路径（部署到子路径时设置为 /子目录/）
  server: {
    port: 5173,
    open: true,
    proxy: {
      // 本地接口代理（避免跨域）
      "/api": "http://localhost:3000",
    },
  },
  resolve: {
    alias: { "@": "/src" }, // 路径别名：import '@/utils/xxx'
  },
  build: {
    outDir: "dist",
    sourcemap: false,
    rollupOptions: {
      output: {
        manualChunks: {
          // 重要依赖单独分包（加快首屏）
          vue: ["vue"],
        },
      },
    },
  },
});
```

五、环境变量与模式（重要）

在根目录创建 `.env` / `.env.development` / `.env.production`：

```
VITE_API_BASE=https://api.example.com
```

代码中读取：

```ts
console.log(import.meta.env.MODE); // 当前模式
console.log(import.meta.env.VITE_API_BASE); // 以 VITE_ 开头的变量会暴露到客户端
```

切换模式：

```bash
vite build --mode staging
```

六、CSS 与预处理器（Sass/Less/PostCSS）

直接安装即可使用（Vite 内置支持导入 CSS/预处理器）：

```bash
npm i -D sass
```

示例：

```scss
/* src/styles/main.scss */
$primary: #22c55e;
.btn {
  background: $primary;
}
```

```ts
// src/main.ts
import "./styles/main.scss";
```

七、静态资源与图片字体（关键规则）

- `import logo from '@/assets/logo.svg'` 可得到路径；在模板里 `<img :src="logo" />`
- `public/` 下文件会原样复制到根路径，直接用 `/xxx.png`
- 小体积资源自动内联为 base64，超过阈值生成独立文件（可在 build 配置里调）

八、常用插件（按需）

Vue

```bash
npm i -D @vitejs/plugin-vue
```

React

```bash
npm i -D @vitejs/plugin-react
```

SVG 组件化（以 Vue 为例，选择社区方案如 vite-svg-loader）

```bash
npm i -D vite-svg-loader
// vite.config.ts
import svgLoader from 'vite-svg-loader'
export default defineConfig({ plugins: [vue(), svgLoader()] })
```

九、代码分割与动态导入（重要）

动态导入页面或重组件，减少首屏体积：

```ts
const About = () => import("@/pages/About.vue"); // Vue 路由懒加载
// React：const About = lazy(() => import('./pages/About'))
```

十、Mock/代理（本地联调）

开发期代理（见上 server.proxy）。简单 Mock：

```ts
// src/mocks/users.ts
export function getUsers() {
  return [{ id: 1, name: "Ada" }];
}
// 在代码中 if (import.meta.env.DEV) 读取本地 mock；生产改为真实接口
```

十一、类型/规范/测试（推荐最小闭环）

TypeScript（若未选 TS 模板）

```bash
npm i -D typescript
npm run dev    # Vite 自动处理 ts（无需额外打包配置）
```

ESLint + Prettier（核心规则）

```bash
npm i -D eslint prettier eslint-plugin-import eslint-config-prettier
```

Vitest（单元测试）

```bash
npm i -D vitest @vitest/ui @vitejs/plugin-vue
# package.json
{ "scripts": { "test": "vitest" } }
```

十二、构建与本地预览

```bash
npm run build      # 产出 dist/
npm run preview    # 本地以生产构建预览
```

十三、部署（三选一示例）

- 静态托管（Netlify/Vercel）：绑定仓库，构建命令 `npm run build`，发布目录 `dist`
- Nginx：
  ```
  root /var/www/my-app/dist;
  location / { try_files $uri /index.html; }   # SPA 需要
  ```
- GitHub Pages：把 `dist` 推到 `gh-pages` 分支，`base` 设为仓库子路径 `/repo/`

十四、常见问题（速查）

- 路由 404（SPA）：服务端需将未知路由回退到 `index.html`（Nginx 示例见上）
- 部署子路径：`vite.config.ts` 设置 `base: '/子路径/'`
- 跨域：用 `server.proxy` 在开发期代理；生产由网关/服务器处理
- 大依赖慢：用 `build.rollupOptions.output.manualChunks` 分包；按需动态导入
- 图片不显示：`/public` 直接根路径访问；`src/assets` 建议通过 `import` 引用

十五、最小可用示例（Vue）

index.html

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
  </head>
  <body>
    <div id="app"></div>
    <script type="module" src="/src/main.ts"></script>
  </body>
</html>
```

src/main.ts

```ts
import { createApp } from "vue";
import App from "./App.vue";
createApp(App).mount("#app");
```

src/App.vue

```vue
<template>
  <main>
    <h1>Hello Vite + Vue</h1>
    <img :src="logo" alt="logo" width="80" />
  </main>
</template>
<script setup lang="ts">
import logo from "@/assets/logo.svg";
</script>
<style>
main {
  font: 16px/1.6 system-ui;
  padding: 24px;
}
</style>
```

vite.config.ts（与上示例一致，确保有 `@vitejs/plugin-vue` 和别名 `@`）

到这里，你已完成：创建 → 开发(HMR) → 配置 → 环境变量 → 资源/样式 → 分包优化 → 构建 → 预览 → 部署 的完整闭环。\*\*\* End Patch

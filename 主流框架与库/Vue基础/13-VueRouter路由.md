# Vue Router 路由（从入门到上手）

> 目标：完成多页面导航，掌握动态/嵌套路由、编程式导航与守卫、懒加载。

## 安装与创建 Router

```bash
npm i vue-router
```

```js
// src/router/index.js
import { createRouter, createWebHistory } from "vue-router";
import Home from "../views/Home.vue";
import About from "../views/About.vue";

const routes = [
  { path: "/", name: "home", component: Home },
  { path: "/about", name: "about", component: About },
];

export const router = createRouter({
  history: createWebHistory(),
  routes,
  scrollBehavior() {
    return { top: 0 };
  },
});
```

```js
// src/main.js
import { createApp } from "vue";
import App from "./App.vue";
import { router } from "./router";
createApp(App).use(router).mount("#app");
```

## 基本使用

```html
<router-link to="/">首页</router-link>
<router-link :to="{ name: 'about' }">关于</router-link>
<router-view />
```

## 动态路由与参数

```js
{ path: "/users/:id", name: "user", component: () => import("../views/User.vue") }
```

```vue
<!-- 在组件中读取参数 -->
<script>
export default {
  computed: {
    userId() { return this.$route.params.id; },
  },
};
</script>
```

## 嵌套路由

```js
{
  path: "/users/:id",
  component: () => import("../views/User.vue"),
  children: [
    { path: "", name: "user-profile", component: () => import("../views/UserProfile.vue") },
    { path: "posts", name: "user-posts", component: () => import("../views/UserPosts.vue") },
  ],
}
```

```html
<!-- User.vue -->
<router-link :to="{ name: 'user-profile', params: { id } }">资料</router-link>
<router-link :to="{ name: 'user-posts', params: { id } }">文章</router-link>
<router-view /> <!-- 渲染子路由 -->
```

## 编程式导航

```js
// 进入
this.$router.push({ name: "about" });
// 返回
this.$router.back();
```

## 导航守卫（鉴权/埋点）

```js
router.beforeEach((to, from, next) => {
  const authed = Boolean(localStorage.getItem("token"));
  if (to.meta.requiresAuth && !authed) {
    next({ path: "/login", query: { redirect: to.fullPath } });
  } else {
    next();
  }
});
```

## 路由懒加载与分包

```js
const Home = () => import(/* webpackChunkName: "home" */ "../views/Home.vue");
```

## 滚动行为

```js
const router = createRouter({
  history: createWebHistory(),
  routes,
  scrollBehavior(to, from, saved) {
    if (saved) return saved; // 返回浏览器记录位置
    return { top: 0 };
  },
});
```

## 常见问题
- 切换同一路由不同参数时，组件不会重建：使用 `watch(() => route.params.id, ...)` 或为 `<router-view :key="$route.fullPath" />`
- 嵌套路由必须在父组件中放置一个 `<router-view>`

## 练习建议
- 搭建“用户 → 用户资料/文章”三级路由，文章路由要求登录  
- 为详情页启用懒加载与返回时还原滚动位置  



# Vue Router 进阶与导航守卫

## 动态路由与参数

```js
{ path: '/post/:id', component: Post }
// 在组件内：useRoute().params.id
```

## 嵌套路由

```js
{
  path: '/user/:id',
  component: UserLayout,
  children: [
    { path: 'profile', component: Profile },
    { path: 'posts', component: UserPosts },
  ],
}
```

## 全局与路由独享守卫

```js
router.beforeEach((to) => {
  if (to.meta.requiresAuth && !isAuthed()) return "/login";
});

const route = {
  path: "/admin",
  component: Admin,
  beforeEnter: () => isAdmin() || "/403",
};
```

## 滚动行为

```js
const router = createRouter({
  // ...
  scrollBehavior(to, from, saved) {
    if (saved) return saved;
    if (to.hash) return { el: to.hash, behavior: "smooth" };
    return { top: 0 };
  },
});
```

## 练习

- 给需要登录的页面添加守卫，并实现滚动位置还原。\*\*\*

---

## 知识拓展

- 路由懒加载与分块命名

```js
const routes = [
  {
    path: "/",
    component: () => import(/* webpackChunkName: "home" */ "./pages/Home.vue"),
  },
  { path: "/about", component: () => import("./pages/About.vue") },
];
```

- 路由元信息与标题管理

```js
const routes = [{ path: "/", component: Home, meta: { title: "首页" } }];
router.afterEach((to) => {
  document.title = to.meta.title ? `${to.meta.title} - 站点` : "站点";
});
```

- 基于权限的动态路由

```js
const adminRoutes = [
  {
    path: "/admin",
    component: Admin,
    meta: { requiresAuth: true, role: "admin" },
  },
];
if (user.role === "admin") adminRoutes.forEach((r) => router.addRoute(r));
```

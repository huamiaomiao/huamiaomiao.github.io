## 实战：Vue Router + 动态详情（可运行）

目标：使用 Vue Router 实现列表页 + 动态详情页（/post/:id），并在详情页展示对应数据。

### 运行方式

- 新建 `vue-router-demo.html`，复制下方代码，直接在浏览器打开即可。

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <meta charset="utf-8" />
  <title>Vue Router Demo</title>
  <script src="https://unpkg.com/vue@3/dist/vue.global.prod.js"></script>
  <script src="https://unpkg.com/vue-router@4"></script>
  <style>
    body {
      font-family: system-ui;
      margin: 40px auto;
      max-width: 760px;
    }
    a {
      color: #2563eb;
      text-decoration: none;
    }
    nav {
      display: flex;
      gap: 12px;
      margin-bottom: 16px;
    }
    .card {
      padding: 12px;
      border: 1px solid #e5e7eb;
      border-radius: 8px;
      margin: 8px 0;
    }
  </style>
  <body>
    <div id="app">
      <nav>
        <router-link to="/">首页</router-link>
        <router-link to="/about">关于</router-link>
      </nav>
      <router-view></router-view>
    </div>
    <script>
      const data = [
        { id: 1, title: "第一篇文章", summary: "这是第一篇摘要" },
        { id: 2, title: "第二篇文章", summary: "这是第二篇摘要" },
        { id: 3, title: "第三篇文章", summary: "这是第三篇摘要" },
      ];

      const Home = {
        template: `
          <div>
            <h1>文章列表</h1>
            <div v-for="p in posts" :key="p.id" class="card">
              <h3>{{ p.title }}</h3>
              <p>{{ p.summary }}</p>
              <router-link :to="'/post/' + p.id">查看详情</router-link>
            </div>
          </div>`,
        data() {
          return { posts: data };
        },
      };
      const Post = {
        template: `
          <div v-if="post">
            <h1>{{ post.title }}</h1>
            <p>这里是文章 #{{ post.id }} 的详细内容示例。</p>
            <router-link to="/">← 返回列表</router-link>
          </div>
          <div v-else>未找到该文章</div>
        `,
        computed: {
          post() {
            const id = Number(this.$route.params.id);
            return data.find((p) => p.id === id);
          },
        },
      };
      const About = { template: "<h1>关于本示例</h1>" };

      const routes = [
        { path: "/", component: Home },
        { path: "/post/:id", component: Post },
        { path: "/about", component: About },
      ];
      const router = VueRouter.createRouter({
        history: VueRouter.createWebHashHistory(), // Demo 用 hash，避免本地 404
        routes,
      });
      const app = Vue.createApp({});
      app.use(router).mount("#app");
    </script>
  </body>
</html>
```

### 期望输出/效果

- 首页展示三条“文章”，点击任意“查看详情”跳到对应 ID 的详情页；地址栏带有 hash 路由。

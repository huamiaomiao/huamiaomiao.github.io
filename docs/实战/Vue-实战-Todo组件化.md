## 实战：Vue 组件化 Todo（可运行）

目标：组件化实现 Todo 清单，支持新增/勾选/删除，数据持久化到 localStorage。

### 运行方式

- 新建 `vue-todo.html`，复制下方代码，直接用浏览器打开即可看到结果。

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <meta charset="utf-8" />
  <title>Vue Todo</title>
  <script src="https://unpkg.com/vue@3/dist/vue.global.prod.js"></script>
  <style>
    body {
      font-family: system-ui;
      margin: 40px auto;
      max-width: 720px;
    }
    .row {
      display: flex;
      gap: 8px;
    }
    input {
      flex: 1;
      padding: 8px 10px;
    }
    button {
      padding: 8px 12px;
    }
    li {
      display: flex;
      align-items: center;
      gap: 8px;
      padding: 6px 0;
    }
    li.done span {
      text-decoration: line-through;
      color: #888;
    }
  </style>
  <body>
    <div id="app">
      <h1>Vue 组件化 Todo</h1>
      <todo-input @add="addTodo"></todo-input>
      <p>未完成：{{ left }} / 总数：{{ todos.length }}</p>
      <todo-list
        :items="todos"
        @toggle="toggleTodo"
        @remove="removeTodo"
      ></todo-list>
    </div>
    <script>
      const TodoInput = {
        template: `
          <div class="row">
            <input v-model.trim="text" @keydown.enter="emitAdd" placeholder="输入待办..." />
            <button @click="emitAdd">添加</button>
          </div>`,
        data() {
          return { text: "" };
        },
        methods: {
          emitAdd() {
            if (!this.text) return;
            this.$emit("add", this.text);
            this.text = "";
          },
        },
      };
      const TodoList = {
        props: ["items"],
        emits: ["toggle", "remove"],
        template: `
          <ul>
            <li v-for="(t,i) in items" :key="i" :class="{done: t.done}">
              <input type="checkbox" v-model="t.done" @change="$emit('toggle', i)" />
              <span>{{ t.text }}</span>
              <button @click="$emit('remove', i)">删除</button>
            </li>
          </ul>`,
      };
      const STORAGE_KEY = "vue_todos";
      const app = Vue.createApp({
        components: { TodoInput, TodoList },
        data() {
          return {
            todos: JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]"),
          };
        },
        computed: {
          left() {
            return this.todos.filter((t) => !t.done).length;
          },
        },
        watch: {
          todos: {
            handler(v) {
              localStorage.setItem(STORAGE_KEY, JSON.stringify(v));
            },
            deep: true,
          },
        },
        methods: {
          addTodo(text) {
            this.todos.unshift({ text, done: false });
          },
          toggleTodo(i) {
            this.todos[i].done = !this.todos[i].done;
          },
          removeTodo(i) {
            this.todos.splice(i, 1);
          },
        },
      });
      app.mount("#app");
    </script>
  </body>
</html>
```

### 期望输出/效果

- 新增后展示在顶部，勾选变灰、统计更新，删除从列表移除；刷新后数据保留。

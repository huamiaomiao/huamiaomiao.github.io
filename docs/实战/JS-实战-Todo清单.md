## 实战：原生 JS 实现 Todo 清单（含截图说明）

目标：无需任何框架，使用原生 HTML/CSS/JS 实现可新增/勾选/删除的 Todo。

### 功能要求

- 输入框新增待办，回车或点击“添加”
- 列表展示待办项，支持勾选完成、删除
- 顶部统计未完成数量
- 数据保存在 localStorage，刷新不丢

### 代码骨架（可直接复制为 `todo.html` 运行）

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <meta charset="utf-8" />
  <title>Todo - 原生JS</title>
  <style>
    body {
      font-family: system-ui;
      margin: 40px auto;
      max-width: 640px;
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
  <h1>Todo 清单</h1>
  <div class="row">
    <input id="input" placeholder="输入待办..." />
    <button id="add">添加</button>
  </div>
  <p id="stats"></p>
  <ul id="list"></ul>
  <script>
    const input = document.getElementById('input');
    const list = document.getElementById('list');
    const stats = document.getElementById('stats');
    const addBtn = document.getElementById('add');
    let todos = JSON.parse(localStorage.getItem('todos')||'[]');

    function save(){ localStorage.setItem('todos', JSON.stringify(todos)); }
    function render(){
      list.innerHTML='';
      todos.forEach((t,i)=>{
        const li=document.createElement('li');
        li.className=t.done?'done':'';
        li.innerHTML = \`\n        <input type="checkbox" \${t.done?'checked':''}>\n        <span>\${t.text}</span>\n        <button>删除</button>\n      \`;
        const [ck, , del]=li.querySelectorAll('*');
        ck.onchange=()=>{ t.done=ck.checked; save(); render(); };
        del.onclick=()=>{ todos.splice(i,1); save(); render(); };
        list.appendChild(li);
      });
      const left = todos.filter(t=>!t.done).length;
      stats.textContent = \`未完成：\${left} / 总数：\${todos.length}\`;
    }
    function add(){
      const text=input.value.trim();
      if(!text) return;
      todos.unshift({text, done:false});
      input.value=''; save(); render();
    }
    addBtn.onclick=add;
    input.onkeydown=(e)=> e.key==='Enter' && add();
    render();
  </script>
</html>
```

### 期望输出/效果

- 新增后列表出现条目，勾选后样式变灰并统计减少，删除后从列表移除
- 刷新页面后仍能看到之前的待办

## 实战：Fetch 请求 + DOM 渲染（含输出）

目标：使用 Fetch 获取公开 API 数据，渲染到页面，并支持搜索过滤。

### 示例（可直接复制为 `users.html` 运行）

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <meta charset="utf-8" />
  <title>Users - Fetch Demo</title>
  <style>
    body {
      font-family: system-ui;
      margin: 40px auto;
      max-width: 760px;
    }
    input {
      padding: 8px 10px;
      width: 100%;
      margin-bottom: 12px;
    }
    table {
      width: 100%;
      border-collapse: collapse;
    }
    th,
    td {
      border: 1px solid #ddd;
      padding: 8px;
    }
    th {
      background: #f6f8fa;
    }
  </style>
  <h1>用户列表</h1>
  <input id="q" placeholder="按名称/邮箱搜索..." />
  <table>
    <thead>
      <tr>
        <th>ID</th>
        <th>姓名</th>
        <th>邮箱</th>
      </tr>
    </thead>
    <tbody id="tbody">
      <tr>
        <td colspan="3">加载中...</td>
      </tr>
    </tbody>
  </table>
  <script>
    const tbody=document.getElementById('tbody');
    const q=document.getElementById('q');
    let data=[];
    async function load(){
      try{
        const res=await fetch('https://jsonplaceholder.typicode.com/users');
        data=await res.json();
        render();
      }catch(e){
        tbody.innerHTML = '<tr><td colspan=\"3\">加载失败</td></tr>';
      }
    }
    function render(){
      const kw=q.value.trim().toLowerCase();
      const rows=data.filter(u=>{
        const s=(u.name+' '+u.email).toLowerCase();
        return !kw || s.includes(kw);
      }).map(u=>\`<tr><td>\${u.id}</td><td>\${u.name}</td><td>\${u.email}</td></tr>\`).join('');
      tbody.innerHTML = rows || '<tr><td colspan=\"3\">无匹配结果</td></tr>';
    }
    q.oninput=render;
    load();
  </script>
</html>
```

### 期望输出/效果

- 页面初次加载时显示 10 个用户；输入关键字可实时过滤表格
- 网络失败时表格出现“加载失败”提示

# 第 8 周 Day 05 - 2026.11.13

> 前端最后一块（Vue + Ajax）学完。力扣今天只讨论了思路，没写代码

## 今天学了（P25–30 Vue + Ajax）

代码在 `E:\javaweb\代码\JS-Vue\`，12–16 这几个文件是今天的产出：Vue 快速入门、员工列表（常用指令版）、Axios 入门、（异步交互版）。

### 1) Vue 快速入门（P25）

```javascript
import { createApp } from 'https://unpkg.com/vue@3/dist/vue.esm-browser.js'

createApp({
  data() {
    return {
      message: 'Hello Vue',
      count: 100
    }
  }
}).mount('#app')
```
```html
<div id="app">
  <h1>{{message}}</h1>   <!-- 插值表达式 -->
</div>
```

**核心思想**：**数据变了，页面自动变**。不用再 `document.querySelector(...).innerHTML = ...`——昨天手写的 DOM 操作，Vue 全接管了。

三件事：`createApp(对象)` → 对象里有 `data()` 返回数据 → `.mount('#app')` 挂载到哪个 div。

### 2) 五个常用指令（P26–28）

| 指令 | 简写 | 作用 |
|---|---|---|
| `v-for` | — | 循环渲染列表，`v-for="(e, index) in empList" :key="e.id"` |
| `v-bind` | `:` | 把变量绑到属性上，`:src="e.image"` |
| `v-if` / `v-show` | — | 条件显示 |
| `v-model` | — | **双向绑定**，输入框和变量互相同步 |
| `v-on` | `@` | 绑定事件，`@click="resetForm"` |

**`v-if` vs `v-show` 的区别**（面试常问）：
- `v-if`：条件不成立时元素**真的被销毁/重建**，切换有开销
- `v-show`：只是 `display: none`，元素还在，切换便宜
- 频繁切换用 `v-show`，一次性条件渲染用 `v-if`

**`@submit.prevent`**：阻止表单默认提交（否则页面会刷新跳转）。这是 Vue 里代替 `return false` 的写法。

**`:key`**：`v-for` 必须加，帮 Vue 识别每一行，不加会有渲染 bug 且控制台报警告。

### 3) 案例：员工列表（常用指令版）

```javascript
createApp({
  data() {
    return {
      searchForm: { name: '', gender: '', position: '' },   // 封装查询条件
      empList: [ {...}, {...}, {...} ]                       // 员工数据
    }
  },
  methods: {
    search() { console.log(this.searchForm) },
    resetForm() { ... }
  }
})
```

几个实用细节（写在代码注释里）：
- **插值表达式 `{{ }}` 不能出现在标签属性里**，属性要用 `:属性名="变量"`
- `type="reset"` **不会触发 `input` 事件**，`v-model` 收不到，所以清空按钮改用 `@click` 手动清数据
- 三元表达式可以写在插值里：`{{e.gender == 1 ? '男' : '女'}}`

### 4) Ajax / Axios（P29–30）

```javascript
// GET
axios.get('url').then(res => { console.log(res.data) })

// POST，第二个参数是请求体
axios.post('url', 'id=1').then(res => { console.log(res.data) })
```

**Axios 是 Ajax 的封装库**，本质还是「不刷新页面就向服务器要数据」，返回 JSON。

### 5) Vue + Axios 组合（异步交互版）

```javascript
methods: {
  async search() {
    const res = await axios.get('https://web-server.itheima.net/emps/list', {
      params: { name: ..., gender: ..., job: ... }   // 用 params 传参，axios 自动 URL 编码
    })
    this.empList = res.data.data      // 赋值给 data 里的变量，页面自动更新
  }
},
mounted() {
  // 页面加载完成后自动调用
}
```

要点：
- **`async` / `await`**：让异步代码按同步方式书写，函数仍是异步的、不阻塞主线程
- **`params`** 传查询参数，axios 自动 URL 编码
- **`this.empList = ...`** 赋值后页面自动刷新——这就是 Vue 双向绑定的价值
- **`mounted()`** 是生命周期钩子，页面挂载完成后执行，适合初始加载数据

## 没完成的

- **力扣 232 用栈实现队列**：只看懂了两栈的思路和例子，代码没写
- **力扣配额 2/5 → 仍 2/5**，剩 11.14、11.15 两天要清 3 题

## 计划调整

- 前端（HTML/CSS → JS → Vue → Ajax）**全部学完**，明天开始后端
- 力扣进度吃紧：明天（休息日，不健身，时间充裕）**至少做 2 题**，否则周日清不完

## 明天计划（11.14 周六 · week08 Day06）

> 休息日不健身，时间充裕

- [ ] **P31–34** Maven（介绍 / 概述与安装 / IDEA 集成 / 依赖管理），原速 1h29m
- [ ] **P35–39** 单元测试（概述 / 断言与注解 / 企业规范与 AI 生成 / 依赖范围 / 常见问题），原速 1h34m
- [ ] **力扣 2 题**（2/5 → 4/5）：232 用栈实现队列（已有的思路）+ 每日温度或最小栈
- [ ] 当天 commit：第8周Day06笔记
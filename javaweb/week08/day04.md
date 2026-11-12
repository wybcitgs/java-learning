# 第 8 周 Day 04 - 2026.11.12

> JS 章节一次看完（P17–24），外加 2 道力扣

## 今天学了（P17–24 JS 核心语法）

代码在 `E:\javaweb\代码\JS-Vue\`，01–11 全部过了一遍：引入方式、基础语法、数据类型、函数、自定义对象与 JSON、DOM、事件监听、员工列表案例、常见事件、模块化（加 `js/demo.js`、`utils.js`、`eventDemo.js`）。

### 1) 三种引入方式
```html
<!-- 内部：建议放 body 底部，避免阻塞页面加载 -->
<script> alert('hello') </script>

<!-- 外部：src 引入，推荐 -->
<script src="./js/demo.js"></script>

<!-- 行内：写在 onclick 属性里，不推荐 -->
```

### 2) 变量与类型（和 Java 最大的区别）
```javascript
let a = 10
a = "hello world"   // ✅ 不报错，类型随便变
a = true            // ✅ 又变 boolean

const b = 100
// b = 200          // ❌ 常量不能重新赋值
```

**三种输出**：`alert()` 弹窗 / `console.log()` 控制台 / `document.write()` 写入页面（不常用）

**数据类型与 `typeof`**：
| 值 | `typeof` |
|---|---|
| `10`、`1.5` | `number`（**不分整数小数**） |
| `"Hello"` / `'JS'` / `` `JavaScript` `` | `string`（三种引号都行） |
| `true` / `false` | `boolean` |
| `null` | **`object`**（历史遗留 bug，面试常问） |
| 只声明没赋值 | `undefined` |

**模板字符串**（反引号）：
```javascript
console.log(`我是${name}，我今年${age}岁`)   // 变量用 ${} 包裹
```

### 3) 三种函数写法
```javascript
function add(a, b) { return a + b }          // 具名函数
let add = function(a, b) { return a + b }    // 函数表达式
let add = (a, b) => { return a + b }         // 箭头函数
```

### 4) 自定义对象与 JSON
```javascript
let user = {
  name: '张三',
  age: 18,
  sing() { alert('我会唱歌') }   // 方法简写（对象里的 this 问题，箭头函数不适用）
}

JSON.stringify(user)              // JS 对象 → JSON 字符串
JSON.parse('{"name":"张三"}')      // JSON 字符串 → JS 对象
```
- **JSON 的键和字符串值都必须用双引号**
- 对象里的方法**不建议用箭头函数**（`this` 指向不同）

### 5) DOM 操作
```javascript
document.querySelector('#title1')      // 选第一个匹配的（CSS 选择器写法）
document.querySelectorAll('h1')        // 选全部，返回 NodeList，用下标访问
element.innerHTML = '新内容'            // 改元素内容
```

### 6) 事件监听
```javascript
// 推荐：可以绑多个监听
document.querySelector('#btn1').addEventListener('click', function() {
  alert('试试就试试')
})

// 早期写法：只能绑一个，后绑的覆盖前面的
document.querySelector('#btn2').onclick = function() { ... }
```

**九种常见事件**（案例里全试了一遍）：

| 事件 | 触发时机 |
|---|---|
| `click` | 鼠标点击 |
| `mouseenter` / `mouseleave` | 鼠标移入 / 移出 |
| `keydown` / `keyup` | 键盘按下 / 抬起 |
| `blur` / `focus` | 失去 / 获得焦点 |
| `input` | 用户输入 |
| `submit` | 表单提交 |

### 7) 模块化（P24）
```html
<script src="js/eventDemo.js" type="module"></script>
```
```javascript
// utils.js
export function Print(msg) { console.log(msg) }

// eventDemo.js
import { Print } from './utils.js'
```
**必须加 `type="module"`**，否则 `import` 报错。

### 8) 案例：员工列表行悬停变色（`09`）
```javascript
const rows = document.querySelectorAll('tbody tr')
rows.forEach(row => {
  row.addEventListener('mouseenter', () => row.style.backgroundColor = '#f2e2e2')
  row.addEventListener('mouseleave', () => row.style.backgroundColor = '#fff')
})
```
这就是上周 CSS 里 `tr:hover` 的 JS 写法——**同一个效果，两种实现方式**。

## 今天的两道力扣（配额 0/5 → 2/5）

### 20. 有效的括号 ✅ `3ms，击败 77.50%`
```java
Deque<Character> st = new ArrayDeque<>();
for (char c : s.toCharArray()) {
    if (c == '(') st.push(')');
    else if (c == '[') st.push(']');
    else if (c == '{') st.push('}');
    else if (st.isEmpty() || st.pop() != c) return false;
}
return st.isEmpty();
```
- **新学：栈 `push` / `pop` / `peek` / `isEmpty`**（之前只记过"后进先出"这个概念）
- 关键一行：`st.isEmpty() || st.pop() != c` —— **短路顺序不能反**，反了空栈时会抛 `NoSuchElementException`
- 踩坑：`=` 写成赋值、`peek()` 参数写错、`else if` 分支写重复

### 1047. 删除字符串中的所有相邻重复项 ✅
**版本一：`Deque` 当栈** `18ms，击败 72.73%`
```java
if (!st.isEmpty() && st.peek() == c) st.pop();
else st.push(c);
// 最后 for-each 遍历栈 + StringBuilder + reverse
```

**版本二：`StringBuilder` 当栈** `11ms，击败 88.52%`
```java
StringBuilder stack = new StringBuilder();
for (char c : s.toCharArray()) {
    int len = stack.length();
    if (len > 0 && stack.charAt(len - 1) == c) stack.deleteCharAt(len - 1);
    else stack.append(c);
}
return stack.toString();
```
- **认知**：栈是**逻辑结构**（只能从一端进出），不是某个类。`StringBuilder` 底层是"char 数组 + 长度"，`append` / `deleteCharAt(len-1)` 就是压栈 / 弹栈
- 快的两个原因：**省掉 `Character` 装箱**、少一次 `reverse` 遍历
- `reverse()` 是 `StringBuilder` 的方法，**`String` 没有**

## 没完成的

- **P25–30**（Vue 常用指令 + Ajax）→ 明天
- **力扣 3 题**（配额 2/5，剩 11.13–11.15 三天）

## 计划调整

- 今天原计划 P17–24 + 1 题力扣，实际**全部完成并多做 1 题**（2/5）
- 前端最后一块（Vue + Ajax）明天收尾，之后进入 Maven / SpringBootWeb（真正的后端内容）

## 明天计划（11.13 周五 · week08 Day05）

- [ ] **P25–28** Vue 常用指令：`v-for` / `v-bind` / `v-if` / `v-show` / `v-model` / `v-on`（原速 1h36m，1.25 倍速约 1h17m）
- [ ] **P29–30** Ajax 入门 + 案例（原速 50m）
- [ ] 力扣 1 题（2/5 → 3/5）
- [ ] 当天 commit：第8周Day05笔记
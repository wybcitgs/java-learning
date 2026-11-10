# 第 8 周 Day 02 - 2026.11.10

> 状态：**有点懈怠**，只看了 P10–13，原计划 P10–16 未完成，力扣 0 题

## 今天学了（P10–13）

代码换到了新目录：`E:\javaweb\HTML-CSS\`（跟着阶段走，原来是 `E:\HTML-CSS`）

### P10 整体布局（`08央视新闻-整体布局.html`）
- 页面内容用 `width: 70%` + `margin: 0 auto` 实现**水平居中**
- 布局本质：**用 `div` 切块，再用 CSS 定尺寸和位置**

### 盒子模型（`09盒子模型.html`）
- `width` / `height` **默认是内容区域的宽高**，不含 padding、border
- 四层从内到外：**content → padding → border → margin**
- `padding: 30px` 四周内边距；`border: 10px solid red` 边框
- **`box-sizing`**：
  - `content-box`（默认）：宽高只算内容，实际占位 = width + padding*2 + border*2
  - `border-box`：宽高**包含** padding 和 border，方便按尺寸设计 —— **实际开发几乎都用这个**
- `margin: 0 auto` = 上下 0、左右自动 → **块级元素水平居中**

### `div` vs `span`
- `div`：**块级元素，独占一行**
- `span`：**行内元素，一行可以放多个**

### flex 布局（`11flex布局.html`）
```css
.box { display: flex; }
```
- **主轴对齐** `justify-content`：
  - `flex-start` 从头排 / `flex-end` 从尾排 / `center` 居中
  - `space-around` 每个元素两侧等距
  - `space-between` 两端贴边、中间等距
  - `space-evenly` 所有间隔完全相等
- **交叉轴对齐** `align-items`：`flex-start` / `flex-end` / `center` / `stretch`

### tlias 顶部导航栏（`10Tlias案例-顶部导航栏.html`）
- 结构：`.container` 居中容器（1170px + `margin: 0 auto`）→ `.header`（flex）
- `justify-content: space-between` 让**标题靠左、退出登录靠右**
- `align-items: center` 让两者**垂直居中**
- 其他：`letter-spacing` 字间距、`border-left` 分隔竖线、`:hover` 悬停变色、`cursor: pointer`

### 表单标签（`12表单标签.html`）
```html
<form action="/save" method="post"> ... </form>
```
- `action`：数据提交的地址
- `method`：
  - **`get`（默认）**：数据拼在 URL 后 → **不安全**（隐私数据可见）、数据量大时 URL 过长
  - **`post`**：数据放在请求体 → **安全、可传大数据量**
- **表单项必须有 `name` 属性**，否则采集不到数据

### 表单项标签（`13表单项标签.html`）
`input` 的 `type` 一览：

| type | 作用 |
|---|---|
| `text` | 文本框 |
| `password` | 密码框 |
| `radio` | 单选框（**同组 name 必须相同**才能互斥） |
| `checkbox` | 复选框（name 相同也可多选） |
| `file` | 文件上传 |
| `date` / `time` / `datetime-local` | 日期 / 时间 / 日期时间 |
| `button` / `reset` / `submit` | 普通按钮（不提交）/ 重置 / 提交 |

其他表单项：
- `select` + `option`：下拉框，`selected` 默认选中，**第一个占位项 `value=""`**
- `textarea`：文本域，`cols` / `rows` 控制大小
- **`label for="id"`**：点击文字也能选中对应控件，**`for` 必须等于目标控件的 `id`**

**`radio` / `checkbox` 必须写 `value`**，否则提交上去只有 `on`。

## 今天的代码

| 文件 | 对应 |
|---|---|
| `08央视新闻-整体布局.html` | width 70% + margin auto 居中 |
| `09盒子模型.html` | padding / border / box-sizing 对比 |
| `10Tlias案例-顶部导航栏.html` | flex + space-between + 居中容器 |
| `11flex布局.html` | justify-content 六种取值测试页 |
| `12表单标签.html` | form / get vs post / name |
| `13表单项标签.html` | 全部 input type + label + select + textarea |

## 没完成的

- **P14–16**（tlias 搜索表单区域 / 底部版权区域 / 课程总结）→ 明天补
- **力扣 0 题**（week08 配额 0/5，周日 11.15 前要清 5 题）

## 计划调整

- 今天状态不好，实际只推进 4 集（约 1h25m），原计划 7 集（2h30m）
- **按用户判断调整前端强度**：HTML/CSS 这类"AI 一秒能生成"的内容，重点从"手写"转向"能审"——
  - 必懂：`div`/`span` 区别、盒模型与 `box-sizing`、flex 两个轴、`form`/`input`/`label` 的对应关系
  - 不必记：具体像素值、颜色、字体大小
- week08 力扣进度吃紧：剩 5 天要清 5 题，**明天起每天 1 题**

## 明天计划（11.11 周三 · week08 Day03）

- [ ] **P14–16**（搜索表单区域 / 底部版权区域 / 课程总结），原速 53m
- [ ] 力扣 1 题（0/5 → 1/5）
- [ ] 当天 commit：第8周Day03笔记
# 第 8 周 Day 03 - 2026.11.11

> 状态：今天很忙，视频看完了（P14–16），**力扣没做**（配额仍 0/5）

## 今天学了（P14–16 + 整合完整页）

代码在 `E:\javaweb\HTML-CSS\`：

| 文件 | 内容 |
|---|---|
| `14Tlias案例-搜索表单区域.html` | flex 横排搜索框 |
| `15Tlias案例-表格数据展示.html` | 员工列表表格 |
| `16Tlias案例-底部版权区域.html` | 页脚 |
| `17Tlias案例.html` | **整合版**：header + 搜索 + 表格 + 页脚 拼成完整页 |

### 1) 搜索表单区域（P14）

```css
.search-form {
  display: flex;
  align-items: center;        /* 标签和控件高度不同也能对齐 */
  padding: 20px 0;
}
.search-form .buttons {
  margin-left: auto;          /* 吃掉剩余空间，把按钮组推到最右 */
  display: flex;
  gap: 12px;
}
```

要点：
- **`display: flex`** 让 label / input / select 排成一行
- **`margin-left: auto`** 是「推到最右」的常用写法（比 `justify-content: space-between` 更灵活，因为它只作用于该元素）
- **`gap`** 直接给子元素之间留间距，不用逐个写 `margin-right`
- `white-space: nowrap` 防止「姓名」「性别」这类短标签被挤到换行
- `:focus { border-color: ... }` 输入框聚焦时高亮
- 按钮 `type="submit"` 提交表单、`type="reset"` 清空表单

### 2) 表格数据展示（P15）

```css
.table {
  width: 100%;
  border-collapse: collapse;   /* 合并相邻边框，否则会出双线 */
  table-layout: fixed;         /* 固定列宽，由表头决定 */
}
.table tbody tr:hover { background-color: #fafafa; }   /* 行悬停高亮 */
.avatar { object-fit: cover; }                          /* 头像固定尺寸不被拉伸 */
```

结构：
```html
<table>
  <thead><tr><th>姓名</th>...</tr></thead>
  <tbody><tr><td>令狐冲</td>...</tr></tbody>
</table>
```

要点：
- **`border-collapse: collapse`** 去掉单元格之间的双线——表格必写
- **`table-layout: fixed`** 让列宽由第一行（`th`）决定，不随内容跳动
- `th` / `td` 用 `text-align: center` + `vertical-align: middle` 居中
- `object-fit: cover` 让头像**裁剪填满**指定尺寸，不拉伸变形

### 3) 底部版权区域（P16）

```css
.footer {
  padding: 18px 0;
  background-color: #dcdcdc;
  color: #fff;
  line-height: 26px;      /* 两行文字的行距 */
  text-align: center;     /* 内容居中 */
}
```

最简单的一集：背景色 + 文字居中 + 行距。

### 4) 整合页（`17Tlias案例.html`）

把前三天的东西拼成一个完整页面：

```
.container（1170px 居中）
  ├── .header     flex + space-between：logo 靠左、退出登录靠右
  ├── .search-form flex + margin-left:auto：搜索条件一行，按钮靠右
  ├── .table      border-collapse + table-layout:fixed
  └── .footer     灰底居中版权
```

**这就是所有后台管理页面的通用骨架**：头部 → 搜索区 → 数据表格 → 分页/页脚。以后换成 ElementPlus 组件，结构不变。

## 今天用到的可复用技巧（记这几个就够）

| 技巧 | 用途 |
|---|---|
| `margin: 0 auto` + 固定宽度 | 页面上内容区水平居中 |
| `display: flex` + `align-items: center` | 一行内多个元素垂直居中对齐 |
| `margin-left: auto` | 把某个元素推到容器最右 |
| `gap` | 子元素之间统一间距，代替一堆 margin |
| `border-collapse: collapse` | 表格边框合并 |
| `object-fit: cover` | 图片裁剪填满，不变形 |
| `* { margin:0; padding:0; box-sizing:border-box }` | 每个页面开头必写的重置 |

## 没完成的

- **力扣 0 题**（week08 配额 **0/5**，剩 11.12–11.15 四天要清 5 题，压力大）
- 今天的计划里「P17」没看——P17 是 **JS 第一集（课程介绍）**，属新章节，明天开始

## 计划调整

- 前端 HTML/CSS 基础**全部结束**（P1–16）
- 力扣进度落后：**明天起每天 1 题，11.14 周六补 2 题**，否则周日复盘前清不完
- 明天进入 **JS 章节**（P17–24，原速 2h15m）

## 明天计划（11.12 周四 · week08 Day04）

- [ ] **P17–24** JS 核心语法：引入方式 / 变量与数据类型 / 函数 / 自定义对象与 JSON / DOM / 事件监听 / 模块化（原速 2h15m，1.25 倍速约 1h48m）
- [ ] 力扣 1 题（0/5 → 1/5）
- [ ] 当天 commit：第8周Day04笔记
# 第 8 周 Day 01 - 2026.11.09

> JavaWeb 阶段开工。week08 = 11.9–11.15（自然周），复盘日改为周日

## 今天学了（Tlias 课程 P1–9，前端 Web 基础）

### HTML 骨架
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>标题</title>
</head>
<body>...</body>
</html>
```
- `<!DOCTYPE html>` 声明文档类型
- `meta charset="UTF-8"` 字符集，不写中文会乱码
- `meta name="viewport"` 移动端显示宽度与缩放

### 常用标签
| 标签 | 作用 | 关键属性 |
|---|---|---|
| `h1` | 一级标题 | — |
| `a` | 超链接 | `href` 目标地址；`target="_blank"` 新窗口 / `_self` 当前窗口（默认） |
| `img` | 图片 | `src` / `alt` / `width` / `height`（宽高设一个，另一个等比缩放） |
| `br` | 换行 | 单标签 |
| `p` | 段落 | — |
| `video` | 视频 | `src` / `controls` / `autoplay` / `width` |
| `audio` | 音频 | `src` / `controls` / `autoplay` / `loop` |
| `strong` / `b` | 加粗（strong 有语义） | — |

**路径三种写法**：绝对磁盘路径（不推荐）/ 绝对网络路径 / **相对路径**（`./` 当前目录可省略、`../` 上一级）

### CSS 三种引入方式
1) **行内样式**：`<span style="color: grey;">`（写在标签上，优先级最高、复用性最差）
2) **内部样式**：`<head>` 里写 `<style> ... </style>`
3) **外部样式**：`<link rel="stylesheet" href="css/news.css">`（推荐，可复用）

### 颜色四种表示
| 写法 | 例子 |
|---|---|
| 关键字 | `grey` |
| RGB | `rgb(128, 128, 128)` |
| RGBA | `rgba(128, 128, 128, 0.5)`（最后一位透明度） |
| 十六进制 | `#b2b2b2`（六位：红红绿绿蓝蓝） |

### 选择器与优先级
- **元素选择器** `span { }`
- **类选择器** `.cls { }`
- **id 选择器** `#time { }`
- **优先级：id > 类 > 元素**；行内样式高于以上三者

### 今天用到的样式
```css
color: #b2b2b2;          /* 文字颜色 */
text-decoration: none;   /* 去掉超链接下划线 */
line-height: 2;          /* 行高 2 倍 */
text-indent: 2em;        /* 首行缩进 2 个字宽 */
```

### 注释
- HTML：`<!-- 注释内容 -->`
- CSS：`/* 注释内容 */`

## 环境插曲（占用约 2 小时）

把 Claude 桌面版调通了，途中解决四个关卡：

| 关卡 | 解决方式 |
|---|---|
| 地区锁（App unavailable in region） | 干净重装，不预先接 CC Switch（`3p` 模式会触发地区校验） |
| 主程序缺失（Host Claude Code binary not available） | 从 **npm 镜像**下 `@anthropic-ai/claude-code-win32-x64@2.1.317`，放到 `AppData\Local\Claude-3p\claude-code\2.1.317\a33aa5518497\`，并按官方格式写 `.verified`（清单校验值）和 `.payload`（sha256+size） |
| haiku 模型未映射 | `~/.claude/settings.json` 补 `ANTHROPIC_DEFAULT_HAIKU_MODEL_NAME` |
| 模型带 `[1m]` 后缀被网关拒 | CC Switch 里关掉供应商的「支持 1M 上下文」 |

**汉化补丁装不上**：`javaht/claude-desktop-zh-cn` 不支持 MSIX 商店版（WindowsApps 受系统完整性保护，接管所有权也写不进去）。已验证失败时**没有留下任何改动**（asar 时间戳未变、无备份目录、目录所有者仍为 SYSTEM）。中文界面需要官网 exe 版，而官网 CDN `downloads.claude.ai` 当前网络连不上。

## 没完成的

- **力扣 0 题**（week08 配额 1/5，周日 11.15 前清零）
- P10–16（明天）

## 计划调整

- 今天实际学习时间被桌面版折腾掉约 2 小时，视频完成 P1–9（原计划 P1–8），**略超原计划进度**
- 前端按 1.25 倍速策略执行，HTML/CSS 不需要记住所有属性，能用能改即可

## 明天计划（11.10 周二 · week08 Day02）

- [ ] **P10–16**：央视新闻整体布局 + tlias 案例（顶部导航栏 / 表单标签 / 表单项标签 / 搜索表单区域 / 底部版权区域）+ 课程总结
- [ ] 原速约 2h35m，1.25 倍速约 2h05m
- [ ] 力扣 1 题（配额 1/5 → 2/5）
- [ ] 当天 commit：第8周Day02笔记
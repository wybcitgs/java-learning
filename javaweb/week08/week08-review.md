# Week08 周复盘（2026.11.09 – 11.15）

本周是 JavaWeb 阶段第一周，也是**第一个自然周**（周一–周日，复盘日改为周日）。覆盖：前端基础（HTML/CSS/JS/Vue/Ajax）+ Maven + 单元测试，共 P1–39。

**验收方式**：闭卷自测 20 题（**16 对 / 4 半对 / 0 错**）。题目覆盖 HTML/CSS、JS、Vue/Ajax、Maven 四块；单元测试（P35–39，当天新学）未进本次自测题，内容已按实际代码并入下方「本周所学」。

## 本周所学（对照 PLAN）

| 内容 | 视频 | 状态 |
|---|---|---|
| HTML/CSS 基础：标签、样式、选择器、央视新闻案例 | P1–9 | ✅ 11.09 |
| 整体布局、盒模型、flex、tlias 顶部导航/表单 | P10–13 | ✅ 11.10 |
| tlias 搜索表单 / 表格 / 页脚 + 整合完整页 | P14–16 | ✅ 11.11 |
| JS：引入方式、数据类型、函数、对象与 JSON、DOM、事件、模块化 | P17–24 | ✅ 11.12 |
| Vue 快速入门 + 五个常用指令 | P25–28 | ✅ 11.13 |
| Ajax / Axios + Vue 异步交互 | P29–30 | ✅ 11.13 |
| Maven：简介、安装、IDEA 集成、依赖管理、生命周期、排除依赖 | P31–34 | ✅ 11.14 |
| 单元测试：入门、断言、注解、参数化、企业规范、依赖范围、常见问题 | P35–39 | ✅ 11.15 |
| ⏳ SpringBootWeb 入门 + HTTP + 三层架构 + IOC/DI | P40–49 | **未开始 → week09** |

**前端部分（P1–30）全部完成**，Maven 与单元测试（P31–39）完成，后端框架内容顺延到 week09。

## 假会清单（重点，动手时对照）

### 1) HTML 骨架不完整 半对
- **我的误区**：只记得 `html / head / body / title`
- **正确**：标准骨架必须有 `<!DOCTYPE html>` + `<meta charset="UTF-8">`（不写中文乱码）+ `<meta name="viewport">`（移动端适配）
- **待复测**：week09 写 SpringBoot 页面时顺带过

### 2) `margin: 0 auto` 的适用范围 半对
- **我的误区**：答「对行内元素也有效」
- **正确**：**只对块级元素有效**。`span` 这类行内元素没有固有宽度，设 `margin: auto` 无效；要居中得先 `display: block` 或改用 flex 的 `justify-content: center`
- **待复测**：week09

### 3) `let` / `const` / `var` 的准确定义 半对
- **我的误区**：`let` 是"弱类型"、`var` 是"字符"
- **正确**：`let` = **可重新赋值的变量**；`const` = 不可重新赋值的常量（对象内容仍可改）；`var` = 老写法，有变量提升坑，**不要用**。类型随便变是 JS 本身的特性，与三者无关
- **待复测**：week09

### 4) 模板字符串的好处 半对
- **我的误区**：只说"更简洁"
- **正确**：关键是**支持换行**、**不用一堆 `+` 拼接**，`${}` 里能直接放表达式
- **待复测**：week09

## 扎实的（不用再看）

- **HTML/CSS**：div 块级 vs span 行内、盒子模型四层、`box-sizing` 两种模式、CSS 三种引入与优先级、三种选择器优先级、flex 两个轴
- **JS**：`typeof null === "object"`、三种函数写法、`JSON.stringify` / `JSON.parse`、`querySelectorAll` 返回 NodeList、`addEventListener` 可绑多个
- **Vue**：五个指令（`v-for` / `v-bind` / `v-if` / `v-model` / `v-on`）、`v-if` vs `v-show` 的区别与场景、`@submit.prevent`
- **Axios**：`axios.get(url).then(res => res.data)`、`params` 传参、`async/await` + `mounted()`
- **Maven**：两个核心问题（依赖管理 + 构建）、仓库三层、生命周期五阶段及级联执行、依赖传递、`exclusions`

## 认知突破

1. **Vue 的数据驱动 vs 手动 DOM**：上周写 `document.querySelector(...).innerHTML = ...`，这周变成 `this.empList = res.data.data` 页面自己刷新——**从"操作页面"变成"操作数据"**，这是前端框架的核心价值
2. **依赖传递的量级**：只声明 `spring-context` 一个依赖，`dependency:tree` 自动带进 `spring-aop` / `spring-beans` / `spring-core` / `spring-jcl` / `spring-expression`——**一个坐标可能拉进十几个 jar**
3. **单元测试的边界值思维**：`Userservice4Test` 里一个 `getAge` 写了 5 个用例——`null` / 空串 / 长度不足 / 长度超过 / 正常。**正常路径只占一个，其余全是边界**——这才是测试的价值所在
4. **`@BeforeEach` 抽公共前置**：`userService` 的初始化从每个测试方法里提到 `setUp()`，测试方法只剩"测什么"
5. **`@DisplayName` 让报告可读**：测试失败时看到的是「测试获得年龄 -空串」而不是 `testGetAgeAssert2`
6. **依赖范围 `test`**：`<scope>test</scope>` 让 JUnit 只参与测试编译和运行，**不打进最终 jar**——打包产物不含测试依赖
7. **Maven 生命周期是级联的**：`mvn install` 自动跑 `compile → test → package → install`，`clean` 是独立另一条线，所以常写 `mvn clean package`

## 本周差距（对照 PLAN）

- **知识线**：PLAN 本周排到 P49，实际完成 **P1–39**（差 P40–49 SpringBootWeb 入门）→ week09 补
- **代码量**：
  - HTML/CSS：入门 7 个 + tlias 案例 6 个（含整合页）
  - JS：11 个 HTML + `demo.js` / `utils.js` / `eventDemo.js`
  - Vue/Ajax：4 个（快速入门 / 常用指令 / Axios 入门 / 异步交互）
  - Maven + 单元测试：1 个 Maven 项目 + 5 个测试类
- **力扣**：**5/5 已清零**（20 / 1047 / 232 / 155 / 225），连续第 5 周满配 🏁
- **博客**：**本周欠 1 篇**（第 8 篇），上次发布是 11.08 的 JavaSE 阶段总结
- **环境**：Maven 3.9.4 装好、阿里云镜像配通、本地仓库改到 `.m2\repository`、`del.bat` 清理工具就位
- **进度**：JavaWeb 39/200 集，约 17h / 76h

## week09 预告（11.16 – 11.22，自然周）

| 日期 | 内容 |
|---|---|
| 11.16 周一 | P40–42 SpringBootWeb 入门 + HTTP 协议（约 1h） |
| 11.17 周二 | P43–45 请求/响应协议 + 案例（约 1h25m） |
| 11.18 周三 | P46–49 分层解耦：三层架构 + IOC/DI ⭐（约 1h20m） |
| 11.19 周四 | P50–55 MySQL 概述 + DDL/DML（约 2h50m） |
| 11.20 周五 | P56–61 MySQL 多表 + 约束 + 事务（约 2h） |
| 11.21 周六 | P62–64 JDBC + 补漏 | 约 1h05m |
| 11.22 周日 | **week09 复盘** + 力扣清算 + 掘金第 8 篇 |

**待办清单**
- [ ] P40–49 SpringBootWeb 入门（本周欠账）
- [ ] 力扣 5 题（周日 11.22 前清零）
- [ ] **掘金第 8 篇**（欠账，本周必须发）
- [ ] 假会清单 4 项：week09 写代码时顺带验证
- [ ] MySQL：学校课程同步跟，视频作补充
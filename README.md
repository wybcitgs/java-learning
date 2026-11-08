# java-learning

My Java learning journey 🚀

---

## 学习路线（v3 · 2026.09.30 修订）

- **起始日**：2026.09.17
- **目标**：2027 年 3 月开始投递 Java 后端实习
- **时间投入**：上午健身，学习从 15:00 起约 6h
- **课程结构**（详见 [PLAN.md](PLAN.md)）：
  - 📺 主线：黑马 2026 版 AI+Java（BV1TJxCzSEEZ，149 集）
  - 📺 补充：黑马零基础 上部（BV17F411T7Ao）+ 下部（BV1yW4y1Y7Ms），按需点名筛选

## 目录结构（按阶段分层）

```
javase/                 第一阶段 JavaSE（week00 – week08）
  week00-setup/ … week07-map-stream-exception/
  finale/                收尾期（11.6 – 11.8，3 天，**不计入周号**）
javaweb/                第二阶段 JavaWeb（week08–11，11.9 开始，自然周）
spring/                 第三阶段 Spring Boot + 项目（尚未开始，week12–21）
course-catalog/         四套课分集目录与每集时长（跨阶段共用）
PLAN.md                 每日/每周计划
```

---

### 第一阶段：JavaSE（week00–week08，9.17 – 11.8）

> 主线课看到 P149，上下部只看点名内容；原理类内容看一遍留印象

| 周 | 内容 | 状态 |
|---|---|---|
| week00–01 | 环境搭建、基础语法、数组、方法 | ✅ |
| week02 | 面向对象基础（第 8 章）+ 原理篇 + 学生管理系统 v0.1 | ✅ |
| week03 | static、final、枚举 + 继承、权限修饰符 | ✅ |
| week04 | 多态、抽象类、接口、内部类 + String、StringBuilder | ✅ |
| week05 | ArrayList + 游戏项目（P146–149 当时未跟，已补） | ✅ |
| week06 | 集合进阶（Collection/List/Set）、泛型、lambda | ✅（Map/Stream/异常顺延 week07） |
| week07 | Map/HashMap、Stream、异常、时间类、BigInteger + 周复盘 | ✅ **收官**（视频全清，力扣 5/5，掘金第 6 篇） |
| 收尾期 | JavaSE 总复习与模拟面试 → **11.8 完结**（`javase/finale/`，不计入周号） | 进行中 |

> ✅ 欠账已清零：**P146–149（Math / System + Object / 包装类 / BigInteger-BigDecimal）**——week05 存档未跟，week07（11.3–11.4）按主线第 14 章补完
> 📅 **11.9 起进入 JavaWeb，学习周切换为自然周（周一–周日）**
> 📚 四套课分集目录与每集时长：[course-catalog/](course-catalog/)（主线 149 / 上部 200 / 下部 200 / JavaWeb 200）

> ⏸️ JavaSE 阶段不学：IO 流、多线程、反射、网络编程 Socket、正则、斗地主项目
> → 前三项**排到第三阶段开课前集中补**（见 PLAN「开课前补齐」），Socket 永久跳过

---

### 第二阶段：JavaWeb（week08–11，11.9 – 12 月上旬）

> 📺 全网首发 AI+JavaWeb 开发入门 · Tlias 教学管理系统项目实战（BV1yGydYEE3H）
> **200 集，约 75h58m**；目录见 [course-catalog/javaweb-BV1yGydYEE3H.md](course-catalog/javaweb-BV1yGydYEE3H.md)

| 周 | 内容 |
|---|---|
| week08 | 前端基础（P1–30，1.25 倍速）+ Maven/单元测试（P31–39）+ SpringBootWeb 入门（P40–49）⭐ |
| week09 | MySQL + JDBC + MyBatis + 部门管理 + 多表查询（P50–92） |
| week10 | 员工管理 + 事务 + 文件上传 + 登录认证（P93–132） |
| week11 | AOP + 原理篇 + Maven高级 + Vue工程化 + **Tlias 案例（P160–181，核心）** |
| 后置 | Linux + Docker（P182–199，7h34m）→ 12 月上旬或实习前 |

- MySQL：依托学校课程同步跟学
- OS / 网络理论：面试前 1 周突击

---

### 第三阶段：Spring Boot + 项目实战（week12–21，12 月 – 2027 年 2 月中）

> 开课前约 3 天集中补：多线程（下部 P135–167）、IO 流（P74–134 精选）、反射（P191–197）

Maven / Spring → Spring Boot → MyBatis → RESTful → Redis → 项目实战 → 简历

**2027 年 3 月中投递** 🎯

---

## 每周固定动作（雷打不动）

| 项目 | 要求 |
|---|---|
| LeetCode | **周配额 5 题**（不绑死每天，周四复盘前清零）；三步法（自己憋 25min → 看题解 → 关掉重写）|
| 掘金博客 | 每周至少 1 篇，**学习周最后一天（周四）前发完**（技术笔记 / 面试题 / 项目复盘）|
| GitHub | 每天 commit，保持绿点 |
| 周复盘 | **学习周最后一天（周四）** 2 小时，回顾本周 + 调整下周计划 |

---

## 学习节奏（重要）

1. 开课前先核对 P 号，笔记里报集数
2. 看 1h 视频 + 写 1h 代码交替，练习优先
3. 欠账当周清——连续顺延 2 天 = 计划失效，当天重排

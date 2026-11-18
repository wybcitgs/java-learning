# 第 9 周 Day 03 - 2026.11.18

> P49 IOC/DI 详解 + MySQL P50–54，P55 只开了个头

## 今天学了

### 1) IOC / DI 详解（P49）

**四个分层注解，本质都是 `@Component`**：

| 注解 | 用在 | 语义 |
|---|---|---|
| `@Component` | 任意类 | 通用，交给 IOC 容器 |
| `@Service` | Service 层实现类 | 业务逻辑层 |
| `@Repository` | Dao 层实现类 | 数据访问层 |
| `@Controller` | 控制器 | 表现层（`@RestController` = `@Controller` + `@ResponseBody`） |

**用法上完全等价**，区别只在语义可读性。规范做法是**按层用对应注解**，一眼能看出这个类属于哪一层。

**Bean 名称规则**：默认是**类名首字母小写**

```
UserServiceImpl   →  userServiceImpl
UserServiceImpl2  →  userServiceImpl2
```

也可以在注解里指定：`@Service("userService")`。

**为什么会有注入冲突**：当一个接口有多个实现类，容器里就有多个同类型的 bean，`@Autowired` 按类型找不到唯一的一个，启动就报错：

```
NoUniqueBeanDefinitionException: expected single matching bean but found 2
```

**三种解法**（今天的代码三种都写了）：

| 解法 | 写法 | 特点 |
|---|---|---|
| **`@Primary`** | 加在其中一个实现类上 | 设默认优先，**调用方不用改** |
| **`@Qualifier`** | `@Qualifier("userServiceImpl")` + `@Autowired` | 按 bean 名称精确指定 |
| **`@Resource`** | `@Resource(name = "userServiceImpl")` | JDK 自带（`javax.annotation`），按名称注入 |

```java
// 第三种写法（当前生效的）
@Resource(name = "userServiceImpl")
private UserService userService;
```

**三种选择建议**：
- 想让某个实现类"默认优先" → `@Primary`
- 只想在某处特指 → `@Qualifier`
- 不想依赖 Spring 的注解 → `@Resource`（JDK 标准）

### 2) MySQL 入门（P50–54）

**版本确认**：本机已装 **MySQL 8.0.46 Community Server**，服务 `MySQL80` 正在运行，端口 3306。

**数据模型**（一定要建立这个层级概念）：

```
数据库（Database）→ 表（Table）→ 行/记录（Row）→ 列/字段（Column）
```

一个 MySQL 服务里可以有多个数据库，一个数据库里有多张表。

**DDL（数据定义语言）——操作库和表的结构**

| 操作 | SQL |
|---|---|
| 查询所有数据库 | `SHOW DATABASES;` |
| 创建数据库 | `CREATE DATABASE 库名;` |
| 删除数据库 | `DROP DATABASE 库名;` |
| 切换数据库 | `USE 库名;` |
| 查看当前库 | `SELECT DATABASE();` |
| 查询库下所有表 | `SHOW TABLES;` |
| 查看表结构 | `DESC 表名;` |

**建表语句的基本结构**：

```sql
CREATE TABLE 表名 (
    字段1 类型 [约束] [COMMENT '注释'],
    字段2 类型 [约束],
    ...
) COMMENT '表注释';
```

**常用数据类型**：

| 类型 | 用途 |
|---|---|
| `INT` / `BIGINT` | 整数（主键常用 BIGINT） |
| `VARCHAR(n)` | 变长字符串，**必须指定长度** |
| `CHAR(n)` | 定长字符串 |
| `DECIMAL(m,d)` | 精确小数（**金额必须用它**，不用 float/double） |
| `DATE` / `DATETIME` / `TIMESTAMP` | 日期 / 日期时间 / 时间戳 |
| `TEXT` | 长文本 |

**图形化工具**：Navicat / DataGrip / IDEA 自带 Database 面板——建表、看数据比敲命令直观，但**建表语句还是要知道怎么写**，因为项目里都是脚本化的。

## 今天的代码改动

在 `springboot-web-01` 里验证了多种注入方式：

```java
// UserServiceImpl2：第二个实现类，演示"一个接口多个实现"
@Service
public class UserServiceImpl2 implements UserService { ... }

// UserController：用 @Resource 按名称指定注入哪一个
@Resource(name = "userServiceImpl")
private UserService userService;
```

另外 `UserServiceImpl` 的 `@Component` 换成了 `@Service`——**分层注解的语义化用法**。

## 没完成的

- **P55–61 MySQL**（表操作续 / DML / 多表查询 / 事务等，约 2h45m）
- **力扣 0 题**（配额仍 2/5，剩 11.19–11.22 四天要清 3 题）

## 计划调整

- MySQL 有基础，按「速通」策略推进：语法类快过，**多表查询和事务放慢**
- 明天重点是 **P55–61** + 把 `springboot-web-01` 跑起来验证三层架构 + IOC 都正常
- 力扣剩 3 题 / 4 天，节奏正常，但别再拖到最后一天

## 明天计划（11.19 周四 · week09 Day04）

- [ ] **P55–61** MySQL：表操作续、DML（增删改）、DQL 基础查询、多表查询、事务（约 2h45m）
- [ ] 动手：在本机 MySQL 里建一张 `user` 表，写几条增删改查
- [ ] 力扣 1 题（2/5 → 3/5）
- [ ] 当天 commit：第9周Day04笔记
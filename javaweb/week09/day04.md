# 第 9 周 Day 04 - 2026.11.19

> MySQL 基础全章（P50–61）+ JDBC（P62–64）看完

## 今天学了（MySQL P50–61）

### 1) 数据模型与 DDL

**层级**：数据库 → 表 → 行/记录 → 列/字段

**库操作**：

```sql
SHOW DATABASES;                 -- 查所有库
CREATE DATABASE db01;           -- 建库
DROP DATABASE db01;             -- 删库
USE db01;                       -- 切换
SELECT DATABASE();              -- 当前库
```

**建表**：

```sql
CREATE TABLE tb_user (
    id       INT PRIMARY KEY AUTO_INCREMENT COMMENT '主键',
    username VARCHAR(20) NOT NULL UNIQUE COMMENT '用户名',
    name     VARCHAR(10) COMMENT '姓名',
    age      TINYINT UNSIGNED COMMENT '年龄',
    gender   CHAR(1) COMMENT '性别',
    create_time DATETIME COMMENT '创建时间'
) COMMENT '用户表';
```

**常用数据类型**：

| 类型 | 说明 |
|---|---|
| `INT` / `BIGINT` | 整数，主键常用 |
| `VARCHAR(n)` | 变长字符串，**必须指定长度** |
| `CHAR(n)` | 定长字符串 |
| `TINYINT UNSIGNED` | 0–255，适合年龄 |
| `DECIMAL(m,d)` | 精确小数，**金额必须用它** |
| `DATE` / `DATETIME` / `TIMESTAMP` | 日期 / 日期时间 / 时间戳 |

**约束**：

| 约束 | 作用 |
|---|---|
| `PRIMARY KEY` | 主键，唯一且非空 |
| `AUTO_INCREMENT` | 自增 |
| `NOT NULL` | 不能为空 |
| `UNIQUE` | 唯一 |
| `DEFAULT` | 默认值 |
| `FOREIGN KEY` | 外键 |

**表结构维护**：

```sql
DESC tb_user;                                  -- 查表结构
SHOW CREATE TABLE tb_user;                     -- 查建表语句
ALTER TABLE tb_user ADD COLUMN phone VARCHAR(11);  -- 加列
ALTER TABLE tb_user MODIFY COLUMN name VARCHAR(20); -- 改类型
ALTER TABLE tb_user CHANGE COLUMN phone tel VARCHAR(11); -- 改列名
ALTER TABLE tb_user DROP COLUMN tel;           -- 删列
DROP TABLE tb_user;                            -- 删表
```

### 2) DML：增删改

```sql
-- 插入
INSERT INTO tb_user (username, name, age) VALUES ('zhangsan', '张三', 20);
INSERT INTO tb_user (username, name) VALUES ('lisi','李四'), ('wangwu','王五');  -- 批量

-- 修改
UPDATE tb_user SET age = 21 WHERE username = 'zhangsan';

-- 删除
DELETE FROM tb_user WHERE id = 3;
```

⚠️ **`UPDATE` / `DELETE` 不写 `WHERE` 会作用到全表**——这是最危险的一条，实操时先写 `WHERE` 再写 `SET`。

### 3) DQL：五种查询

**基本查询**：

```sql
SELECT * FROM tb_user;
SELECT id, username, age FROM tb_user;
SELECT DISTINCT age FROM tb_user;                 -- 去重
SELECT username AS 用户名, age AS 年龄 FROM tb_user;  -- 别名
```

**条件查询**：

```sql
SELECT * FROM tb_user WHERE age > 20;
SELECT * FROM tb_user WHERE age BETWEEN 18 AND 30;
SELECT * FROM tb_user WHERE username LIKE '张%';      -- 模糊，% 任意多个
SELECT * FROM tb_user WHERE id IN (1, 2, 3);
SELECT * FROM tb_user WHERE age IS NULL;              -- 判空用 IS
SELECT * FROM tb_user WHERE age > 20 AND gender = '男'; -- 逻辑运算
```

**分组查询**：

```sql
SELECT gender, COUNT(*) FROM tb_user GROUP BY gender;
SELECT gender, AVG(age) FROM tb_user GROUP BY gender HAVING AVG(age) > 20;
```

- **`WHERE` 在分组前过滤行，`HAVING` 在分组后过滤组**
- 聚合函数：`COUNT` / `SUM` / `AVG` / `MAX` / `MIN`
- **`SELECT` 里只能出现分组字段和聚合函数**，其他字段在严格模式下报错

**排序与分页**：

```sql
SELECT * FROM tb_user ORDER BY age DESC;              -- 降序
SELECT * FROM tb_user ORDER BY age ASC, id DESC;      -- 多字段
SELECT * FROM tb_user LIMIT 0, 10;                    -- 第 1 页，每页 10 条
SELECT * FROM tb_user LIMIT 10, 10;                   -- 第 2 页
```

**分页公式**：`LIMIT (页码 - 1) * 每页条数, 每页条数`

### 4) 章节边界（避免误判进度）

今天到 P61 为止**只是单表操作**。后面还有：

- **多表查询**：P86–92（内连接 / 左外连接 / 子查询）
- **事务**：P101–103（ACID、COMMIT / ROLLBACK）

这两块是面试重点，不能跳，但也确实还没学。

### 5) JDBC（P62–64）

**依赖**：`mysql-connector-j` 8.0.33（新版驱动，旧版是 `mysql-connector-java`）

**六步流程**：

```java
// 1. 注册驱动
Class.forName("com.mysql.cj.jdbc.Driver");

// 2. 获取连接
String url = "jdbc:mysql://localhost:3306/web01";
Connection conn = DriverManager.getConnection(url, "root", "123456");

// 3. 获取执行对象
Statement stmt = conn.createStatement();

// 4. 执行 SQL
int rows = stmt.executeUpdate("update user set age = 25 where id = 1");  // 增删改，返回影响行数
ResultSet rs = stmt.executeQuery("select * from user");                   // 查，返回结果集

// 5. 处理结果
while (rs.next()) {
    rs.getInt("id"); rs.getString("username");   // 按列名取，也可以用列序号
}

// 6. 释放资源（后开的先关：rs → stmt → conn）
rs.close(); stmt.close(); conn.close();
```

**`Statement` vs `PreparedStatement`** ⭐ 面试必问：

| | Statement | PreparedStatement |
|---|---|---|
| SQL 写法 | 字符串直接拼接 | `?` 占位符 + 参数绑定 |
| 预编译 | 不预编译 | **预编译**，执行效率高 |
| SQL 注入 | **有风险**（拼接可被注入） | **防注入**（参数被当作值处理） |
| 使用建议 | 少用 | **一律用它** |

```java
String sql = "select * from user where username = ? and password = ?";
PreparedStatement ps = conn.prepareStatement(sql);
ps.setString(1, "daqiao");      // 索引从 1 开始，不是 0！
ps.setString(2, "123456");
ResultSet rs = ps.executeQuery();
```

**三个容易踩的点**：

1. **占位符索引从 1 开始**（`setString(1, ...)`），和数组从 0 开始不一样
2. **`executeUpdate` vs `executeQuery`**：增删改用前者（返回影响行数），查询用后者（返回 `ResultSet`）
3. **释放顺序**：先关 `ResultSet`，再关 `Statement`，最后关 `Connection`——**先开的最后关**

## 今天的力扣

**66. 加一** ✅ `0ms，击败 100%`（配额 2/5 → 3/5）

```java
public int[] plusOne(int[] digits) {
    for (int i = digits.length - 1; i >= 0; i--) {
        digits[i]++;
        if (digits[i] < 10) return digits;   // 没进位，直接返回
        digits[i] = 0;                        // 是 10，归零继续进位
    }
    int[] res = new int[digits.length + 1];   // 走到这说明全是 9
    res[0] = 1;
    return res;
}
```

**三个点**：
1. **必须逐位处理**：100 位的数远超 `long` 范围，不能转成数字再 +1——这是面试官想听的
2. **边界 `i >= 0`**：写成 `i > 0` 会漏掉最高位（`[0]` 直接失败）
3. **循环外兜底**：走到循环外说明每一位都进位了（全是 9），新建长度 +1 的数组

**排查思路**：一个用例失败时，先分清是「逻辑写错」还是「这条路径没走到」——`[0]` 那次失败是后者。

## 没完成的

- **力扣剩 2 题**（配额 3/5，剩 11.20–11.22 三天）

## 计划调整

- MySQL 基础章节完成（P50–61，约 4h38m）
- **JDBC 也补完了（P62–64）**：六步流程 + `Statement` vs `PreparedStatement`
- 原以为今天能到"多表查询 + 事务"，实际这两块在 P86–92 和 P101–103，**排到 week10**
- 力扣 3/5，剩两题三天清，节奏正常

## 明天计划（11.20 周五 · week09 Day05）

- [ ] **P65–72 MyBatis**（8 集 2h19m）：MyBatis 入门、注解开发、XML 映射、动态 SQL
- [ ] 力扣 1 题（3/5 → 4/5）
- [ ] 当天 commit：第9周Day05笔记
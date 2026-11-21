# 第 9 周 Day 06 - 2026.11.21

> 今天推进 P67–76（10 集）：MyBatis 增删改查 + XML 映射 + SpringBoot 配置 + 开发规范 + 工程搭建
> **状态：内容密度过高，学完感觉"虚"**

## 今天学了

### 1) 数据库连接池（P67）

每次请求都新建数据库连接代价太大，连接池就是**预先建好一批连接反复用**。

| 连接池 | 说明 |
|---|---|
| **HikariCP** | Spring Boot **默认**，性能最好，无需额外配置 |
| **Druid** | 阿里出品，带监控面板，企业里常用 |

**切换 Druid**：加 `druid-spring-boot-starter` 依赖 + 配置 `spring.datasource.druid.*` 即可，代码不用改。

### 2) MyBatis 注解 CRUD（P68–71）

```java
@Mapper
public interface UserMapper {
    @Select("select * from user where id = #{id}")
    User findById(Integer id);

    @Delete("delete from user where id = #{id}")
    void deleteById(Integer id);

    @Insert("insert into user(username, password, name, age) values(#{username}, #{password}, #{name}, #{age})")
    void insert(User user);

    @Update("update user set username = #{username}, name = #{name} where id = #{id}")
    void update(User user);

    @Select("select * from user where username = #{username} and password = #{password}")
    User findByUsernameAndPassword(@Param("username") String username, @Param("password") String password);
}
```

**`@Param` 的使用规则**（这块最容易混）：

| 场景 | 是否需要 `@Param` |
|---|---|
| 单个简单参数 | 不需要，`#{任意名}` 都能取 |
| **多个简单参数** | **必须加**，否则报 `Parameter 'xxx' not found` |
| 对象参数 | 不需要，`#{属性名}` 直接取 |
| 集合/数组 | 配合 `@Param` + `<foreach>` |

### 3) XML 映射（P72）

接口方法只留签名，SQL 写进 XML：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.itheima.mapper.UserMapper">
    <select id="findAll" resultType="com.itheima.pojo.User">
        select id, username, password, name, age from user
    </select>
</mapper>
```

**三条对应关系**：

| XML | Java |
|---|---|
| `namespace` | 接口**全限定名** |
| `<select id="findAll">` | 接口**方法名** |
| `resultType` | 返回类型（集合写元素类型） |

**必须配置扫描路径**，否则 XML 不生效：

```yaml
mybatis:
  mapper-locations: classpath*:com/itheima/mapper/*.xml
```

**注解 vs XML 怎么选**：
- 简单固定 SQL → 注解
- **动态 SQL、复杂查询** → XML（`<if>` / `<where>` / `<foreach>` 只能写在 XML 里）

### 4) SpringBoot 配置文件（P73）

```yaml
spring:
  application:
    name: springboot-mybatis-quickstart
  datasource:
    url: jdbc:mysql://localhost:3306/web01
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: '123456'

mybatis:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl   # 打印 SQL，调试用
```

**YAML 的三个要点**：
1. **层级靠缩进**（2 空格），不能用 Tab
2. 冒号后面**必须有一个空格**
3. 密码是纯数字时**要加引号**（`'123456'`），否则可能被当成数字

**`.properties` 和 `.yml` 同时存在时，`.properties` 优先级更高**。

**多环境配置**：

```
application.yml            # 公共配置
application-dev.yml        # 开发环境
application-prod.yml       # 生产环境
```

```yaml
spring:
  profiles:
    active: dev            # 激活哪个环境
```

### 5) 开发规范与 Restful（P74–75）⭐

**RESTful 的核心思想：URL 表示资源，HTTP 方法表示操作。**

| 操作 | 传统写法 | RESTful |
|---|---|---|
| 查列表 | `GET /dept/list` | `GET /depts` |
| 查单个 | `GET /dept/get?id=1` | `GET /depts/1` |
| 新增 | `POST /dept/add` | `POST /depts` |
| 修改 | `POST /dept/update` | `PUT /depts` |
| 删除 | `GET /dept/delete?id=1` | `DELETE /depts/1` |

**对比传统写法的优势**：
- URL 简洁，只出现名词（资源），不出现动词
- 同一个 URL 配不同方法就是不同操作
- 一眼能看出这个接口在"操作什么"

**面试题**：「你们项目的接口是怎么设计的？」——答 RESTful 风格 + 统一响应格式，是标准答案。

### 6) 工程搭建（P76）

**Maven 多模块（聚合工程）结构**：

```
web-ai-project02/              ← 父工程（只放 pom，管版本）
├── pom.xml                    ← <modules> 里列出子模块
└── tlias-web-management/      ← 子模块（真正的代码）
    ├── pom.xml                ← <parent> 指向父工程
    └── src/main/java/...
```

**父工程的两个作用**：
1. **统一管理依赖版本**（`<dependencyManagement>`）——子模块写依赖不用写版本号
2. 一处升级，所有子模块跟着变

## 今天写的代码

**tlias-web-management 的三层骨架已建好**：

```
controller/DeptController      ← @RestController，目前是空的
service/DeptService + impl     ← 接口 + 实现
mapper/DeptMapper              ← @Mapper，目前是空的
pojo/Dept                      ← 实体
pojo/Result                    ← 统一响应结果
```

**`Result` 统一响应类**（这是企业项目的标准做法）：

```java
@Data
public class Result {
    private Integer code;    // 1 成功，0 失败
    private String msg;
    private Object data;

    public static Result success() { ... }
    public static Result success(Object data) { ... }
    public static Result error(String msg) { ... }
}
```

**为什么需要它**：前端拿到的数据结构统一，不用每个接口猜字段。`{code, msg, data}` 这个三件套是行业惯例。

## 今天的真实感受

**这一周塞得太满了，学完是"知道有这些东西"，不是"掌握了"。**

具体表现：

| 现象 | 说明 |
|---|---|
| MyBatis 注解和 XML 两套写法混着来 | 两套心智模型压在一周 |
| JDBC → MyBatis → Spring Boot 一路往上封装 | 底下没消化，上面又盖一层 |
| Restful、开发规范、工程搭建 | 这些是**工程经验**，看一遍记不住很正常 |

**本周总量**：27 集、约 9h27m，平均每天 4.7 集——相当于一学期两门课的量。

**决定**：先跟一遍把地图看完，**后面二刷**。但跟课要标记「没懂的地方」，否则二刷等于重看。

## 没完成的

- **P77–85**（部门管理 CRUD + 日志技术，约 4h10m）
- **力扣 2 题**（配额 3/5，剩 11.22 一天）

## 计划调整

- 不追加新内容，今天到此为止
- **明天（周日）：力扣 2 题清零 + P84–85 日志 + week09 复盘**
- **二刷不排在现在**：等 12 月初 JavaWeb 学完，用 2–3 天集中回炉 MyBatis + Spring Boot
- 跟课新纪律：每集只记「讲了什么 / 哪里没懂 / 和已学内容有没有冲突」，这些标记就是二刷清单

## 明天计划（11.22 周日 · week09 Day07）

- [ ] **力扣 2 题**（3/5 → 5/5 清零）
- [ ] **P84–85 日志技术**（Logback 入门 + 配置文件与日志级别，约 52m）
- [ ] **week09 周复盘**（自然周最后一天）：闭卷自测 + 假会清单 + 排 week10
- [ ] 当天 commit：第9周Day07笔记
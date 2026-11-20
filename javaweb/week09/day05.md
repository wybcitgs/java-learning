# 第 9 周 Day 05 - 2026.11.20

> 状态：**今天懈怠**，只看了 P65–66（MyBatis 概述 + 入门）

## 今天学了（MyBatis P65–66）

### 1) MyBatis 解决什么问题

JDBC 写一个查询要六步，还要手动 `close()` 三次、手动把 `ResultSet` 拼成对象：

```java
Class.forName("com.mysql.cj.jdbc.Driver");
Connection conn = DriverManager.getConnection(url, user, pwd);
PreparedStatement ps = conn.prepareStatement(sql);
ps.setString(1, name);
ResultSet rs = ps.executeQuery();
while (rs.next()) { user.setId(rs.getInt("id")); ... }
rs.close(); ps.close(); conn.close();
```

MyBatis 把这些全封掉，**你只写两样东西**：一个接口 + 一条 SQL。

```java
@Mapper
public interface UserMapper {
    @Select("select * from user")
    List<User> findAll();
}
```

框架替你完成：创建连接、执行 SQL、把结果集映射成对象、释放资源。

### 2) 依赖与配置

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>4.0.1</version>
</dependency>
<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
</dependency>
```

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/web01
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=123456
```

### 3) `@Mapper` 的作用

程序启动时，**自动扫描这个接口，生成一个实现类（代理对象）并交给 IOC 容器管理**。

所以你能直接 `@Autowired private UserMapper userMapper;`——拿到的不是接口（接口没法 new），是**框架生成的代理实现**。

两个注意点：
- `@Mapper` 加在每个接口上，或者启动类加 `@MapperScan("com.itheima.mapper")` 统一扫描
- 没加 `@Mapper` 又没加 `@MapperScan`，启动时会报「找不到这个类型的 bean」

### 4) 跑通的验证

```java
@SpringBootTest
class SpringbootMybatisQuickstartApplicationTests {
    @Autowired
    private UserMapper userMapper;

    @Test
    public void testFindAll() {
        userMapper.findAll().forEach(System.out::println);
    }
}
```

控制台输出：

```
User(id=1, username=daqiao, password=123456, name=大乔, age=25)
User(id=2, username=xiaoqiao, password=123456, name=小乔, age=18)
...
Process finished with exit code 0
```

**看到数据 + 退出码 0，就是成功**。日志里那堆 `WARNING: A Java agent has been loaded dynamically`、`Mockito is currently self-attaching` 都来自测试框架，不用管。

**判断成功只看两处**：最后一行 `exit code 0`、有没有 `Caused by: ...Exception` 堆栈。

### 5) IDEA 红线 ≠ 报错

`@Select("select * from user")` 里的 `user` 画了红线，`@Mapper` 显示 *no usages*——这些是 **IDEA 的检查提示**，不是编译错误。

`mvn compile` 通过就说明代码没问题。想消除 SQL 红线，配一下 IDEA 的数据库数据源（能读表结构后就不报了，还能补全字段名）。

## 今天的插曲

**IDEA 打不开了**，报 `Cannot collect JVM options — stream did not contain valid UTF-8`。

原因：机器级环境变量 `IDEA_VM_OPTIONS` 指向课程资料里破解补丁的配置文件，那个文件含 GBK 编码的中文路径，新版 IDEA 强制校验 UTF-8 就崩了。**DataGrip 2026.3 之前也是同一个原因。**

处理：把用户级 `IDEA_VM_OPTIONS` 覆盖成 IDEA 自带的 `idea64.exe.vmoptions`，启动恢复正常。

**代价**：破解 agent 不再加载，需要走试用或学生许可。学生许可免费覆盖全部 JetBrains 产品，比每次修破解配置省事。

## 今天还做了一件计划外的事

**立项了 TRAE AI 全国大学生创造力大赛**（校园生活赛道），选题是「大学生减脂记录网站」。已写进 PLAN，包含 MVP 范围、12 月时间表和三条纪律。

三个关键约束（问询后确认）：

| 约束 | 影响 |
|---|---|
| 作品须**赛事期间用 TRAE 从 0 到 1 创作**，提交 Session ID ≥3 + 关键步骤截图 ≥3 | 开发工具要从 IDEA 换成 TRAE |
| TRAE **一键部署不支持 Java + MySQL**（只支持静态站点和 Node.js 20） | 部署要自己搞（学生云服务器） |
| 体验链接须**可公开访问**，评审期（12.25–1.8）能打开 | 得有公网服务 + 保证不停机 |

## 没完成的

- **P67–72**（注解开发 CRUD / 参数传递 / 动态 SQL / 综合案例，约 1.5h）→ 明天
- **力扣 0 题**（配额仍 3/5，剩 11.21–11.22 两天要清 2 题）

## 计划调整

- 今天只推进 2 集，MyBatis 章节整体顺延半天
- 明天（周六）任务较重：**P67–72 + 力扣 + TRAE 装好验证能跑 Java**（30 分钟，决定比赛可行性）

## 明天计划（11.21 周六 · week09 Day06）

- [ ] **P67–72** MyBatis：注解 CRUD、参数传递（`@Param`）、动态 SQL（`<if>` / `<where>` / `<foreach>`）、XML 映射
- [ ] **装 TRAE，建最小 Spring Boot 项目验证能否运行**（决定比赛是否可行）
- [ ] 力扣 1 题（3/5 → 4/5）
- [ ] 当天 commit：第9周Day06笔记
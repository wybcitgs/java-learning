# 第 9 周 Day 02 - 2026.11.17

> 状态：P40–48 看完，P49 只开了个头（时间不够，明天补）

## 今天学了

### 1) SpringBootWeb 入门（P40–42）

第一个能跑起来的 Web 接口：

```java
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String hello() {
        return "Hello World";
    }
}
```

启动类：

```java
@SpringBootApplication
public class SpringbootWeb01Application {
    public static void main(String[] args) {
        SpringApplication.run(SpringbootWeb01Application.class, args);
    }
}
```

三个注解的作用：

| 注解 | 作用 |
|---|---|
| `@SpringBootApplication` | 启动类标志，开启自动配置 + 组件扫描 |
| `@RestController` | 标记为控制器，**返回值直接作为响应体**（相当于 `@Controller` + `@ResponseBody`） |
| `@RequestMapping("/路径")` | 把 URL 映射到这个方法 |

**验证成功的标志**：控制台出现 `Tomcat started on port 8080`，浏览器访问 `http://localhost:8080/hello` 能看到返回值。

### 2) HTTP 协议（P43–45）

**请求结构**：

```
GET /hello?name=张三 HTTP/1.1        ← 请求行：方法 + 路径 + 版本
Host: localhost:8080                 ← 请求头
User-Agent: ...
Content-Type: application/json
                                     ← 空行（必须有）
{"name":"张三"}                      ← 请求体（GET 通常没有）
```

**响应结构**：

```
HTTP/1.1 200 OK                     ← 状态行
Content-Type: application/json      ← 响应头
Content-Length: 128
                                     ← 空行
[{"id":1,"name":"张三"}]             ← 响应体
```

**必须记住的三点**：

1) **GET 参数在 URL 里**（`?name=张三`），**POST 数据在请求体里**——决定了后端怎么接收参数
2) **`Content-Type` 决定数据格式**：`application/json`（JSON）、`application/x-www-form-urlencoded`（表单）
3) **状态码**：`200` 成功 / `302` 重定向 / `401` 未认证 / `403` 无权限 / `404` 找不到 / `500` 服务器错误

### 3) 三层架构（P47）⭐

```
请求 → Controller（接收请求、返回响应）
          ↓ 调用
       Service（业务逻辑：校验、编排、转换）
          ↓ 调用
       Dao（数据访问：读写文件 / 数据库）
```

**各层职责，一句话记住**：

| 层 | 管什么 | 不该管什么 |
|---|---|---|
| Controller | 接收请求参数、调用 Service、返回结果 | 不写业务判断，不直接碰数据源 |
| Service | 业务逻辑：校验、计算、转换 | 不处理 HTTP 请求 |
| Dao | 只负责取数据 | 不写业务规则 |

**判标准**：拿到一个新需求，能说出「这段代码写在层」——比如「判断用户是否存在」是 Service，「从 user.txt 读一行」是 Dao。

### 4) IOC 与 DI（P48）⭐

**改造前**（自己 new，强耦合）：

```java
UserService userService = new UserServiceImpl();
UserDao userDao = new UserDaoImpl();
```

**改造后**（交给容器）：

```java
@Component                    // 1. 把这个类交给 IOC 容器管理
public class UserServiceImpl implements UserService {

    @Autowired                // 2. 运行时自动找到同类型的 bean 注入进来
    private UserDao userDao;
}
```

| 概念 | 含义 |
|---|---|
| **IOC**（控制反转） | 对象的**创建权**从程序员交给 Spring 容器 |
| **DI**（依赖注入） | 容器负责把需要的对象**注入**到成员变量里 |

**解决什么问题**：Service 里只依赖 `UserDao` **接口**，不关心具体实现是谁。以后从「读文件」换成「查数据库」，只改 Dao 的实现类，Service 一行不动——**这就是解耦**。

## 今天的疑惑与解答

**问题**：之前 `/hello` 能直接访问，现在 `/list` 访问不了，还看到了一个"前端网站"。

**原因**：

1. **应用根本没启动**——检查 8080 端口没有监听，`mvn` 和 IDEA 的 java 进程都不是这个应用
2. 看到的"前端网站"是 `static/user.html` 这个静态文件，**直接双击用 `file://` 打开的**，没有后端在支撑，页面里的 `axios.get('/list')` 自然拿不到数据

**关键认知：Spring Boot 一个应用同时提供两类入口**

| 访问地址 | 谁提供 | 返回什么 |
|---|---|---|
| `/user.html` | `static/` 目录 | HTML 页面（人看的表格） |
| `/list` | `@RequestMapping("/list")` | JSON 数据（页面用 axios 取） |

**两者是并列关系，不是替代关系**。页面通过 `axios.get('/list')` 去请求接口，所以：
- 应用没启动 → 两个都打不开
- 应用启动了但只打开 html 文件 → 页面打开但表格空白

**排查口诀**：**先看控制台有没有 `Tomcat started on port 8080`**
- 没有 → 应用没起 / 启动失败
- 有但 404 → 路径写错
- 有但 500 → 代码逻辑问题

## 今天的分层成果

已经按三层架构拆好了：

```
controller/UserController       → 只负责接收请求、调 Service
service/UserService（接口）
service/impl/UserServiceImpl    → @Component + @Autowired 注入 Dao
dao/UserDao（接口）
dao/impl/UserDaoImpl            → @Component，只负责读 user.txt
pojo/User                       → 实体类
```

**接口 + 实现分离**是关键：Service 依赖 `UserDao` 接口而不是 `UserDaoImpl`，这才叫面向接口编程。

## 今天的力扣

**125. 验证回文串** ✅ `2ms，击败 97.73%`

双指针 + 跳过非字母数字：

```java
int l = 0, r = s.length() - 1;
while (l < r) {
    while (l < r && !Character.isLetterOrDigit(s.charAt(l))) l++;
    while (l < r && !Character.isDigit(s.charAt(r)) && !Character.isLetterOrDigit(s.charAt(r))) r--;
    if (Character.toLowerCase(s.charAt(l)) != Character.toLowerCase(s.charAt(r))) return false;
    l++; r--;
}
return true;
```

三个收获：`Character` 的静态方法要用类名调、跳过字符要用 `while` 不是 `if`、每个 `while` 都要带 `l < r` 防越界。

## 没完成的

- **P49 IOC & DI 详解**（只开了头，约 42 分钟）
- **P50–61 MySQL**（本周剩余部分）
- 力扣配额 **2/5**（周三至周日 4 天要清 3 题）

## 计划调整

- week09 前两天进度偏慢（Day01 休息、Day02 只到 P48），**MySQL 部分可能需要压缩**：
  - 学校在教 MySQL，视频可以只挑「多表查询 / 事务 / 索引」等学校没讲深的部分
  - 优先保证 P49 和 P46–49 的三层架构 + IOC/DI 吃透
- 下一段 P49 是 IOC/DI 详解，**面试高频**，不能倍速

## 明天计划（11.18 周三 · week09 Day03）

- [ ] **P49** IOC & DI 详解（42m，正常速度）
- [ ] **动手**：把 springboot-web-01 跑起来，验证 `/list` 返回 JSON
- [ ] 力扣 1 题（2/5 → 3/5）
- [ ] 当天 commit：第9周Day03笔记
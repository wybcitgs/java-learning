# 第 8 周 Day 07 - 2026.11.15

> week08 收官日：周复盘 + 掘金第 8 篇 + 单元测试

## 今天做了

### week08 周复盘 ✅

**闭卷自测 20 题：16 对 / 4 半 / 0 错**（比 week07 的 6/20 大幅提升）

4 个半对的点：

1. HTML 骨架漏了 `<!DOCTYPE html>` 和 `meta charset`/`viewport`
2. `margin: 0 auto` 只对**块级元素**有效，行内元素无效
3. `let` / `const` / `var` 的准确定义（let 不是"弱类型"，var 是老写法）
4. 模板字符串的好处（支持换行、免拼接）

复盘文件已提交（`dd353f9`）。

### 今天发布

- 🚀 **掘金第 8 篇**：《JavaWeb 第一周：前端三件套（HTML / CSS / JS + Vue）到底该记什么》
  https://juejin.cn/post/7708094937464586278
  素材 = week08 前端全部内容；按「必须记 / 理解就行 / 查文档就够」三层切分；草稿不入库，发布后已删除

## 今天学了（P35–39 单元测试）

### 1) 为什么写单元测试

改一行代码，跑一遍测试就知道**有没有弄坏别的功能**。后端项目里，这是防止"改 A 坏 B"的基本手段。

### 2) JUnit 5 基本结构

```java
public class UserServiceTest {
    @Test
    public void testGetGender() {
        UserService userService = new UserService();
        String gender = userService.getGender("44030419900101001X");
        Assertions.assertEquals("男", gender);     // 断言：期望值, 实际值
    }
}
```

**`assertEquals` 的参数顺序**：**第一个是期望值，第二个是实际值**。写反了照样能过，但失败时报错信息会误导。

### 3) 常用断言

| 断言 | 用途 |
|---|---|
| `assertEquals(期望, 实际)` | 值相等 |
| `assertThrows(异常类, () -> 执行)` | 预期抛异常 |
| `assertTrue(条件)` / `assertFalse(条件)` | 布尔判断 |
| `assertNull` / `assertNotNull` | 空判断 |

`assertThrows` 的第二个参数是 **lambda**，把"应该抛异常的那行代码"包进去：

```java
Assertions.assertThrows(IllegalArgumentException.class, () -> userService.getAge(null));
```

### 4) 四个生命周期注解

| 注解 | 执行时机 | 要求 |
|---|---|---|
| `@BeforeAll` | 所有测试**前**一次 | **必须是 static 方法** |
| `@AfterAll` | 所有测试**后**一次 | 必须是 static |
| `@BeforeEach` | **每个**测试前 | — |
| `@AfterEach` | 每个测试后 | — |

实际用得最多的是 `@BeforeEach`——**把对象初始化提到公共前置里**：

```java
private UserService userService;

@BeforeEach
public void setUp() {
    userService = new UserService();     // 每个测试都拿到全新的实例
}
```

这样每个测试方法里只剩"测什么"，不用重复 `new`。

### 5) `@DisplayName` 让测试报告可读

```java
@Test
@DisplayName("测试获得年龄 -空串")
public void testGetAgeAssert2() { ... }
```

不写时报告显示 `testGetAgeAssert2`，写了显示中文描述。**测试失败时能一眼看出是哪个场景挂了**，这是企业规范里的基本要求。

### 6) 参数化测试

```java
@ParameterizedTest
@ValueSource(strings = {"", "  ", "abc"})
public void testGetGenderInvalid(String input) {
    Assertions.assertThrows(IllegalArgumentException.class, () -> userService.getGender(input));
}
```

**一份代码跑多组数据**，不用复制粘贴多个测试方法。

### 7) 用例设计：边界值才是重点

`getAge` 一个方法写了 5 个测试用例：

| 用例 | 输入 |
|---|---|
| null | `null` |
| 空串 | `""` |
| 长度不足 | `"44030419900101001"` |
| 长度超过 | `"44030419900101001XYZ"` |
| 正常 | `"44030419900101001X"` |

**正常路径只占 1 个，其余 4 个全是边界和异常**。这是单元测试最核心的思维——**bug 几乎都出在边界上**。

### 8) 依赖范围 `test`

```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.9.1</version>
    <scope>test</scope>
</dependency>
```

`<scope>test</scope>` 表示这个依赖**只参与测试的编译和运行，不打进最终的 jar**。

不写 scope 时默认是 `compile`，会一直带到生产包里去。

## 没完成的

- **P40–49 SpringBootWeb 入门**（原计划 week08 内完成，顺延到 week09）
- 掘金欠账已补（第 8 篇今天发了）

## 计划调整

- week08 实际完成 **P1–39**（39/200 集），后端框架部分顺延
- 力扣 **5/5 已清零**（20 / 1047 / 232 / 155 / 225），连续第 5 周满配
- 博客欠账补清（8 篇）

## 明天计划（11.16 周一 · week09 Day01）

- [ ] **P40–42** SpringBootWeb 入门（课程安排 / 入门程序 / 入门解析），约 1h
- [ ] 力扣 1 题（week09 配额 1/5）
- [ ] 当天 commit：第9周Day01笔记
- 📌 本周主线：**P40–49 SpringBootWeb + HTTP + 三层架构 + IOC/DI** ⭐ + **P50–61 MySQL**
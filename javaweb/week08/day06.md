# 第 8 周 Day 06 - 2026.11.14

> 上午清完力扣配额，晚上看 Maven（P31–34）并配通环境

## 今天学了：Maven


### 1) Maven 是什么

**项目管理工具**，解决两件事：

| 问题 | Maven 的答案 |
|---|---|
| 依赖的 jar 从哪来、放哪 | 中央仓库 + `pom.xml` 声明，自动下载到本地仓库 |
| 项目怎么编译打包 | `mvn compile` / `package` / `install` |

### 2) 仓库三层

```
本地仓库 → 私服（公司内部）→ 中央仓库（美国）
   有就停      没有才往上找      最终来源
```

- **本地仓库**：`C:\Users\hhh\.m2\repository`（我改成了这个标准位置）
- 找不到就一层层往上找，找到后**往回缓存**

### 3) 生命周期（Lifecycle）

五个常用阶段，**按顺序执行，后面的会带上前面的**：

| 阶段 | 作用 |
|---|---|
| `clean` | 清理，删掉 `target` 目录 |
| `compile` | 编译，`src/main/java` → `target/classes` |
| `test` | 跑单元测试（会先 compile） |
| `package` | 打包，生成 jar/war 到 `target` |
| `install` | 安装，把包装进**本地仓库**供别的项目依赖 |

**关键理解**：执行 `mvn install` 时，Maven 会**自动依次执行** `compile → test → package → install`——不用一条条敲。

**注意 `clean` 是独立的一条线**（clean 生命周期），不属于默认生命周期，所以常写成组合：

```cmd
mvn clean package      # 先清空 target，再打包
mvn clean install      # 先清空 target，再打包并装进本地仓库
```

### 4) pom.xml 的结构

```xml
<project>
  <modelVersion>4.0.0</modelVersion>

  <!-- 坐标：唯一确定一个项目 -->
  <groupId>com.itheima</groupId>
  <artifactId>maven-project01</artifactId>
  <version>1.0-SNAPSHOT</version>

  <properties>
    <maven.compiler.source>21</maven.compiler.source>
    <maven.compiler.target>21</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>6.1.4</version>
    </dependency>
  </dependencies>
</project>
```

**坐标三件套**：`groupId`（组织）+ `artifactId`（项目）+ `version`（版本），像身份证号一样定位一个 jar。

### 5) 依赖传递

只声明了 `spring-context`，但 `mvn dependency:tree` 显示它自动带进来：

```
spring-context
  ├── spring-aop
  ├── spring-beans
  ├── spring-core
  │    └── spring-jcl
  └── spring-expression
```

**这就是依赖传递**——你不用一个个声明，Maven 会顺着 pom 往下找。

### 6) 排除依赖（exclusions）

想砍掉某个传递进来的依赖：

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context</artifactId>
  <version>6.1.4</version>
  <exclusions>
    <exclusion>
      <groupId>io.micrometer</groupId>
      <artifactId>micrometer-observation</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

**验证方法**：`mvn dependency:tree`，确认 `micrometer-observation` 不在树里 ✓（已实测生效）

## 环境配置（今晚的主要工作）

### 装 Maven

- 版本 **3.9.4**，解压在 `D:\maven\apache-maven-3.9.4-bin\apache-maven-3.9.4`
- 用 JDK 21（Temurin），`mvn -v` 显示 Maven 3.9.4 + Java 21.0.12.1 ✓

### 踩过的三个坑

**1) `MAVEN_HOME` 没设置，`mvn` 命令找不到**

系统 PATH 里已经有 `%MAVEN_HOME%\bin`，但 `MAVEN_HOME` 变量是空的 → 展开成 `\bin`，无效。

解决：设置 `MAVEN_HOME` + 把**绝对路径**直接加进用户 PATH：
```
D:\maven\apache-maven-3.9.4-bin\apache-maven-3.9.4\bin
```

**2) 本地仓库默认放进了 Maven 安装目录**

原来配的是 `D:\maven\...\mvn_repo`——Maven 一升级或删掉，几 GB 依赖全丢。

改成标准位置：`C:\Users\hhh\.m2\repository`（IDEA 默认也认这里）。

**3) 阿里云镜像用的是已废弃的老地址**

```xml
<!-- ❌ 老地址，会 301 跳转 -->
<url>http://maven.aliyun.com/nexus/content/groups/public/</url>

<!-- ✅ 现在的地址 -->
<url>https://maven.aliyun.com/repository/public</url>
```
实测老地址返回 301，新地址 200、0.25 秒。

**环境变量改完必须重开终端**——已经打开的窗口（包括 Windows Terminal 的新标签页）读的还是旧 PATH。

### IDEA 配置

| 设置项 | 值 |
|---|---|
| Maven home path | `D:\maven\apache-maven-3.9.4-bin\apache-maven-3.9.4` |
| User settings file | `...\conf\settings.xml` |
| Language level | **21 - Record patterns...**（不带 Preview） |
| JDK | temurin-21（不选 `Use JAVA_HOME`，避免环境变量变动导致失效） |

**Preview 版本不要选**——JDK 21 的 String templates 就是预览特性，到 23 直接被砍掉了。

## 今天的两道力扣（配额 2/5 → 5/5 清零 🏁）

补完上周欠的三题：

### 232. 用栈实现队列 ✅ `0ms，击败 100%`
两个栈：`inStack` 只进、`outStack` 只出，`outStack` 空时把 `inStack` 全倒过去。抽了个 `inToOut()` 给 `pop` / `peek` 复用。

### 155. 最小栈 ✅ `34ms，击败 37.7%`
辅助栈 `minDq` **每次 push 都压"当前最小值"**（不是只压更小的），这样两个栈长度一致，pop 时无脑同步弹。

### 225. 用队列实现栈 ✅ `1ms，击败 72.4%`
一个队列做**旋转**：push 后把前 `size-1` 个元素依次转回队尾，让新元素跑到队头。

**三题对照**：栈→队列靠"倒一次"，队列→栈靠"转一圈"；232 和 225 是同一对矛盾的镜像题。

## 没完成的

- **P35–39 单元测试**（原速 1h34m）→ 明天
- 今天原计划还有单元测试 5 集，实际只完成 Maven 4 集

## 计划调整

- 力扣 **5/5 已清零**，连续第 5 周满配
- week08 剩最后一天（11.15 周日），需要同时完成：单元测试 P35–39 + **week08 周复盘**
- SpringBootWeb 入门（P40–49）顺延到 week09

## 明天计划（11.15 周日 · week08 Day07）

- [ ] **P35–39 单元测试**（概述 / 断言与注解 / 企业规范与 AI 生成 / 依赖范围 / 常见问题），原速 1h34m
- [ ] **week08 周复盘**（首个自然周，周日复盘）：勾验收 + 假会清单 + 排 week09
- [ ] 力扣：本周已清零，不排新题
- [ ] 当天 commit：第8周Day07笔记
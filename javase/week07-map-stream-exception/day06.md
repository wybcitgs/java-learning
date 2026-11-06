# 第 7 周 Day 06 - 2026.11.04

> 本篇按 `E:\java\项目\Api\src\com\itheima\time\` 下今天实际敲的 15 个文件整理

## 今天学了（上部 P169–173 时间类 + 主线 P149，约 2h25m + 40m）

文件清单：`Date`(2) / `SimpleDateFromat`(3) / `Calendar`(1) / `jdk8Date`(3) / `jdk8SimpleDateFormat`(1) / `jdk8Calendar`(3) / `jdk8util`(3)

### Date
- `new Date()` 当前时间；`new Date(long)` 指定毫秒值
- `getTime()` 取毫秒值 / `setTime(long)` 改毫秒值
- **时间原点 = 1970-01-01 00:00:00 UTC**；东八区打印出来是 **1970-01-01 08:00:00**
- 练习：时间原点 1 年后 → `new Date(1000L * 60 * 60 * 24 * 365)`；加 `L` 是为了让整个表达式走 **long 运算**，否则 `int` 相乘超范围（`1000*60*60*24*365 ≈ 3.15e10`，远超 int 上限）
- 练习：两个 Date 比大小 → 转 `getTime()` 比较，或用 `after()` / `before()`

### SimpleDateFormat
- `format(Date)` 日期 → 字符串；`parse(String)` 字符串 → 日期
- **`parse` 抛 `ParseException`，是受检异常（编译时异常）**：不 `try-catch` 也不 `throws`，**编译直接失败**。`main` 后面写 `throws ParseException` 就是在处理它
- **构造格式必须和字符串一致**，否则 parse 失败
- 模式字母：`y` 年 / `M` 月 / `d` 日 / `H` 24 小时 / `h` 12 小时 / `m` 分 / `s` 秒 / `E` 星期 / `a` 上午下午
- **`MM` 写成 `mm` 不报错**，但会把分钟当月份解析 → 结果错。这类错误编译器抓不到
- 练习：`2007-2-21` → `2007年2月21日`（先 parse 成 Date，再用新格式 format）
- 练习：秒杀活动判断（开始/结束/下单三个时间都转 `getTime()` 比较区间）

### Calendar
- **抽象类，不能 new**，用 `Calendar.getInstance()` 获取（另一种：`new GregorianCalendar()`）
- `getInstance()` 底层：按时区生成对象，把年月日时分秒存进一个数组
- **`MONTH` 范围 0–11**，1 月是 0 → `get(MONTH)` 返回 10 就是 11 月
- **`DAY_OF_WEEK` 范围 1–7，1 是星期日**
- 方法：`get(int field)` / `set(int field, int value)` / `add(int field, int amount)` / `getTime()` / `setTime(Date)` / `getTimeInMillis()` / `setTimeInMillis(long)`
- **`add` 是在原值上增减，`set` 是改成指定值**
- 字段也能用数字表示：0 纪元、1 年、2 月、3 一年中第几周、4 一个月中第几周、5 一个月中第几天……（用常量更可读）

### JDK8 时间（Instant / ZoneId / ZonedDateTime / DateTimeFormatter / LocalDate(Time) / 工具类）
- **`ZoneId`**：`getAvailableZoneIds()` 全部时区 / `systemDefault()` 系统默认 / `of("Asia/Shanghai")` 指定
- **`Instant`（标准时间，UTC）**：`now()` / `ofEpochMilli(0L)` / `ofEpochSecond(1L)` / `ofEpochSecond(1L, 1000000000L)`
  - ⚠️ `ofEpochSecond(秒, **纳秒**)`，第二个参数是**纳秒**不是秒 → 传入 10⁹ ns 相当于再加 1 秒，所以 `ofEpochSecond(1L, 1000000000L)` = **第 2 秒**
  - `isBefore` / `isAfter` 判断；`plusXxx` / `minusXxx` 增减
- **`ZonedDateTime`（带时区）**：`now()` / `of(年,月,日,时,分,秒,纳秒,ZoneId)` / `ofInstant(Instant, ZoneId)`
  - **参数顺序：年 月 日 时 分 秒 纳秒 时区**（共 8 个）
  - `Instant.now().atZone(zone)` 也能转成 ZonedDateTime
- **`DateTimeFormatter`**：`ofPattern("yyyy-MM-dd HH:mm:ss EE a")` + `format(时间对象)`（`EE` 全称星期、`E` 简称）
- **`LocalDate` / `LocalTime` / `LocalDateTime`**（三个只有方法注释，无实际调用）
  - get 系列取值；`is` 开头是判断；`with` 开头是**修改**（只改年月日）；`minus` 减少；`plus` 增加
  - `LocalDateTime` 是前两者的汇总，可转成 `LocalDate` / `LocalTime`
- **三个间隔工具类**：

| 类 | 算什么 | 单位 | 场景 |
|---|---|---|---|
| `Period` | 两个**日期**之间 | 年 / 月 / 日 | 算年龄、合同到期 |
| `Duration` | 两个**时间点**之间 | 秒 / 纳秒 | 程序耗时、接口超时 |
| `ChronoUnit` | 任意两个时间之间 | 年→纳秒任选 | 万能版，`ChronoUnit.DAYS.between(a, b)` |

**最重要的认知：JDK8 时间类都是不可变的**
```java
ZonedDateTime time4 = time3.withYear(2024);   // time3 本身不变，返回新对象
ZonedDateTime time5 = time4.minusDays(1);
```
和 String 一样；而 `Calendar.add()` 是**改自己**。这是新旧 API 最本质的区别。

### P149 BigInteger / BigDecimal（47m，1.25 倍速，20:32 加看）

**BigInteger：超大整数（超出 long 范围，19 位以上）**
- 两种创建方式：`new BigInteger("100")` 字符串构造 / `BigInteger.valueOf(15)`
- **缓存**：类加载时提前创建 **-16 ~ 16 共 33 个对象**放数组里，`valueOf` 在这范围内直接复用（和 `Integer.valueOf` 同一思路）
- **超出 long 范围就只能用构造方法**（字符串）

**BigDecimal：精确小数**
- 创建：`new BigDecimal("11.5")` 字符串构造（**精确，推荐**）/ `new BigDecimal(char[])`（不常用）/ `BigDecimal.valueOf(11.5)`（底层走字符串构造，0–10 预建缓存）
- 结论：**小数在 double 范围内可用静态方法，超出范围必须用字符串构造**

**两者方法一致**（`BigInteger` / `BigDecimal` 同名同义）：

| 方法 | 作用 |
|---|---|
| `add` / `subtract` / `multiply` | 加 / 减 / 乘 |
| `divide(val)` | 除法，取商 |
| `divideAndRemainder(val)` | 返回 `BigInteger[]`：`[0]` 商、`[1]` 余数 |
| `equals` | 比较 |
| `pow(int)` | 次方 |
| `max` / `min` | 最大 / 最小 |

- **运算都返回新对象，不改变原对象**（和 JDK8 时间类一样是不可变的）
- **BigDecimal 除法陷阱**：除不尽时 `divide(b2)` 直接抛 `ArithmeticException`，必须指定精度
  ```java
  b1.divide(b2, RoundingMode.HALF_UP);      // 保留 1 位，四舍五入
  b1.divide(b2, 2, RoundingMode.HALF_UP);   // 保留 2 位
  ```
- 实战意义：**金额计算必须用 BigDecimal**（double 有精度误差），这是面试常问点

## 时间类自检 12 题 → 5 对 / 4 半 / 4 错

| # | 我的答案 | 判定 | 修正 |
|---|---|---|---|
| 1 | 1970-01-01 00:08:00，东八区 +8h | ⚠️ | 推理对，数字写错：是 **08:00:00** |
| 2 | 不加 L 会溢出，发生在运行时 | ⚠️ | 会溢出，但这是**常量表达式，编译期就折叠**算错，不报错不抛异常 |
| 3 | 空指针异常 | ❌ | 是 **`ParseException`，受检（编译时）异常**；签名上明写 `throws`，不处理编译不过 |
| 4 | y年 M月 d日 H时 m分 s秒 E周几 | ✅ | 映射全对；补：`MM`→`mm` 不报错但结果错 |
| 5 | 11 月，星期二 | ⚠️ | 月份对；**`DAY_OF_WEEK` 返回 1 是星期日** |
| 6 | 抽象类，需子类或 get 方法 | ⚠️ | 抽象类对；另一条路是 **`new GregorianCalendar()`** |
| 7 | add 增减 / set 改成指定值 | ✅ | — |
| 8 | 第 2 秒 | ✅ | 第二个参数是纳秒，10⁹ ns = 1 s |
| 9 | time3 不变，Calendar.add 改自己 | ✅ | — |
| 10 | 未答 | ❌ | 见上表 Period / Duration / ChronoUnit（第 12 题想写 Duration + ChronoUnit，写串了） |
| 11 | 年 月 日 时 秒 分 微秒 时区 | ❌ | 顺序是 **年 月 日 时 分 秒 纳秒 时区**，单位是**纳秒**不是微秒 |
| 12 | after / before，或用 Period/Duration 算间隔 | ✅ | 补：`after/before` 属于 `Date`（无 is 前缀）；`isAfter/isBefore` 是 JDK8 类才有 |

## week06 复测 3 项（补测）

| # | 结果 | 要点 |
|---|---|---|
| A 泛型通配符 | ✅ | `? extends` 不能 add、读出 `Number`；`? super` 能 add、读出 `Object` |
| B 集合复杂度 | ❌ | **LinkedList 是双向链表不是数组**。ArrayList：`get(i)` O(1) / 头部 add O(n) / 尾部 add 均摊 O(1)；LinkedList：`get(i)` O(n) / 头尾 add O(1) |
| C equals-hashCode | ❌ | 因果链说反了。正确：hashCode 定位桶 → **同桶才调 equals** → 只重写 equals 时 hashCode 按地址算 → 落不同桶 → **equals 压根不执行** → 去重失效（不是"equals 比地址"） |

- B、C 两项**仍未通过**，进 11.6 总复习重测

## LeetCode 9 回文数 ✅（week07 配额 5/5 清零 🏁）

**第一版（int 版）**：`5ms，击败 94.44%`
- 踩坑：`if (result == temp) { return true; }` 之后**没有兜底 return** → 编译报 `missing return statement`
- 隐患：`result * 10 + lastnumber` 没防溢出，靠「非回文恰好返回 false」侥幸通过
- 修法：`return result == temp;`

**第二版（只反转一半，优化版）**：`4ms，击败 100%`，内存 45.11MB
```java
if (x < 0 || (x % 10 == 0 && x != 0)) return false;   // 负数、非 0 的 10 的倍数直接排除
int rev = 0;
while (x > rev) {           // 只反转到一半就停
    rev = rev * 10 + x % 10;
    x /= 10;
}
return x == rev || x == rev / 10;   // 奇数位时中间那位不用比
```
- 优点：**不用防溢出**（只反转一半，max 也就 5 位），循环次数减半
- `x == rev / 10` 处理奇数位：`12321` → 循环停时 `x = 12`、`rev = 123`，`rev / 10 = 12` 相等

## 没完成的
- **week06 复测 B、C 两项未通过** → 11.6 总复习重测
- 掘金第 6 篇（明天 11.5 复盘日发）

## 计划调整（11.4 晚）
- 今天原排 5 集时间类（P169–173）**全部完成**，15 个练习文件
- **P149 提前到今晚看完**（原排 11.7），4 个练习文件 → **JavaSE 阶段的视频全部清完，一集不剩**
- week07 力扣配额 **5/5 已清零**（242 / 350 / 387 / 7 / 9），连续第 4 周满配
- week06 复测原定 11.4 重测 3 项，实际 A 过、B/C 未过，**顺延到 11.6**
- 环境备注：20:33 Oracle **JRE 8 自动更新**到 1.8.0_511（`C:\Program Files\Java\jre1.8.0_511`）；JDK 8（1.8.0_503）和 JDK 21（21.0.12.1）均未变动，IDEA 项目不受影响

## 明天计划（11.5 周四 = 学习周最后一天）
- [ ] **week07 周复盘**（勾验收、更新假会清单、排 week08）
- [ ] **掘金第 6 篇**（学习周最后一天前发完，不顺延）
- [ ] 力扣：本周已清零，不排新题
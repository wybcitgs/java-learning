# 第 7 周 Day 05 - 2026.11.03

> 本篇按 `E:\java\项目\Api\src\com\itheima\` 下今天实际敲的四个包整理：`math` / `system`（含 object）/ `integer` / `arrays`

## 今天学了（主线 P146–148 + 上部 P182 Arrays，约 2h20m）

### P146 Math 类（39:51）

**API 速查**：`abs` / `ceil` / `floor` / `round` / `max` / `min` / `pow` / `random` / `sqrt` / `cbrt`
- **Math 不能 new**：构造方法私有，全是静态方法（`new Math()` 编译不过）

**三个反直觉点**：
- **`round` 不是四舍五入**，规则是「**返回与参数最近的整数，若距离相等则取正无穷方向**」：
  ```java
  Math.round(3.14)  // 3
  Math.round(3.5)   // 4
  Math.round(-1.5)  // -1  ← 不是 -2，因为 -1.5 距 -2 和 -1 一样近，取向正无穷方向
  ```
- `Math.abs(Integer.MIN_VALUE)` 返回**负数**（边界值取绝对会溢出）
- `Math.random()` 范围 **[0.0, 1.0)**；取 `[a, b]` 整数：`(int)(Math.random() * (b - a + 1)) + a`

**开方两种写法**：`Math.sqrt(9)` = 3.0 / `Math.cbrt(27)` = 3.0，等价于 `Math.pow(9, 0.5)`、`Math.pow(27, 1.0/3)`

**两道算法题**：
1. **质数判断**（`MathTest2`）：`i` 从 2 循环到 `Math.sqrt(num)`，能整除就不是质数
2. **自幂数判断**（`MathTest3`）：先用 `(num + "").length()` 取位数，再逐位取数 `% 10`、`Math.pow(每位, 位数)` 累加，`/ 10` 砍位

### P147 System 类 + Object 类（42:45）

**System**
| 方法 | 作用 | 细节 |
|---|---|---|
| `exit(int status)` | 终止 JVM | 0 = 正常退出，非 0 = 异常退出；**退出时 finally 不执行** |
| `currentTimeMillis()` | 当前毫秒值 | 从 1970-01-01 00:00:00 UTC 起算；我国东八区对应 08:00:00 |
| `arraycopy(src, srcPos, dest, destPos, len)` | 数组拷贝 | 五个参数顺序：源数组、源起始、目标数组、目标起始、拷贝个数 |

单位换算：1 秒 = 1000 毫秒 = 1,000,000 微秒 = 1,000,000,000 纳秒

**Object**
- Java 中**顶级父类**，所有类都直接或间接继承它，所有对象都是 `Object` 的实例
- 常用方法：`toString()`（返回对象的字符串表示）、`equals()`、`hashCode()`
- 这三个在 week06 集合进阶时已展开学过（现在知道它们**都来自 Object**）

### P148 包装类（30:32）

**获取 Integer 对象的三种方式**：
```java
Integer i1 = new Integer(100);        // ❌ Java 9 起标记废弃：绕过缓存，每次 new 都新建对象
Integer i2 = Integer.valueOf(200);    // ✅ 静态方法，推荐
Integer i3 = 300;                     // ✅ 直接赋值（编译器自动调用 valueOf）
```

**Integer 缓存（课程叫「Integer 常量池」）**：
- `Integer` 类加载时，会**提前创建 -128~127 共 256 个对象**放进一个数组
- `valueOf` 先判断值是否在范围内：**在 → 直接取数组里的对象；不在 → 才 new**
- 所以：
  ```java
  Integer a = 127, b = 127;  a == b  // true（同一个对象）
  Integer c = 128, d = 128;  c == d  // false（各自 new）
  ```
- 严格说：机制是 `Integer` 内部的 `IntegerCache.cache[]` 静态数组，课程称「Integer 常量池」——两种叫法指同一个东西，注意别和 JVM 的运行时常量池/字符串常量池混为一谈

**常用方法**（`IntegerTest2`）：
| 方法 | 作用 |
|---|---|
| `valueOf(int)` | 基本类型 → 包装类 |
| `intValue()` | 包装类 → 基本类型（**拆箱**，这正是自动拆箱背后调的方法） |
| `parseInt(String)` | 字符串 → **int**；字符串里不能有非数字，否则报错 |
| `toBinaryString` / `toOctalString` / `toHexString` | 转二 / 八 / 十六进制字符串 |

**自动拆箱 NPE**：`Integer i = null; int j = i;` → 编译器插入 `i.intValue()` → **NullPointerException**
- 高频场景：`int x = map.get("不存在的key");`

### 上部 P182 Arrays 工具类（27:15）

**这是路线漏项**：原筛选边界从 P183 开始，把 P182 隔在外面，Arrays 从没系统学过（只在力扣用过 `Arrays.sort`、`Arrays.stream`）。

| 方法 | 用途 | 细节 |
|---|---|---|
| `toString(arr)` | 数组 → 字符串 | 不写只能看到 `[I@1b6d3586` |
| `binarySearch(arr, key)` | 二分查找 | **必须升序**才有效；**找不到时返回 `-插入点-1`**（不是 -1） |
| `copyOf(arr, newLen)` | 拷贝 / 扩容 | 新长度 < 原：部分拷贝；= 原：全拷贝；> 原：**剩余位置填 0** |
| `copyOfRange(arr, from, to)` | 拷贝区间 | **左闭右开 [from, to)** |
| `fill(arr, val)` | 全部填充 | 常用于初始化 |
| `sort(arr)` | 默认升序排序 | 基本类型数组也能直接排 |
| `sort(arr, Comparator)` | 自定义规则 | ⚠️ **只能排引用类型数组**，基本类型要先装箱成 `Integer[]` |

自定义排序示例（降序）：
```java
Integer[] arr4 = {5, 3, 1, 4, 2};
Arrays.sort(arr4, new Comparator<Integer>() {
    @Override
    public int compare(Integer o1, Integer o2) {
        return o2 - o1;      // 降序
    }
});
```

## 包装类自检（3 题）→ 1 对 / 1 半 / 1 待修

| # | 我的答案 | 判定 | 修正 |
|---|---|---|---|
| ① `127` vs `128` 的 `==` | true / false，因为 128 不在常量池中 | ✅ | 说法基本对（课程确实叫「Integer 常量池」）；严格机制是 `IntegerCache` 静态数组，范围外才 new |
| ② 拆箱 null | 「i 是对象，不能直接赋值给 j」 | ⚠️ 半 | 现象对，**后果没说**：编译器插入 `intValue()` → **NullPointerException** |
| ③ `parseInt` vs `valueOf` | 「valueOf 是把整数包装成包装类」 | ⚠️ 需修 | `parseInt(String)` → **int**；`valueOf(String 或 int)` → **Integer**。一句话：前者返回基本类型，后者返回对象 |

## LeetCode 7 整数反转 ✅（week07 配额 4/5）

```java
int rev = 0;
while (x != 0) {
    int digit = x % 10;
    x /= 10;
    if (rev > Integer.MAX_VALUE / 10 || (rev == Integer.MAX_VALUE / 10 && digit > 7)) return 0;
    if (rev < Integer.MIN_VALUE / 10 || (rev == Integer.MIN_VALUE / 10 && digit < -8)) return 0;
    rev = rev * 10 + digit;
}
return rev;
```

- **通过 1046/1046，1ms（击败 97.44%），内存 41.99MB**
- **`digit`** = 当前取出的那一位；**`rev`** = 已拼好的倒序数字。`rev * 10` 腾出个位，`+ digit` 填进去
- **负数不用单独处理**：Java 的 `%` `/` 向零取整，`-123 % 10 = -3`，符号自带
- **溢出必须在 `rev * 10` 之前判断**——事后判断没用，那一刻已经回绕成负数
  - `rev > Integer.MAX_VALUE / 10` → 乘 10 必溢出
  - `rev == 214748364` 时看末位：正数 `digit > 7`、负数 `digit < -8` 才溢出
- **认知**：和今天 P146 的**自幂数**是同一套路——逐位取数（`% 10` 取、`/ 10` 砍）。自幂数把每位做幂运算累加，这题把每位拼到新数末尾

## 没完成的
- **力扣 1 题**（week07 配额 4/5，11.5 复盘前清零）
- **复测 3 项**（11.4）：泛型通配符 PECS、ArrayList vs LinkedList 复杂度、equals 与 hashCode 因果链
- **P149 BigInteger / BigDecimal**（47m，已排 11.7）
- 掘金第 6 篇（11.5 复盘日）

## 计划调整（11.3 晚）
- **发现路线漏项**：上部 P182 Arrays 一直被筛选边界挡在外面，已补入 PLAN 并今天看完
- 欠账路线从「上部 P155/P157/P174」改为「主线 P146–149」：是你一路跟下来的课，讲法连贯，且多出 Object 类和 BigInteger/BigDecimal
- 实际完成量超出原计划（原定 1h53m，实际 2h20m 四集全清）

## 明天计划（11.4 周三）
- [ ] **上部 P169–173 时间类**（Date / SimpleDateFormat / Calendar / JDK8 时间，2h25m）
- [ ] **复测 3 项**：泛型通配符 / ArrayList-LinkedList 复杂度 / equals 与 hashCode 因果链
- [ ] 力扣 1 题（4/5 → 5/5 清零）
- [ ] 当天 commit：第7周Day06笔记
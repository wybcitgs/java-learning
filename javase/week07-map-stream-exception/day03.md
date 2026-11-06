# 第 7 周 Day 03 - 2026.11.01

## 今天学了（下部 P20–26 集合收尾 + P35–42 Stream，约 2h50m）

### P20–21 可变参数 / Collections
- **可变参数**：`sum(int... nums)`，本质是**数组**，方法内当数组用；**只能有一个、且必须放形参最后**
- 和重载混用时容易产生歧义（`sum(int... a)` vs `sum(int a, int... b)`）
- **Collections 工具类**（只针对单列集合）：`addAll` / `shuffle` / `sort` / `binarySearch`（**必须先排序**）/ `max` / `min` / `copy`（目标集合要先有长度）/ `fill` / `swap` / `reverse`

### P22–26 综合练习
- 随机点名器四种：普通 / 带概率 / 不重复 / 带权重；集合嵌套 `Map<String, List<Student>>`
- **P25 跳过**：内容是「带权重点名 + 从文件读数据」，前置是 IO 流，知识点 P23–24 已覆盖

### 上部 P106 StringJoiner（13m，临时插入）
```java
StringJoiner sj = new StringJoiner(", ", "[", "]");  // 分隔符, 前缀, 后缀
sj.add("张三").add("李四").add("王五");
System.out.println(sj);           // [张三, 李四, 王五]
System.out.println(sj.length());  // 含分隔符和前后缀的总长度，不是元素个数
```
- `setEmptyValue("无数据")`：没有元素时输出什么
- `merge(other)`（JDK9）：把另一个 StringJoiner 并进来
- 与 Stream 的关系：`Collectors.joining(", ", "[", "]")` 底层就是它

### P35–42 Stream 流（1h54m，本周重点）

**本质**：Stream 不是集合、不存数据，是一套**流水线**——数据进管道，中间加工，出口出成品。

**三个阶段**：
```java
list.stream()                      // ① 获取流
    .filter(s -> s.length() == 3)  // ② 中间方法（返回仍是 Stream，可链式）
    .map(String::toUpperCase)
    .collect(Collectors.toList()); // ③ 终结方法（返回具体结果）
```

**两个关键认知**：
- **惰性求值**：没有终结方法，中间方法一行都不执行
- **流只能用一次**：执行过终结方法后再用同一个流 → `IllegalStateException`

**五种获取流**（课堂代码 `StreamDemo1`）：
- 单列集合：`list.stream()`（Collection 默认方法）
- 双列集合：无直接方法，走 `keySet()/values()/entrySet().stream()`
- 数组：`Arrays.stream(arr)`（`int[]` 也行，得到 IntStream）
- 零散数据：`Stream.of(...)`
- ⚠️ 坑：`Stream.of(基本类型数组)` 会把**整个数组当成一个元素**，不是展开成多个（代码注释里已记）

**中间方法六个**（`StreamDemo2`）：`filter`（返回 boolean，留 true）/ `map`（转换，**必须 return**）/ `distinct`（靠 equals+hashCode）/ `sorted`（元素须可比，否则 `ClassCastException`）/ `limit` / `skip`（越界不报错）
- 补充：`concat` 合并两个流，签名是 `Stream<? extends T>`，两个流类型不同也能编过，但结果类型会被推断成公共父类
- 中间方法返回新流，**不改原集合**

**终结方法**（`StreamDemo3`）：`forEach` / `count`（返回 **long**）/ `toArray()`（返回 Object[]）/ `toArray(IntFunction)`（指定类型）/ `max` / `min`（返回 Optional）/ `collect`

**collect 三形态**（`StreamDemo4`）：
```java
collect(Collectors.toList())
collect(Collectors.toSet())                                  // 自动去重
collect(Collectors.toMap(键规则, 值规则))                       // 键重复 → IllegalStateException
collect(Collectors.toMap(键规则, 值规则, (o, n) -> n))          // 第三个参数仲裁冲突
collect(Collectors.joining(", ", "[", "]"))                   // 底层就是 StringJoiner
```

**方法引用 `::` 三种形态**（看得懂即可，不要求会写）：
- `System.out::println` = `x -> System.out.println(x)` —— 对象::实例方法
- `Student::getName` = `s -> s.getName()` —— 类名::实例方法（第一个参数当调用者）
- `Integer::parseInt` = `s -> Integer.parseInt(s)` —— 类名::静态方法

### P40–42 三道练习：**独立完成** ✅

| 文件 | 做了什么 |
|---|---|
| `test/Test1.java` | 1–10 过滤奇数留偶数，`filter(i -> i % 2 == 0)` → collect 成 List |
| `test/Test2.java` | `"姓名,年龄"` 字符串 → `filter(年龄 >= 24)` → `toMap(姓名, 年龄)` |
| `test/Test3.java` | 男演员过滤 3 字名 + limit(2)、女演员姓"杨" + skip(1)、`Stream.concat` 合并 → `map` 成 `Actor` 对象 → List |

三份都能跑通，逻辑和题目要求一致。

## 代码自查（对照自己写的代码找毛病）

1. **Test3 数据「吴签24」缺逗号 → 危险**
   `"吴签24".split(",")` 只有 1 个元素，走到 `map` 里的 `[1]` 会 **`ArrayIndexOutOfBoundsException`**。现在没崩，纯粹因为 `[0].length() == 3` 把 4 个字的它过滤掉了。**修法**：改数据为 `"吴签,24"`，不要靠过滤条件挡。
2. **split 被重复调用 3 次**
   Test2：filter 里 1 次 + toMap 里 2 次；Test3 同样。**修法**：先 `map(s -> s.split(","))` 拿到 `String[]`，或抽 `parseName(s)` / `parseAge(s)` 两个小方法。
3. **`toMap` 没写仲裁函数**
   Test2 三个人姓名恰好不重复才没报错。真实数据有同名 → `IllegalStateException: Duplicate key`。
4. **收集进 HashMap 不保证顺序**
   打印顺序 ≠ 原集合顺序。要顺序：`toMap(k, v, (a,b) -> b, LinkedHashMap::new)`。
5. Test3 有个没用到的 import：`java.util.function.Function`。

## LeetCode 350 两个数组的交集 II ✅（week07 配额 2/5）
- **第一版错在用 `HashSet`**：Set 只记「有没有」不记「有几个」，`[4,9,5]` vs `[9,4,9,8,4]` 会输出 `[9,4,9,4]`（重复的 9 被判两次 true）
  - 附带两个编译错：`import java.Arraylistl`（包名+类名都错）、`Arraylist` 大小写
- **改 Map 计数后通过：61/61，3ms（击败 89.16%），内存 44.3MB**
- 核心思路：`getOrDefault` 记账 → 遍历 nums2 时账上还有就取一个并 **减 1**（划账），和 242 单 Map 完全同一套路
- 复杂度：时间 O(n+m)，空间 O(min(n,m))

## 复测 5 题（week06 假会清单，闭卷）→ **1 对 / 1 半 / 3 错**

| # | 项目 | 结果 | 要点 |
|---|---|---|---|
| ① | lambda 返回值 | ✅ | 省掉的四样（构造+类体、`@Override`、方法名、返回值类型）全对；改写匿名内部类没写出来 |
| ② | 泛型通配符 | ❌ | **方向记反**。正确：`? extends Number` **不能 add**、只能读成 `Number`；`? super Integer` **能 add**、只能读成 `Object` |
| ③ | ArrayList 复杂度 | ❌ | 说成了 LinkedList 的特性。正确：`get(i)` **O(1)**、尾部 add 均摊 **O(1)**、头部 add **O(n)**（后面整体后移） |
| ④ | 二叉查找树 | ⚠️ | 中序遍历结果**升序**没说；退化条件 = **按有序顺序插入**；退化后查找 **O(n)** |
| ⑤ | HashSet 去重 | ❌ | 因果链错。正确：只重写 equals → hashCode 仍按**地址**算 → 落**不同桶** → **equals 压根不会被调用** → 去重失效 |

- 新增认知：**`equals` 只在同一个桶内才会被调用**
- ②③⑤ **进 11.4 机动日重测**；④ 并入 11.6 总复习

## 没完成的
- P18–19 TreeMap 源码（10.31 已定为主动砍掉）
- 欠账 P146–149（排 11.3）

## 计划调整（11.1 晚）
- 力扣：**已到 2/5**，剩 11.2 / 11.3 / 11.4 每天 1 题即清零，节奏回到安全区
- 219 已于 week06 day07 通过（HashSet 版 72/72），**不是欠账**——此前判断有误，已更正
- **复测没过**，原定「11.6 一次清掉」改为：**11.4 重测 ②③⑤ 三项**，④ 并 11.6 模拟面试一起过
- 11.2 异常是⭐必会内容，**视频不打折**

## 明天计划（11.2 周一）
- [ ] **P52–62 异常**（1h47m，正常速度，吃透）
- [ ] 力扣 1 题（2/5 → 3/5），建议 **387 字符串中的第一个唯一字符**（Map 计数，10 分钟量级）
- [ ] 当天 commit：第7周Day04笔记
# 第 6 周 Day 03 - 2026.10.25

## 今天学了（P185–191）
### P185 Collection 顶层接口
- `Collection` 是**接口**，必须用实现类（`new ArrayList<>()` 赋给 `Collection<String>` 引用）
- 共性方法：`add` / `clear` / `remove(E)` / `contains` / `isEmpty` / `size`
- **没有索引方法**——`Collection` 只定义共性，Set 无序无索引，所以 `remove(int index)` 只在 List 里
- `add` 返回值：List 恒 true；**Set 已存在时返回 false**
- `contains` 底层依赖 **equals**；自定义类不重写 equals 就比地址（内容相同的新对象判不出来）

### P186 迭代器
- 三方法：`iterator()` 获取迭代器 → `hasNext()` 判断 → `next()` 取值并移动指针
- **四个细节**：
  1. 没有更多元素还调用 `next()` → `NoSuchElementException`
  2. **指针不会复位**——一个迭代器对象只能用一次，复用要重新 `iterator()`
  3. 循环里**只能调用一次 `next()`**（多调会跳过元素）
  4. 遍历中不能用集合自身方法增删（→ `ConcurrentModificationException`），要用 `iterator.remove()` / `ListIterator.add()`

### P187–188 增强 for / lambda / List 五种遍历
- 增强 for 底层就是迭代器，JDK5 起的语法糖；**只有单列集合和数组能用**；改循环变量不影响集合
- List 五种遍历与场景：
  | 方式 | 适用 |
  |---|---|
  | 迭代器 | 遍历中要**删除** |
  | 列表迭代器 | 遍历中要**添加** |
  | 增强 for | 只遍历 |
  | lambda `forEach` | 只遍历（更简） |
  | 普通 for | 需要**索引** |
- List 独有方法：`add(index, e)` / `remove(int index)` / `set(index, e)` / `get(index)`

### P189 数据结构（概念）
- 栈：后进先出；队列：先进先出
- 数组：内存连续 → **查询快、增删慢**
- 链表：元素游离 → **查询慢、增删快**

### P190–191 源码（1.5 倍速，留印象）
- ArrayList 底层数组 + 扩容机制
- LinkedList 底层双向链表；迭代器源码

## 快检（11 题：7 全对 / 3 半对 / 1 小错）
- ✅ add 返回值含义、isEmpty vs size、迭代器三方法、迭代器四细节、预测输出 `b,d,c`、五种遍历场景、数据结构四连、手写迭代器删除
- 半 ✅ **Collection 删第一个元素**：只能 `iterator()` → `next()` → `remove()`，或按元素 `remove(Object)`；**Collection 无索引**
- 半 ✅ **contains 不重写 equals 不报错**，只是变成比地址；重写后内容相同的新对象也能判出
- 半 ✅ **遍历中 `list.remove` 抛 `ConcurrentModificationException`**（当时只说"会报错"，异常名没报出）
- 小错 手写题在循环内打印了被删元素（建议只在最后打印集合）

## LeetCode（week06 配额 2/5）
### 217 存在重复元素 ✅ 79/79，25ms（13.07%）
- `Arrays.sort` + 相邻比较；O(n log n)
- **O(n) 版待做**：HashSet 边走边判（P197 后）

### 219 存在重复元素 II ⏳ 68/72，**超时未过**
- 暴力窗口版逻辑正确（创建用例全过），但 **O(n·k)**，k=50000 时 TLE
- 过程：先写成 `j <= k`（把 k 当最大下标）→ 修正为 `j <= i + k`
- **待办**：学完 P197 用 HashSet/滑动窗口重做（已记入 PLAN 提醒）

## 没完成的
- **P192–199**（泛型 + 源码/数据结构/Set 系列）顺延
- 力扣 week06 仍 2/5（还差 3 题，截止周四 10.29）
- 欠账 P146–149 未动

## 明天计划（10.26 周一）
- [ ] **P192–193 泛型**（正常速度，重点）+ P194–199（源码/数据结构倍速留印象）
- [ ] 力扣 1 题（2/5 → 3/5）
- [ ] 当天 commit：第6周Day04笔记
# 第 7 周 Day 01 - 2026.10.30

## 今天学了（下部 P2–13 Map / HashMap / TreeMap）
### Map 基础
- **双列集合四特点**：一次存一对（键+值）；键不可重复、值可重复；键值一一对应；键+值整体叫 **Entry 对象**
- **常用 API**：`put`（键不存在返回 null，存在则覆盖并返回旧值）/ `remove`（返回值，键不存在返回 null）/ `clear` / `containsKey` / `containsValue` / `isEmpty` / `size`
- **三种遍历**：
  ```java
  // ① 键找值
  Set<K> keys = map.keySet();
  for (K k : keys) { V v = map.get(k); }

  // ② 键值对
  Set<Map.Entry<K,V>> entries = map.entrySet();
  for (Map.Entry<K,V> e : entries) { e.getKey(); e.getValue(); }

  // ③ lambda
  map.forEach((k, v) -> ...);   // 对应函数式接口 BiConsumer（一个抽象方法 accept，两个参数）
  ```
- `entrySet()` 返回 **Set** 的原因：键唯一 → Entry 不重复 → 天然满足 Set 语义；且 Entry 只用于遍历，不需要索引

### HashMap / LinkedHashMap / TreeMap
- `HashMap<Student, String>`：**自定义对象当键，equals + hashCode 必须一起重写**，否则"内容相同的另一个对象"按地址算 → 落不同桶 → `get` 不到（week06 HashSet 的同一机制）
- **统计套路**：`map.put(k, map.getOrDefault(k, 0) + 1)`（比 containsKey + if/else 少一层分支）
- `LinkedHashMap`：在 HashMap 基础上维护**插入顺序**
- `TreeMap`：底层**红黑树**，按键**自然排序**；构造时传 `Comparator` 可自定义顺序（如 `o2 - o1` 降序）
- **TreeMap 的键必须可比较**：键类实现 `Comparable`，或构造时传 `Comparator`；否则 `put` 直接抛 **`ClassCastException`**
- `compareTo` / `compare` 返回 **0 视为重复键**，会覆盖旧值

## 快检（10 题：5 对 / 2 半对 / 3 错）
- ✅ 双列四特点、put/remove 返回值、BiConsumer（一个抽象方法、键值两参数）、TreeMap 红黑树 + 自然排序
- 半 ✅ 三种遍历：方向对，但骨架写不完整 → 已补完整代码
- 半 ✅ `entrySet()` 为什么是 Set：直觉"去重"对，精确理由是**键唯一 → Entry 天然不重复**
- ❌ **自定义对象当键不重写 equals/hashCode**：不是"能取到地址值"，而是**取不到（返回 null）**——hashCode 按地址算 → 落不同桶，即使同桶 equals 也比地址
- ❌ **`getOrDefault`**：没学过 → 已补：`map.getOrDefault(k, 默认值)`，统计固定套路 `put(k, getOrDefault(k,0)+1)`
- ❌ **TreeMap 缺比较器**：不是"随机"，是 **`ClassCastException`**（无法比较大小）

## LeetCode（week07 配额 1/5）
### 242. 有效的字母异位词 ✅
- **两 Map 版**（自己写）：分别统计 s、t，再 `equals` 比较
  - 踩坑：`import.java` 写错、数组没有 `add`、`if{...}` 漏括号、`count2` 拼写、两个 Map 用混、**`hs1 == hs2` 比地址**（应 equals）、多余的数组
  - 通过 56/56
- **单 Map 版**（看懂后重写）：先数 s 记账，再遍历 t **逐张划账**
  - `containsKey` 失败 → t 多字符 → false
  - 划到负数 → t 该字符更多 → false
  - 长度相等 + 全部划平 → true，无需最后检查账本归零
  - 通过 56/56，15ms
- **核心认知**：单 Map = 空间优化 + 提前退出；两 Map = 逻辑直白。同一算法两种写法

## 没完成的
- **P14–17 HashMap 源码今晚未看**（22:00 后状态不佳，主动停止；改明天倍速或按五要点速览，笔记如实记）
- P2–13 的课后练习
- week07 配额 4 题（截止 11.5 复盘前）
- 欠账 P146–149（排 11.3）
- week06 待补复测 5 题（lambda 返回值 / 通配符 / ArrayList 复杂度 / 二叉查找树 / HashSet 去重）

## 计划调整（10.30 晚）
- 发现原 PLAN 漏排 **上部 P169–173 时间类**（路线点名必看，约 2h25m）
- 已补进 **11.4 周三机动日**；week07 视频总量修正为约 **10h36m**

## 明天计划（10.31 周六）
- [ ] **P14–19 HashMap / TreeMap 源码**（1.25–1.5 倍速，源码速览留印象）
  - 若状态一般，可按"五要点速览"替代逐帧：hash 高低位异或 / 2 的幂容量 / put 流程 / 扩容 0.75 / 树化双条件
- [ ] **P20–21 可变参数 / Collections 工具类**
- [ ] 力扣 1 题（1/5 → 2/5）
- [ ] 当天 commit：第7周Day02笔记
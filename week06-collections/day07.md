# 第 6 周 Day 07 - 2026.10.29

## 今天发布
- 🚀 **掘金第 5 篇**：《Java 泛型通配符：`? extends` 和 `? super` 到底怎么选》
  https://juejin.cn/post/7701974935602446345
  素材 = week06 泛型专项（泛型不变性 → 通配符读写 → PECS → JDK 实例 → 面试模板）

## 今天学了（P197 收尾 + P198–199）
- **P197 HashSet / LinkedHashSet**（补完剩余约 19 分钟）
  - HashSet 底层：数组 + 链表 + 红黑树；无序性来自哈希值定位
  - 去重靠 **hashCode + equals** 两步：hashCode 定位桶，equals 判桶内是否同一元素
  - **自定义对象去重两个都要重写**：只重写 equals → hashCode 仍按地址算 → 落不同桶 → 判不重复
  - LinkedHashSet：在 HashSet 基础上**维护插入顺序**
- **P198 TreeSet 第一种排序（自然排序）**
  - 元素实现 `Comparable` 接口，重写 `compareTo`
  - `compareTo` 返回 0 视为重复，TreeSet 会去重
- **P199 TreeSet 第二种排序（比较器）**
  - 构造时传入 `Comparator`，重写 `compare`
  - 元素可以不实现 Comparable；比较器优先级高于自然排序
  - ⚠️ 课后综合练习未做

## LeetCode（week06 配额 5/5 清零 🏁）
- **217 存在重复元素（HashSet 版重做）** ✅ 80/80，15ms
  - `set.add(num)` 返回 false 即重复，一次调用完成"判断 + 加入"
- **219 存在重复元素 II（HashSet 版重做）** ✅ 72/72，33ms
  - 原暴力 O(n·k) 68/72 超时 → 滑动窗口 Set，只保留最近 k 个元素
  - 关键：`if (set.size() > k) set.remove(nums[i - k]);`
- **349 两个数组的交集** ✅ 57/57，2ms
  - 两个 Set：一个查表、一个收集结果；Set 转 int[] 用"先开数组再遍历装"
  - 过程踩坑：`contain` 拼写、`remove` 应为 `add`、`r`/`res` 变量名混用

## week06 周复盘（按流程：先测后写）
- **闭卷自测 20 题：10 对 / 8 半对 / 2 错**
  - 错：lambda 三段语义、lambda 返回值形式（`(a,b) -> return ...` 不存在这种写法）
  - 半对集中在：通配符读出类型、ArrayList 查询复杂度、二叉查找树规则、红黑树对比、HashSet 去重两方法
- **专项复测 6 题**：只完成第 1 题（Comparator lambda 改写正确 ✅），其余 5 项未答，已列入下周待补
- `week06-review.md` 已提交（含假会 9 条 + 待复测 5 项）

## 没完成的
- P198–199 课后综合练习
- 专项复测剩余 5 题：lambda 返回值 / 通配符方向 / ArrayList 复杂度 / 二叉查找树 / HashSet 去重
- 欠账 P146–149（P155 / P157 / P174，约 1h37m）→ week07 的 11.3

## 明天计划（10.30 周五，week07 Day01 启动）
- [ ] 下部 **P2–13 Map / HashMap 基础**（约 1h57m）
  - 重点：双列集合特点、Map 常用 API、三种遍历（键找值 / 键值对 / lambda）
- [ ] 力扣 week07 新配额开张：1 题（5 题，11.5 复盘前清零）
- [ ] 当天 commit：第7周Day01笔记
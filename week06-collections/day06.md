# 第 6 周 Day 06 - 2026.10.28

## 今天学了（P194–196 + P197 前半）
- **P194–196 数据结构**（1.5 倍速，留印象级）
  - 二叉树 / 二叉查找树：左子树 < 根 < 右子树，查找速度取决于**树高**
  - 平衡二叉树（AVL）：插入/删除后靠**旋转**维持左右高度差 ≤ 1
  - 红黑树：**不是严格平衡**，靠颜色规则保证"最长路径 ≤ 最短路径 2 倍"，换取插入删除时旋转次数更少 → `TreeMap/TreeSet` 底层用它
  - 退化场景：有序数据依次插入会让二叉查找树变成斜链，退化成链表 O(n) —— 这正是平衡树要解决的问题
- **P197 HashSet / LinkedHashSet**（59 分钟，**看到约 40 分钟，剩约 19 分钟**）
  - 待补完：HashSet 无序性来源、去重靠 hashCode + equals、自定义对象去重为何两个都要重写

## 快检（4 题，2 对 / 2 未讲到）
- ✅ 二叉查找树规则 + 查找速度取决于树高
- ✅ 红黑树不是严格平衡
- — 树退化、各结构底层对比：课程此处未讲，已口头补充
  - 补充：`ArrayList` 数组（查快删慢）/ `LinkedList` 双向链表（查慢删快）/ `HashSet` 底层待 P197 补完

## LinkedList 回补（发现印象浅）
- P191 当时 1.5 倍速合讲源码，印象浅属正常，**不重看整集**，10 分钟补三个点：
  1. 底层双向链表：节点存 `prev / item / next`
  2. 独有能力来自实现 `Deque`：`addFirst/addLast/getFirst/getLast/removeFirst/removeLast`
  3. 选型：日常用 `ArrayList`；频繁头尾增删用 `LinkedList`（作为队列/栈时）
- 练习：`setadvanced/src/com/itheima/List/ListDemo3.java`，六个头尾方法全覆盖，运行正确 ✅
- 补充细节：`getFirst/removeFirst` 空链表抛 `NoSuchElementException`；`peekFirst/pollFirst` 空时返回 null

## 环境折腾（今天的主要时间消耗）
- **IDEA 版本核查**：2026.2.2 Ultimate（`IU-262.10315.125`），JBR 25，项目 JDK 21（temurin-21）
- **项目 SDK 检查**：15 个项目中 14 个是 JDK 21 ✅；只有旧的 `2/` 项目仍是 1.8（不影响，未改）
- **UI 切换折腾**：试了新 UI（Islands）→ 不喜欢 → 已还原经典 UI（`themeID=ExperimentalDark` + 标记 false）
- **ptg 插件**：2020 年的插件，装后菜单出现但点功能报 `NoClassDefFoundError: WriteCommandAction$Simple`
  - 根因：**平台已移除该类**（2026.2.2 所有 jar 都搜不到），属插件 API 不兼容，与破解无关、与打包无关
  - 处理：**卸载 ptg**，改用 IDEA 自带 `Alt+Insert` 生成 JavaBean
  - 替代：装了 **MyBatisX 1.7.7**（支持 251–263），留给 JavaWeb 阶段 MyBatis 用

## LeetCode
- 今日未做，week06 配额仍 **3/5**（167 / 217 / 121）
- 待办：**217 / 219 HashSet 版**（219 上次 68/72 超时）

## 没完成的
- **P197 剩约 19 分钟**
- P198–199 TreeSet
- 力扣 2 题（3/5 → 5/5）
- 欠账 P146–149（P155 / P157 / P174）

## 明天计划（10.29 周四，week06 收官日）
- [ ] 补完 **P197**（约 19 分钟）
- [ ] **P198–199 TreeSet**（约 1h11m）
- [ ] **回做力扣 217 / 219（HashSet 版）**，配额 3/5 → 4/5 或 5/5
- [ ] **week06 周复盘 + 掘金第 5 篇**（固定动作，不顺延）
- [ ] 当天 commit：第6周Day07笔记 + week06-review.md
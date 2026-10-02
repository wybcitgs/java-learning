# 第 3 周 Day 01 - 2026.10.02

## 今天学了（P97–103）
- static：静态变量（全类共享）、内存图、static 方法（工具类）、注意事项
- final 修饰变量：值/地址定死只能赋一次（final 引用 = 地址不可改，内容可改）
- 枚举：enum 定义、构造器传参、isWorkday、values() 遍历、switch 匹配枚举

## 动手 3 题（全部验收通过）
- ArrayUtil 工具类：max/sum/print/reverse/indexOf 全 static + 私有构造器防 new
- Dog 计数器：static int count 全类共享——实测不加 static 每次都是 1，加了是 3 ✅ 悟到点
- WeekDay 枚举：构造器存中文名 + switch 表达式 + int 反例对比注释

## LeetCode 27 移除元素（本周第 1 题）
- 弯路：一开始写成双层嵌套循环，还混进 return nums（返回类型是 int），写不下去
- 纠正：**快慢指针 = 单层循环 + 两个 int 变量**，不是两层循环
- 关键迁移：这题和移动零同构——把「!=0」换成「!=val」，最后 return count（就是新长度）
- 一次写对，两个用例手算通过。快慢指针模型立住了

## 小插曲
- 口答 final 方法/类的作用超纲了——视频（P101）只讲了修饰变量，方法/类的禁令是
  继承章的内容，等课程讲到再学（final = 终态：变量定值、方法定实现、类定结构）

## 明天计划（10.3 周六，6h）
- [ ] P104–113 继承（extends、方法重写、super、权限修饰符）——本周主菜
- [ ] 动手：设计一个继承结构
- [ ] LeetCode 日清 1 题
- [ ] 当天 commit：第3周Day02笔记

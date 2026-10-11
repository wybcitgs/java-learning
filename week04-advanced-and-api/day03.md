# 第 4 周 Day 03 - 2026.10.11

## 今天学了（P121–124 内部类）
- 成员内部类、静态内部类、局部内部类、匿名内部类
- 8 题快检验收：2 对 / 3 半对 / 3 错——假会 3 条（成员内部类访问权限、匿名类是什么、必须重写什么）
- 补实验：成员内部类 vs 静态内部类的访问区别

## 学生管理系统 v0.2 升级完成 🎉
- **Config**（static + final）：MAX_SIZE、SYSTEM_NAME、VERSION
- **MenuOption**（枚举）：ADD/DELETE/SEARCH/SHOW/EXIT，`fromIndex()` 数字转枚举
- **Person**（抽象类）→ **Student**（继承）：super 构造、getRole() 重写、show() 复用
- **Searchable**（接口 + default）：findById + exists（替代判重 for 循环）
- **StudentSystem**：implements Searchable，菜单用枚举，查询功能上线
- 全量测试通过：添加、判重、查询、查看、退出 ✅

## LeetCode 35 搜索插入位置
- 第一版 3 个 bug：else 导致循环只跑一次、nums[i+1] 越界、条件逻辑错
- 纠正后通过 ✅（核心：找到第一个 ≥ target 的位置，return nums.length 放循环外）

## 没完成的
- 无，今日全清

## 明天计划（10.12 周一，6h）
- [ ] P125–127 String 开篇（API 文档、String 概述、比较与遍历）
- [ ] 动手：String 小练习（equals vs ==、遍历字符串）
- [ ] LeetCode 日清 1 题
- [ ] 当天 commit：第4周Day04笔记

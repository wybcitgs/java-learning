# 第 4 周 Day 05 - 2026.10.13

## 今天学了（P128–130）
- **P128 字符串的截取、替换和其他常见方法**
  - `substring(begin, end)`：含头不含尾；`substring(begin)`：从 begin 截到末尾
  - `replace(char, char)` / `replace(String, String)`：两个重载，**返回新 String**
  - `split(分隔符)`：按分隔符切成 `String[]`
  - `trim()`：只剪**两端**空格，中间不动
- **P129 StringBuilder**
  - 存在意义：String 不可变，循环拼接会产生大量垃圾对象；StringBuilder 在**同一对象上改**
  - `append()` 返回 `this`，**可以链式**调用
  - `reverse()` 是 StringBuilder 特有，String 没有（因为改不了内容）
  - String ↔ StringBuilder：`new StringBuilder(str)` / `sb.toString()`
- **P130 字符串的四个算法小练习**（跟着视频敲了一遍，未独立默写）

## 12 题快检（8 对 / 4 错）
- ✅ `substring(2,5)` 取 2,3,4，不含 5（含头不含尾）
- ✅ `substring(3)` 从索引 3 截到末尾
- ✅ 为什么要用 StringBuilder：循环拼接造大量垃圾对象，它在同一对象上改
- ✅ `reverse()` 是 StringBuilder 特有，因为 String 不可变、改不了内容
- ✅ String ↔ StringBuilder 互转（`new StringBuilder(str)` / `sb.toString()`）
- ✅ `trim()` 只剪两端（abc 移到中间时中间空格不动）
- ✅ `"2026-10-13".split("-")` → `{"2026","10","13"}`，长度 3
- ✅ `sb.append("c").append("d")` 链式成立，`toString().length()` = 4
- ❌→✅ **假会 ①**：`append` 返回的不是"拼接后的字符"，而是 **`return this`**（返回自己），所以能链式
- ❌→✅ **假会 ②（核心洞）**：String 不可变只停在名词，没落成"方法必须接住"的动作
  - `s.replace('l','L');` 执行完 `s` 没变——**没接住新返回的串，白干**
  - **铁律：String 的所有"修改"方法（substring / replace / trim / toUpperCase…）都不改自己，只返回新串**；不接住 = 白干
- ❌→✅ **假会 ③**：`toUpperCase` / `toLowerCase` 是"整串变换"，`replace` 是"定点替换"
  - 要把 `"hello"` 变成 `"heLLo"`，只能用 `s = s.replace('l','L')`；`toUpperCase` 会误伤 h/e/o
- ❌→✅ **假会 ④**：第一版交换写反——`s[left] = temp` 方向错了，`temp` 没备份就先用（局部变量未初始化直接编译报错）；交换三步顺序必须是"先备份 → 再覆盖 → 最后回填"

## 补漏（快检里没学到的）
- `replace` 两个重载（`char,char` / `String,String`）返回类型都是 `String`（不是 void）
- `split` 三个坑：
  1. 点是正则元字符，要用 `"\\."` 或 `"[.]"`
  2. 空段保留：`"a,b,,c".split(",")` 长度 4
  3. 默认丢**尾部**空串：`"a,b,,".split(",")` 长度 2，加 `-1` 才保留
- `trim()` 只剪两端

## LeetCode 344 反转字符串
- **手写双指针**（不许用 `new` 数组，要求原地）
- 第一版 3 个 bug：`temp` 赋值方向反了、交换三步顺序乱、for 套 while 结构冗余
- 改完一遍通过 ✅
- **辨清一件事**：`StringBuilder.reverse()` 虽然能出结果，但这题参数是 `char[]` 且要求**原地**，用它属于绕路（造 2 个多余对象）
  - **判据：先看输入类型和输出要求，再选工具**，别先想"我会哪个方法"
- 双指针收口条件：`while (left < right)` 而非 `<=`
  - `left == right` 时只剩一个元素，无需自换；`<=` 只是"碰巧对"，多干一次无用功
- 顺带：`char[] ↔ String` 是 `new String(arr)` / `s.toCharArray()`

## 工具迁移
- 今日同时完成工具迁移：VS Code 里的 Claude Code 插件 + 外接小米 mimo → **DeepSeek Harness (DSH)**

## 没完成的
- **P131 字符串计算（难）未看**
- **P132 字符串的作业未看**

## 明天计划（10.14 周三）
- [ ] P131 字符串计算（难）
- [ ] P132 字符串的作业
- [ ] LeetCode 日清 1 题
- [ ] 当天 commit：第4周Day06笔记

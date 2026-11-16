# 第 9 周 Day 01 - 2026.11.16

> 状态：**今天当休息日**，视频没看（原计划 P40–45），只做了一道力扣

## 今天做了

### LeetCode 14. 最长公共前缀 ✅（week09 配额 1/5）

**通过 126/126，1ms，击败 78.01%**

```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) return "";

    for (int i = 0; i < strs[0].length(); i++) {
        char c = strs[0].charAt(i);
        for (int j = 1; j < strs.length; j++) {
            if (i == strs[j].length() || strs[j].charAt(i) != c) {
                return strs[0].substring(0, i);
            }
        }
    }
    return strs[0];
}
```

**思路：纵向扫描**——外层遍历字符下标 `i`，内层遍历每个字符串，一列一列往下比，遇到不一致立刻返回前 `i` 个字符。

### 这题学到的四个点

**1) `substring(begin, end)` 是左闭右开**

```java
"flower".substring(0, 2)   // "fl"（取下标 0、1，不含 2）
```

所以 `i` 是"第一个不匹配的位置"时，直接用 `substring(0, i)` 正好——**不用 +1**。

**2) `||` 的短路顺序不能反**

```java
if (i == strs[j].length() || strs[j].charAt(i) != c)
```

必须先判长度：某个字符串比基准短时，`charAt(i)` 会抛 `StringIndexOutOfBoundsException`。顺序反了就崩。

这和之前括号题的 `st.isEmpty() || st.pop() != c` 是同一个套路——**先判边界，再取值**。

**3) `return` / `break` / 标签的区别**

| 场景 | 用什么 |
|---|---|
| 结果已确定，方法该结束 | **`return`** |
| 跳过当前元素继续 | `continue` |
| 跳出当前这一层 | `break` |
| 真要跳多层且后面还有代码 | 标签 `break outer`（少用） |

这题一旦发现不匹配，答案就确定了，所以内层直接 `return`，不需要 `break`。

**4) `String` 是 `length()` 方法，数组是 `.length` 属性**

写代码时把 `strs[j].length()` 写成了 `strs[j].length`，编译不过。这对是 Java 里最容易混的。

### 今天的编译错误（都记一下）

| 错误 | 原因 |
|---|---|
| `strs[j].length` | String 没有 length 属性，是 `length()` 方法 |
| `length < i` | 应该是 `<=`，因为 `i` 是下标，`length == i` 时已越界 |
| 循环外用 `i` | `i` 在 for 里声明，出循环就没了作用域 |

## 没完成的

- **P40–45**（SpringBootWeb 入门 + HTTP 协议，原速 1h46m）→ 明天补
- 力扣配额 **1/5**（周日 11.22 前清零）

## 计划调整

- week09 第一天当休息日，后端内容整体顺延一天
- 原计划 week09 主线：P40–49（SpringBootWeb + HTTP + 三层架构 + IOC/DI）+ P50–61（MySQL），总量约 8h30m，**需要压到 6 天完成**

## 明天计划（11.17 周二 · week09 Day02）

- [ ] **P40–42** SpringBootWeb 入门（课程安排 / 入门程序 / 入门解析），约 49m
- [ ] **P43–45** HTTP 协议（概述 / 请求协议 / 响应协议），约 57m
- [ ] 力扣 1 题（1/5 → 2/5）
- [ ] 当天 commit：第9周Day02笔记
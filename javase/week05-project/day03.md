# 第 5 周 Day 03 - 2026.10.18

## 今天学了（P137–139 登录闭环 + 角色体系铺垫）
### P137 验证码 + 登录业务完善
- **验证码 `getCode()`**：5 位 = 4 个随机字母（大小写混合）+ 1 个随机数字，数字位置随机
  - 字母池：`ArrayList<Character>` 里用 `(char)('a'+i)` / `(char)('A'+i)` 装 52 个字母
  - **容器三换**：StringBuilder 拼（String 只能拼不能改）→ `toCharArray()` 进 `char[]`（可变，能交换数字位置）→ `new String(arr)` 转回成品
  - `equalsIgnoreCase` 比验证码（忽略大小写）
- **登录完整流程**（两步查落地）：
  1. 录用户名 → `contains` 不存在 → 提示"请先注册"，`return` 回主菜单
  2. `findIndex` 找到该用户 → `status` 为 false → 提示"已被禁用，联系客服"，`return`
  3. **3 次密码机会**（for 循环）：每次输密码后都要过验证码（内层 while：验证码错了 continue 重来，对了 break）
  4. 密码对 → **桥接**：`new FightingGame().gameStart(username)` 游戏启动
  5. 密码错 → 提示剩余次数；第 3 次错 → `u.setStatus(false)` 锁号
- **`return` vs `break`**：`return` 退出整个方法回主菜单（重新选择）；`break` 只跳出密码 for 循环（登录成功后方法自然结束，也回到菜单）
- **`u.setStatus(false)` 为什么有效**：`u` 是 `list.get(index)` 拿到的**集合中那个对象的引用**，改它 = 改在册账号；new 一个新 User 则对在册账号毫无影响

### P138 角色类体系（继承）
- **Character 父类**：name / HP / maxHP / attack / defense + 四个方法
  - `isAlive()`：HP > 0
  - `heal(amount)`：回血但**不超过 maxHP**（所以构造时要存 maxHP = HP）
  - `takeDamage(damage)`：扣血且 **HP 不得为负**（<0 归 0 兜底）
  - `show()`：属性拼串展示
- **HeroCharacter extends Character**：多一个 `ArrayList<String> skillList`；`showSkill()` 用 StringBuilder 拼技能列表（最后一个不加逗号）
- **EnemyCharacter extends Character**：多 skill + `boolean defending` 防御状态
  - **重写 `takeDamage`**：防御状态中伤害减半 → `defending = false`（buff 只持续一次受击）→ `super.takeDamage(damage)` 交给父类扣血
  - **为什么要 super**：父类里还有"HP 归 0"的兜底，直接写 `HP -= damage` 血会减成负数

### P139 战斗界面 + 属性分配
- **FightingGame**：`gameStart(username)` = 标题 → 创建玩家 → 展示属性技能 → 建 4 个敌人列表（初级战士/敏捷刺客/重装坦克/神秘法师，属性表已在注释里）
- **`createPlayerCharacter` 属性分配**（今天最难的一段）：
  - 共 20 点，三个属性用**两个数组**（提示语 `String[] attributes` + 点数 `int[] values`）配合一个 for 循环分配
  - 校验：负数 → 按 0；超剩余点数 → 按剩余
  - 最终公式：生命 = `100+点×10`、攻击 = `10+点×2`、防御 = `点×1`（**每个属性 = 基础值 + 换算**，不投不等于 0 攻击）
  - 技能三个写死进 skillList：普通攻击 / 强力一击 / 生命汲取

## 今天修的 bug（takeDamage 减半写岔）
- 原代码 `damage /= 2>1?damage/2:1` —— 解析成 `damage / (damage/2)`，任何伤害都被削成 2，不是减半
- 根因：三元条件部分是"**整个除法的结果**"，不能按空格目测分组；`/=` 让除法嵌了两层
- 改为 `damage = damage / 2 > 1 ? damage / 2 : 1`（/2 不足 1 按 1），已验证 ✅
- 记忆点：`a / b > c ? x : y` 优先级从紧到松是 `/` → `>` → `?:`，读三元必须按运算顺序拆

## 8 题快检（6 对 / 2 错）
- ✅ return vs break / 引用 vs 新对象 / maxHP 服务 heal / defending 必须重置 / super 的归 0 兜底 / 全投生命 = 300,10,0
- ❌ **假会 ①（Q1 强转）**：以为 list 是 String 类型——实际是 `ArrayList<Character>`；强转真因：**char 参与算术自动升级成 int**，而 `add` 要 Character，int 装箱装的是 Integer 装不成 Character，必须 `(char)` 变回字符
- ❌ **假会 ②（Q2 容器三换）**：核心不是类型相加，而是**交换位置 String 做不到**（不可变铁律）——所以进可变的 `char[]` 里用 temp 交换，再转回 String

## 没完成的
- 战斗逻辑（攻击/回合循环）——P140+ 的事
- LeetCode week05 仍 **1/5**（今日未做；截止周四 10.22 复盘前）

## 明天计划（10.19 周一）
- [ ] **P140+ 跟做**：战斗逻辑（回合制、技能效果触发）——重装坦克的防御减半今天刚修好，正好在战斗里验证
- [ ] **LeetCode 209**（长度最小的子数组，中等）：1/5 → 2/5，别再拖了，周四是硬线
- [ ] 当天 commit：第5周Day04笔记

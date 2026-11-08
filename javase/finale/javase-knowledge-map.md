# JavaSE 知识地图

## 基础语法

- 8 种基本类型及字节数
- == vs equals
- ⚠️ 重载 vs 重写（同名不同参 / 同名同参）
- final 三态（类 / 方法 / 变量）

## 面向对象

- 多态三条件
- ⚠️ 接口 vs 抽象类
- 内部类四种
- 变量存储：堆 / 栈 / 方法区

## 集合

- List / Set / Map 选型
- ArrayList 扩容：默认 10，1.5 倍
- LinkedList 双向链表
- HashMap put 六步
- 容量 2 的幂：(n-1) & hash 等价取模
- equals-hashCode 因果链：同桶才调 equals
- ⚠️ 泛型擦除及其限制

## 异常

- Throwable → Error / Exception
- ⚠️ 受检 vs 非受检：编译器查不查
- throw vs throws
- catch 顺序：子类在前
- ⚠️ 自定义异常选父类：RuntimeException 免强制处理

## 常用 API

- ⚠️ String 不可变 / StringBuilder 可变
- Integer 缓存 -128~127 + 拆箱 NPE
- Arrays 七方法（binarySearch 返回 -插入点-1）
- Math 三个反直觉（round 负数 / abs 溢出 / random 区间）

## 时间与数值

- Date / Calendar 三个坑（MONTH 0-11、DAY_OF_WEEK 1=周日、add 改自己）
- ⚠️ JDK8 不可变：plus / with 返回新对象，原对象不变
- DateTimeFormatter：ofPattern + format
- ⚠️ BigDecimal 必须字符串构造
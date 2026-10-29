# Java 14 天速成 · Day 2 学习总结

> **主题**：运算符、条件判断与循环  
> **学习状态**：已完成；5 道练习全部通过  
> **总成绩**：**48.5 / 50**（平均 **9.7 / 10**）  
> **学习背景**：有少量 C/C++ 基础，每日约 1.5～2 小时

## 一、今日学习目标回顾

- 会使用算术、比较、逻辑和复合赋值运算符。
- 能正确组合 `if / else if / else` 条件。
- 能区分传统 `switch` 和 Java 14 的 `switch` 表达式。
- 会使用 `for`、`while`、`do-while`，并理解循环边界。
- 明白 `break`、`continue`、`return` 的作用范围。
- 能独立编写一个持续运行的控制台 ATM 系统。

## 二、核心知识点

### 1. 运算符

| 类型 | 运算符 | 重点 |
| --- | --- | --- |
| 算术 | `+`、`-`、`*`、`/`、`%` | 两个整数相除会截断小数部分；`%` 求余 |
| 比较 | `==`、`!=`、`>`、`<`、`>=`、`<=` | 结果为 `boolean` |
| 逻辑 | `&&`、`||`、`!` | `&&` 与 `||` 短路求值 |
| 自增自减 | `++`、`--` | `i++` 先使用后自增；`++i` 先自增后使用 |
| 复合赋值 | `+=`、`-=`、`*=`、`/=`、`%=` | 注意自动类型转换和可能的溢出 |

```java
int a = 17;
int b = 5;
System.out.println(a / b);           // 3
System.out.println(a % b);           // 2
System.out.println((double) a / b);  // 3.4
```

**易错点：**

- `int / int` 产生整数结果；整数除以 0 抛出 `ArithmeticException`。
- `&&` 左侧为 `false`、`||` 左侧为 `true` 时，右侧不再执行。
- Java 的 `if` 必须接收布尔表达式，不能直接写 `if (1)`。
- 比较 `String` 的**内容**应使用 `.equals()`，而不是 `==`（后者比较引用身份）。

### 2. 条件判断：`if / else if / else`

```java
int score = 91;
if (score >= 90) {
    System.out.println("优秀");
} else if (score >= 60) {
    System.out.println("及格");
} else {
    System.out.println("不及格");
}
```

- `else if` 是互斥分支链：命中一个分支后不继续匹配后面的分支。
- 判断范围时要留意**顺序**，例如应先判断 `score >= 90`，再判断 `score >= 60`。
- 不要在 `if (...)` 后面误写分号：`if (condition);`。

### 3. `switch`：传统写法与 Java 14 写法

**传统 `switch`：**

```java
int day = 6;
switch (day) {
    case 6:
    case 7:
        System.out.println("周末");
        break;
    default:
        System.out.println("工作日或无效编号");
}
```

- 传统写法通常用 `break` 防止贯穿到下一个 `case`。

**Java 14 `switch` 表达式：**

```java
int day = 6;
String type = switch (day) {
    case 1, 2, 3, 4, 5 -> "工作日";
    case 6, 7 -> "周末";
    default -> "无效编号";
};
System.out.println(type);
```

- 箭头分支 `->` 不发生贯穿，不必写 `break`。
- `switch` 表达式可以赋值给变量；带语句块的分支可以用 `yield` 提供表达式结果。
- Java 允许根据 `String` 选择分支，而传统 C/C++ `switch` 不直接支持字符串。

### 4. 循环：`for`、`while`、`do-while`

```java
for (int i = 1; i <= 3; i++) {
    System.out.println(i);
}

int count = 3;
while (count > 0) {
    count--;
}

int choice;
java.util.Scanner sc = new java.util.Scanner(System.in);
do {
    System.out.println("输入 0 退出：");
    choice = sc.nextInt();
} while (choice != 0);
sc.close();
```

- `for`：常用于循环次数明确的场景。
- `while`：先判断条件，有可能一次也不执行。
- `do-while`：先执行再判断，循环体至少执行一次；特别适合命令行菜单。
- 留意 `<` 与 `<=` 的区别，以及循环变量是否更新。

### 5. `break`、`continue`、`return`

| 关键字 | 效果 |
| --- | --- |
| `break` | 退出最近的循环或 `switch` |
| `continue` | 跳过当前轮循环剩余语句，进入下一轮 |
| `return` | 退出当前方法（在 `main()` 中使用将结束程序主方法） |
| `continue outer;` | Java 带标签写法，直接继续指定的外层循环 |

质数统计题中使用了：

```java
outer: for (int i = 2; i <= n; i++) {
    for (int j = 2; j * j <= i; j++) {
        if (i % j == 0) {
            continue outer;
        }
    }
    System.out.print(i + " ");
    num++;
    sum += i;
}
```

其中 `j * j <= i` 利用因子成对出现的性质，只检查到平方根，减少判断次数。在本题的 `n <= 10000` 范围内，这种写法安全。

## 三、与 C/C++ 的关键对比

| 对比项 | Java | C/C++ |
| --- | --- | --- |
| 条件表达式 | 必须是 `boolean` | C/C++ 可将整数等转换为布尔意义 |
| `String` 内容比较 | `s1.equals(s2)` | C++ `std::string` 可用 `==` |
| `switch` 处理字符串 | 支持 `String` | 传统 `switch` 不支持字符串 |
| 新式 `switch` 表达式 | Java 14 正式支持 | 传统 C/C++ `switch` 不是表达式 |
| 循环与短路逻辑 | 与 C/C++ 基本相似 | 与 Java 基本相似 |
| 带标签循环控制 | 支持 `break label`、`continue label` | 传统 C/C++ 不支持此语法 |

## 四、五道练习最终成绩

| 练习 | 核心知识 | 最终成绩 | 状态 |
| --- | --- | ---: | --- |
| 1. 商品分箱计算器 | `Scanner`、整数除法、取余、边界判断 | **9.5 / 10** | 通过 |
| 2. 闰年判断器 | `%`、`&&`、`\|\|`、`if/else` | **10 / 10** | 通过 |
| 3. 星期查询器 | Java 14 `switch` 表达式 | **9.5 / 10** | 通过 |
| 4. 质数统计程序 | 嵌套循环、平方根优化、标签 `continue` | **10 / 10** | 二次提交通过 |
| 5. ATM 银行终端（Boss） | `do-while`、传统 `switch`、金额校验、状态维护 | **9.5 / 10** | 二次提交通过 |
| **合计** | | **48.5 / 50** | **Day 2 过关** |

## 五、真实踩坑与修复经验

### 踩坑 1：`char` 与 `String` 混用（练习 4）

最初写法：

```java
System.out.print(i + ' ');
```

`' '` 是空格字符 `char`，其数值为 32。`int + char` 会做**数值加法**，于是质数 2 被打印成 34。

修正：

```java
System.out.print(i + " ");
```

`" "` 是字符串，`int + String` 会执行**字符串拼接**。这是本次 Day 2 最有代表性的类型错误。

### 踩坑 2：ATM 取款错误提示未细分（练习 5）

初版把 `minus <= 0`（金额非法）和 `minus > money`（余额不足）合并为一个提示。二次修改后用 `if / else if` 分开处理，逻辑更准确，也更符合题目要求。

### 踩坑 3：ATM 的交互反馈不完整（练习 5）

初版成功存取款没有提示，退出时也没有告别语；二次修改增加了成功信息及退出信息。**后续仍可优化：** 在读取金额前先输出 `请输入存款金额：` 或 `请输入取款金额：`。

### 代码规范小提醒

- 建议写 `Scanner sc = new Scanner(System.in);`，保留运算符前后空格。
- Day 2 源码使用 `package day1;` 并不一定编译错误，但为方便整理项目，建议将该天代码放在 `day2` 包内，确保目录匹配。
- 常规 `switch` 的 `case` 中可以用 `{}` 隔开局部变量作用域。
- 不要为了“用上 break”而硬加冗余控制语句；首先保证结构清晰、行为正确。

## 六、ATM Boss 的设计经验

1. 把余额 `money` 放在循环外，避免每次显示菜单时重置为 1000。
2. 在循环内部读取操作编号、通过 `switch` 分发业务。
3. 先验证金额，再进行 `money += plus` 或 `money -= minus`，确保失败操作不修改余额。
4. `switch` 中的 `break` 只退出 `switch`；`main()` 中 `return` 可以结束 ATM 程序。
5. `do-while (true)` 可以做持续菜单，但退出路径必须明确。

## 七、Day 2 自检清单

- [x] 运算符与整数除法
- [x] 逻辑条件与短路求值
- [x] `if / else if / else`
- [x] 传统与新式 `switch`
- [x] `for`、`while`、`do-while`
- [x] `break`、`continue`、`return`
- [x] 质数统计与循环优化
- [x] ATM 综合练习通过
- [ ] 后续巩固：用普通 `break` + `boolean isPrime` 重写质数判断
- [ ] 后续巩固：为 ATM 增加输入提示，并在 Day 3 学习后拆分成方法

## 八、Day 3 预告

**Day 3：数组与方法（Array & Method）**

重点学习一维数组、索引与 `length`、`for-each`、方法定义与调用、参数和返回值、`void`、方法重载，以及 Java 与 C/C++ 在数组和参数传递上的区别。学习目标是把较长的 `main()` 拆成职责清晰、可复用的方法。

---

> 学习记录：Day 2 已完成；等 Day 3 继续进行“知识讲解 → 独立做题 → 提交代码 → 反馈修改 → 验收评分”的流程。

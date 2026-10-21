# Java 14 天速成学习笔记 — Day 1：从 C/C++ 过渡到 Java

> 学习主题：开发环境、Java 程序结构、基本数据类型、变量与常量、输入输出  
> 前置基础：C 语言基础；C++ 的 `cin/cout`、`string`、函数、`bool` 等  
> 学习目标：能够独立编写、编译并运行简单的 Java 控制台程序  
> 学习状态：**Day 1 核心知识已学习；练习 1～4 已通过批改；Boss 题自述已掌握，未提交代码验收。**

## 目录

- [一、环境与运行原理](#一环境与运行原理)
- [二、第一个 Java 程序](#二第一个-java-程序)
- [三、基本数据类型与变量](#三基本数据类型与变量)
- [四、控制台输出](#四控制台输出)
- [五、Scanner 键盘输入](#五scanner-键盘输入)
- [六、Java 与 C/C++ 的关键区别](#六java-与-cc-的关键区别)
- [七、常见错误与排查](#七常见错误与排查)
- [八、今日练习与完成情况](#八今日练习与完成情况)
- [九、Day 1 自检与 Day 2 衔接](#九day-1-自检与-day-2-衔接)

---

## 一、环境与运行原理

### 1. JDK、JRE、JVM

| 名称 | 全称 | 职责 |
| --- | --- | --- |
| JVM | Java Virtual Machine | 加载并执行 Java 字节码，解释执行或通过 JIT 编译为本地代码 |
| JRE | Java Runtime Environment | Java 运行所需的环境（传统术语，包含 JVM、类库等） |
| JDK | Java Development Kit | Java 开发工具包，包含运行组件、`javac` 编译器等开发工具 |

**记忆：开发用 JDK，运行依靠 Java 运行环境，字节码由 JVM 执行。** 现代 JDK 已包含运行所需组件，入门一般无需另外安装独立 JRE。

### 2. 编译与运行流程

```text
Java 源文件 Main.java
        │
        ▼  javac Main.java
Java 字节码 Main.class
        │
        ▼  java Main（启动 JVM）
      运行结果
```

对比 C/C++ 的常见情况：C/C++ 一般编译、链接为针对某平台的原生可执行程序；Java 一般先编译为字节码，再由兼容的 JVM 运行。因此 Java 常具有较强的跨平台能力，但仍需相应的运行环境。

### 3. 环境安装与检查

可安装 **Eclipse Temurin JDK 25 LTS**（JDK 21 LTS 也适合本课程）：<https://adoptium.net/temurin/releases/>

打开新终端运行：

```bash
java -version
javac -version
```

若 `java` 可用但 `javac` 不可用，检查是否安装的是完整 JDK，以及 `PATH` 是否包含 JDK 的 `bin` 目录。

---

## 二、第一个 Java 程序

### 1. 源代码

**文件：`Main.java`**

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello Java!");
    }
}
```

```bash
javac Main.java
java Main
```

预期输出：

```text
Hello Java!
```

> `java Main` 使用类名，不加 `.class` 后缀。现代 Java 也支持简单的单文件源程序直接用 `java Main.java` 启动，但先掌握两步编译运行最有助于理解原理。

### 2. 程序结构解析

- `public class Main`：声明公共类 `Main`；公共顶层类的名字与源文件名 `Main.java` 对应。
- `public`：访问权限修饰符，暂时理解为公开。
- `static`：主方法可在不先创建 `Main` 对象的情况下被调用。
- `void`：方法无返回值。
- `main`：本课程采用的经典程序入口方法名。
- `String[] args`：命令行参数形成的字符串数组。
- `{}`：定义类体或方法体；普通语句通常用 `;` 结尾。

`class`、`static`、对象等概念会在 Day 5～8 进一步学习。

### 3. 使用 package 时的编译与运行

我在练习中使用了：

```java
package day1;
```

推荐的目录：

```text
Java14Days/
└── day1/
    └── Main.java
```

在 **`Java14Days` 目录**打开终端：

```bash
javac day1/Main.java
java day1.Main
```

更规范的独立输出目录写法：

```bash
javac -d out day1/Main.java
java -cp out day1.Main
```

**注意**：包名与完整类名很重要。定义了 `package day1;`，通常应运行 `java day1.Main`，而非 `java Main`。

---

## 三、基本数据类型与变量

### 1. Java 的八种基本类型

| 类型 | 位宽 | 典型例子 | 备注 |
| --- | ---: | --- | --- |
| `byte` | 8 | `byte a = 10;` | 整数 |
| `short` | 16 | `short b = 20;` | 整数 |
| `int` | 32 | `int age = 18;` | 最常用整数类型 |
| `long` | 64 | `long n = 10000000000L;` | 大整数后缀常用 `L` |
| `float` | 32 | `float f = 3.14f;` | 浮点字面量使用 `f` 后缀 |
| `double` | 64 | `double h = 1.75;` | 最常用浮点类型 |
| `char` | 16 | `char grade = 'A';` | 单个 UTF-16 代码单元 |
| `boolean` | 规范未规定固定存储位宽 | `boolean ok = true;` | 只能表示 `true` / `false` |

示例：

```java
public class Main {
    public static void main(String[] args) {
        int age = 18;
        double height = 1.75;
        char grade = 'A';
        boolean isStudent = true;
        String name = "张三";

        System.out.println("姓名：" + name);
        System.out.println("年龄：" + age);
        System.out.println("身高：" + height);
        System.out.println("等级：" + grade);
        System.out.println("是否学生：" + isStudent);
    }
}
```

### 2. 基本类型 vs 引用类型

- `int`、`double`、`char`、`boolean` 等是**基本类型**，变量保存对应的值。
- `String`、数组、自定义类等属于**引用类型**，变量保存对对象的引用。
- Java 不提供 C/C++ 式的通用指针运算；对象内存通常由垃圾回收（GC）管理，但文件、网络连接等资源仍需正确关闭。
- `String` 必须大写开头；`char` 用单引号，`String` 用双引号。

### 3. 声明、赋值、重新赋值

```java
int age = 18; // 声明并赋值
age = 19;     // 给现有变量重新赋值
```

Java 局部变量不能在未确定赋值的情况下读取：

```java
int score;
// System.out.println(score); // 编译错误：变量可能尚未初始化
score = 90;
System.out.println(score);
```

### 4. `final` 常量

```java
final String SCHOOL = "Java University";
final int MAX_SCORE = 100;
```

- 被 `final` 修饰的变量完成赋值后不能再赋值。
- 常量命名通常采用大写字母与下划线。
- 对引用变量使用 `final`，限制的是引用重新赋值，不保证对象内部状态不可改变。

---

## 四、控制台输出

| 场景 | C | C++ | Java |
| --- | --- | --- | --- |
| 普通输出 | `printf(...)` | `cout << ...` | `System.out.print(...)` |
| 输出并换行 | `printf("...\n")` | `cout << ... << endl` | `System.out.println(...)` |
| 格式化输出 | `printf("%.2f", x)` | `fixed`、`setprecision` 等 | `System.out.printf("%.2f", x)` |

完整示例：

```java
public class Main {
    public static void main(String[] args) {
        int age = 18;
        double score = 93.456;

        System.out.print("Age: ");
        System.out.println(age);
        System.out.printf("Score: %.2f%n", score);
    }
}
```

输出：

```text
Age: 18
Score: 93.46
```

- `print`：不自动换行。
- `println`：自动换行；`println()` 可以只输出一个空行。
- `printf`：格式化输出；`%.2f` 显示两位小数，`%n` 换行。
- 字符串遇到 `+` 可以拼接其他值，如 `"年龄：" + age`。

---

## 五、Scanner 键盘输入

### 1. 基础步骤

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("请输入姓名：");
        String name = sc.nextLine();

        System.out.print("请输入年龄：");
        int age = sc.nextInt();

        System.out.println();
        System.out.println("你好，" + name + "！");
        System.out.println("你的年龄是：" + age);

        sc.close();
    }
}
```

这与我在练习 3 中通过批改的程序逻辑一致。

### 2. 常见读取方法

| 方法 | 读取内容 |
| --- | --- |
| `sc.nextInt()` | 整数 |
| `sc.nextDouble()` | 小数 |
| `sc.nextBoolean()` | 布尔值 `true` / `false` |
| `sc.next()` | 下一个不含空白分隔符的词 |
| `sc.nextLine()` | 一整行文本（可以包含空格） |

### 3. `nextInt()` 与 `nextLine()` 的换行问题

错误风险示例：

```java
int age = sc.nextInt();
String name = sc.nextLine(); // 可能读到当前行剩余的空字符串
```

原因：`nextInt()` 读走整数后，通常把本行末尾的换行符留在输入流中。

若先读整数，再读下一行完整姓名，可以先消费当前行剩余内容：

```java
int age = sc.nextInt();
sc.nextLine();  // 消费当前行剩余内容和换行
String name = sc.nextLine();
```

**注意**：关闭 `Scanner(System.in)` 也会关闭标准输入流，程序还要继续读取标准输入时不要提前关闭。

### 4. 如何选学号类型

学号、邮编等即使全部由数字组成，也可能有前导零，通常应使用字符串类型：

```java
String studentId = sc.nextLine(); // 输入 00123，则保留 "00123"
```

---

## 六、Java 与 C/C++ 的关键区别

| 对比点 | C/C++ | Java |
| --- | --- | --- |
| 程序组织 | 可以直接定义全局函数 | 常规方法定义在类中 |
| 入口 | 常见 `int main()` | 本课程使用 `public static void main(String[] args)` |
| 编译结果 | 常见原生可执行文件 | 通常是 `.class` 字节码 |
| 运行环境 | 直接运行本地可执行程序 | 由 JVM 启动并运行 |
| 输入输出 | `scanf` / `printf`、`cin` / `cout` | `Scanner`、`System.out` |
| 布尔类型 | C++ 的 `bool` | `boolean` |
| 字符串 | C 字符数组、C++ `string` | `String` 引用类型 |
| 整数位宽 | 依实现而定 | `int` 固定 32 位 |
| 字符 | C 的 `char` 是 1 字节 | Java `char` 是 16 位 UTF-16 代码单元 |
| 内存管理 | C 手动管理；C++ 也有 RAII/智能指针 | 对象内存通常由 GC 自动回收 |

---

## 七、常见错误与排查

1. **文件名、类名不一致**：`public class Main` 应对应 `Main.java`。
2. **大小写错误**：`String`、`System`、`Scanner`、`main` 各有固定大小写；Java 基本布尔类型是 `boolean`，不是 `bool`。
3. **漏写分号**：语句后经常需要 `;`。
4. **类型不匹配**：`int age = 18.5;` 不能直接编译。
5. **引号用错**：`char grade = 'A';` 与 `String text = "A";` 不同。
6. **未导入 Scanner**：要在类定义之前写 `import java.util.Scanner;`。
7. **误读 `nextLine()`**：使用 `nextInt()` 后读取整行，注意残留换行符。
8. **不正确的类名启动方式**：有 `package day1;` 就要正确使用 `day1.Main` 作为完整类名。
9. **资源关闭遗漏**：练习 3、4 初稿都漏掉了 `sc.close()`，随后在二次提交中修复。注意别过早关闭标准输入。
10. **代码风格不统一**：建议运算符两侧留空格，如 `int age = 18;`；布尔变量可命名为 `isStudent`，常量可命名为 `SCHOOL`。

---

## 八、今日练习与完成情况

| 练习 | 任务 | 当前情况 | 批改结果 |
| --- | --- | --- | --- |
| 练习 1 | Hello Java（`print` / `println`） | ✅ 已提交并通过 | **10/10** |
| 练习 2 | 个人名片（基本类型、`String`、`final`） | ✅ 已提交并通过 | **10/10** |
| 练习 3 | 年龄信息录入（`Scanner`） | ✅ 二次提交后通过 | **9/10 → 10/10** |
| 练习 4 | 成绩登记器（字符串学号、`printf`） | ✅ 二次提交后通过 | **9/10 → 10/10** |
| 练习 5 | Boss：学生档案管理器 | 🟡 自述能够独立完成，未提交源代码 | **未正式验收** |

> **说明**：目前有代码记录并通过批改的题目为 4/5。Boss 题未提交，不能把它记录为已完成批改；如果后续想严格执行 Day 1 的全部过关标准，可补交代码验证。

### 练习题目回顾（不附未完成题的参考答案）

**练习 1 — Hello Java**

输出固定的四行文本，至少使用 `print()` 和 `println()` 两种方法。

**练习 2 — 个人名片**

声明姓名、年龄、身高、等级、学生身份以及学校名称常量，使用变量输出名片。

**练习 3 — 年龄信息录入**

使用 `Scanner` 输入完整姓名（允许空格）与整数年龄，输出问候信息，并关闭扫描器。

**练习 4 — 成绩登记器**

输入姓名、学号和浮点成绩；学号保留前导零；用 `final` 定义课程名称；成绩显示两位小数。

**练习 5 — Boss：学生档案管理器**

输入姓名、学号、年龄、身高、是否住宿；学校名称使用 `final`；身高保留两位小数；输出完整档案并关闭扫描器。测试：姓名含空格、学号带前导零、身高 `1.5` 显示为 `1.50`、住宿 `false`。

### 从练习中得到的经验

- **先考虑数据的含义，再选择类型**：学号使用 `String` 而不是 `int`。
- **让输入顺序更容易处理**：完整姓名先用 `nextLine()` 读取，之后再读取数字。
- **格式化展示不等于修改数值**：`printf("%.2f", value)` 只控制输出样式。
- **养成收尾习惯**：写完 Scanner 相关程序，检查是否应当调用 `close()`。
- **注意规范**：`Scanner sc = ...`、`final String COURSE = ...` 比挤在一起更易读。

---

## 九、Day 1 自检与 Day 2 衔接

### 1. 自检清单

- [x] 理解 JDK、JRE、JVM 的基本关系。
- [x] 能辨认 `public class Main` 和 `main` 方法。
- [x] 能使用变量、基本类型、`String` 与 `final`。
- [x] 能通过 `System.out.print/println/printf` 输出。
- [x] 能通过 `Scanner` 读取姓名、整数、小数。
- [x] 练习 1～4 已通过批改。
- [ ] Boss 题源代码验收（目前由本人自评已掌握，选择跳过提交）。
- [ ] Day 1 基础知识五题小测达到 4/5（未见正式答题记录）。

### 2. 进入 Day 2 的建议

已具备学习 Day 2 所需的基础。若将来希望严格补齐 Day 1 的全部验收，再提交 Boss 题即可；现在可以按计划进入下一天，不必重复已掌握的练习。

**Day 2 主题：运算符、条件判断与循环**。重点关注：

- 算术、比较、逻辑运算符；
- `if` / `else`、`switch`；
- `for`、`while`、`do-while`；
- `break`、`continue`；
- 利用 Day 1 的 `Scanner`，编写能根据用户输入进行判断或重复处理的程序。

> 学习方法：坚持“看示例 → 自己手写 → 编译运行 → 独立做题 → 提交批改”，不要提前查看练习答案。

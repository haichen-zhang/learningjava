# Day 3 — 数组与方法（Arrays & Methods）

> Java 14 天速成计划 | Day 3 / 14  
> 学习成果：五道练习全部通过，48.5 / 50（97%）

## 1. 一维数组

数组是长度固定、元素类型统一的对象。数组变量保存的是引用，而不是整个数组对象。

```java
int[] a;                       // 声明引用，尚未创建数组
int[] b = new int[5];          // 创建数组
int[] c = {10, 20, 30};        // 声明时初始化
int[] d = new int[]{1, 2, 3};  // 显式初始化
```

- `a.length` 是数组长度，不是方法；String 使用 `length()`。
- 合法下标为 `0` 到 `length - 1`。
- 长度为 0 的数组合法，但不能访问任何元素。
- 负长度触发 `NegativeArraySizeException`；非法下标触发 `ArrayIndexOutOfBoundsException`。
- `new` 创建的数组元素自动初始化；局部变量本身必须先赋值才能读取。

| 元素类型 | 默认值 |
|---|---|
| 整数类型 | 0 |
| 浮点类型 | 0.0 |
| boolean | false |
| char | U+0000 |
| 引用类型（如 String） | null |

### 与 C/C++ 对比

| 项目 | Java | C/C++ |
|---|---|---|
| 长度 | 创建后固定，可用 `.length` 获取 | 原生数组长度固定，退化为指针后不携带长度 |
| 越界 | 运行时检查并抛出异常 | 原生数组越界访问通常属于未定义行为 |
| 初始化 | new 数组元素有默认值 | 未初始化的自动存储期原生整数数组元素不保证为零 |
| 内存管理 | 垃圾回收 | 视存储期而定；动态分配需管理，C++ 可使用 RAII |
| 动态容器 | ArrayList（后续学习） | C++ vector |

## 2. 数组遍历

```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
for (int value : arr) {
    System.out.println(value);
}
```

普通 for 适合需要下标、修改元素或逆向遍历的场景；增强 for 适合顺序读取。对 `int[]` 而言，给增强 for 的 `value` 重新赋值不会改变数组元素。

## 3. 方法（Method）

```java
static int add(int a, int b) {
    return a + b;
}
```

- `static`：方法属于类，可在同类的静态 main 中直接调用。
- `int`：返回值类型；`void` 表示不返回值。
- `a, b`：形参；调用 `add(2, 3)` 时，2、3 是实参。
- `return` 结束当前方法，并在需要时返回结果。
- 方法重载：同名方法具有不同的参数类型、数量或顺序；仅改变返回类型不能构成重载。
- Java 方法定义在类中，不能像普通语句一样定义在另一个方法内部。C/C++ 可以定义独立函数；C++ 支持函数重载，C 不支持普通函数重载。

## 4. 值传递与数组引用

**Java 始终是值传递。** 基本类型复制数值，引用类型复制引用值。

```java
static void change(int[] arr) {
    arr[0] = 99;
}
static void reassign(int[] arr) {
    arr = new int[]{100};
}
```

若调用者有 `int[] a = {1, 2, 3};`，调用 `change(a)` 后 a 为 `[99, 2, 3]`；再调用 `reassign(a)`，a 仍为 `[99, 2, 3]`。

```text
调用者 a ──────┐
               ├──> 同一个数组对象 [99, 2, 3]
形参 arr ──────┘
```

`int[] b = a;` 只复制引用；`new int[a.length]` 再复制元素才会得到独立的 int 数组。`a == b` 判断引用是否指向同一对象，`Arrays.equals(a, b)` 比较一维数组内容。

C/C++ 的指针按值传递与 Java 引用值复制有相似之处，但 Java 不支持指针算术；C++ 还支持真正的引用参数，例如 `int&`。

## 5. 核心算法参考

以下方法片段可放入同一个 Java 类中；使用 Arrays 的片段需要 `import java.util.Arrays;`。

### 5.1 求和与平均值

```java
static void printStatistics(int[] arr) {
    if (arr.length == 0) {
        System.out.println("空数组，无平均值");
        return;
    }
    long sum = 0;
    for (int value : arr) sum += value;
    System.out.println("总和：" + sum);
    System.out.printf("平均值：%.2f%n", (double) sum / arr.length);
}
```

先提升累加器类型，避免 int 溢出；先转换为 double 再除法，避免整数截断。时间 O(n)，额外空间 O(1)。

### 5.2 最大最小值及首次下标

```java
static void printMinMax(int[] arr) {
    if (arr.length == 0) return;
    int maxIndex = 0, minIndex = 0;
    for (int i = 1; i < arr.length; i++) {
        if (arr[i] > arr[maxIndex]) maxIndex = i;
        if (arr[i] < arr[minIndex]) minIndex = i;
    }
    System.out.println("最大值：" + arr[maxIndex] + "，索引：" + maxIndex);
    System.out.println("最小值：" + arr[minIndex] + "，索引：" + minIndex);
}
```

严格比较保留首次下标；不能把最大值初始化为 0，否则全负数组可能出错。时间 O(n)，额外空间 O(1)。

### 5.3 查找全部匹配位置

```java
static void search(int[] arr, int target) {
    int count = 0;
    for (int value : arr) if (value == target) count++;
    System.out.println("匹配次数：" + count);
    if (count == 0) {
        System.out.println("未找到目标值");
        return;
    }
    System.out.print("匹配下标：");
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) System.out.print(i + " ");
    }
    System.out.println();
}
```

两次线性遍历仍然是 O(n)，额外空间 O(1)。

### 5.4 逆序副本与统一输出

```java
static int[] reversedCopy(int[] source) {
    int[] result = new int[source.length];
    for (int i = 0; i < source.length; i++) {
        result[i] = source[source.length - 1 - i];
    }
    return result;
}
static void printArray(String title, int[] arr) {
    System.out.print(title);
    for (int value : arr) System.out.print(value + " ");
    System.out.println();
}
```

逆序映射：`i -> n - 1 - i`。时间 O(n)，额外空间 O(n)。空数组也返回新对象。

### 5.5 双指针原地逆序

```java
static void reverse(int[] arr) {
    int left = 0, right = arr.length - 1;
    while (left < right) {
        int temp = arr[left];
        arr[left++] = arr[right];
        arr[right--] = temp;
    }
}
```

时间 O(n)，额外空间 O(1)。连续逆序两次恢复原顺序。

### 5.6 稳定去重：本人练习采用的删除法

```java
static int[] unique(int[] data) {
    int size = data.length;
    int[] temp = new int[size];
    for (int i = 0; i < size; i++) temp[i] = data[i];
    for (int i = 0; i < size; i++) {
        for (int j = 0; j < i; j++) {
            if (temp[i] == temp[j]) {
                for (int k = i; k < size - 1; k++) {
                    temp[k] = temp[k + 1];
                }
                size--;
                i--;
                break;
            }
        }
    }
    int[] result = new int[size];
    for (int i = 0; i < size; i++) result[i] = temp[i];
    return result;
}
```

物理长度 `temp.length` 不变，逻辑长度 `size` 随删除减少。删除后 `i--` 抵消外层 `i++`，避免漏检移入当前位置的元素。最坏时间 O(n²)：每个原始元素至多被处理一次，查重和移动各至多 O(n)；额外空间 O(n)。

### 5.7 稳定去重：筛选法（推荐复习）

```java
static int[] uniqueByFilter(int[] data) {
    int[] temp = new int[data.length];
    int count = 0;
    for (int value : data) {
        boolean exists = false;
        for (int j = 0; j < count; j++) {
            if (temp[j] == value) {
                exists = true;
                break;
            }
        }
        if (!exists) temp[count++] = value;
    }
    return Arrays.copyOf(temp, count);
}
```

只收集首次出现的元素，无须左移。最坏时间 O(n²)，额外空间 O(n)。练习 4 允许 Arrays.copyOf 辅助裁剪，但不允许现成去重工具。

### 5.8 替换负数并返回数量

```java
static int replaceNegatives(int[] arr) {
    int count = 0;
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] < 0) {
            arr[i] = 0;
            count++;
        }
    }
    return count;
}
```

调用时接收返回值：`int count = replaceNegatives(a);`。数组修改与返回值是两件独立的事。时间 O(n)，额外空间 O(1)。

## 6. Boss：ArrayLab 综合管理系统

设计思路：初始化一次数组，使用 do-while 与 switch 循环调度六个独立功能。所有操作共享同一数组对象，逆序与替换负数直接修改该对象。退出时通过带标签的 break 跳出菜单循环。

| 菜单 | 方法 | 是否修改数组 |
|---|---|---|
| 1 查看 | printArray | 否 |
| 2 统计 | printStatistics | 否 |
| 3 极值 | printMinMax | 否 |
| 4 查找 | search | 否 |
| 5 逆序 | reverse | 是 |
| 6 替换负数 | replaceNegatives | 是 |
| 0 退出 | break menu | 否 |

### 调度参考代码

以下是与第 5 节方法配合使用的 main 方法（另需导入 Scanner）；输出方法使用第 5.4 节的签名，并在菜单 1 中单独打印下标。

```java
public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    int n;
    do {
        System.out.println("请输入数组长度（1~20）：");
        n = sc.nextInt();
        if (n < 1 || n > 20) System.out.println("长度错误");
    } while (n < 1 || n > 20);
    int[] a = new int[n];
    System.out.println("请输入" + n + "个整数：");
    for (int i = 0; i < n; i++) a[i] = sc.nextInt();
    menu: do {
        System.out.println("1.查看 2.统计 3.极值 4.查找 5.逆序 6.替换负数 0.退出");
        switch (sc.nextInt()) {
            case 1:
                for (int i = 0; i < a.length; i++)
                    System.out.println("a[" + i + "] = " + a[i]);
                break;
            case 2: printStatistics(a); break;
            case 3: printMinMax(a); break;
            case 4:
                System.out.println("请输入目标值：");
                search(a, sc.nextInt());
                break;
            case 5:
                reverse(a);
                printArray("逆序结果：", a);
                break;
            case 6:
                System.out.println("替换数量：" + replaceNegatives(a));
                break;
            case 0:
                System.out.println("系统已退出");
                break menu;
            default: System.out.println("无效选项");
        }
    } while (true);
    sc.close();
}
```

此处为复习用简化调度示例；本人最终提交将菜单与查看功能也分别封装，结构更符合 Boss 要求。

## 7. 本人典型错误复盘

| 错误 | 原因 | 正确处理 |
|---|---|---|
| `n < 1 && n > 30` | 两个条件不可能同时成立 | `n < 1 || n > 30` |
| 输出 target 作为匹配次数 | 混淆目标与计数变量 | 输出 count |
| 输入提示固定为 7 个整数 | 未使用实际长度 | 拼接 n |
| `source[n-i]` | 首次访问下标 n 越界 | `source[n-1-i]` |
| 忘记长度校验 | 只关注正常输入 | 创建数组前验证范围 |
| 所有逻辑放在 main | 忽略方法封装要求 | 计算、输出、调度分离 |
| 左移循环使用 `b[i]=b[i+1]` | 混淆外层与移动下标 | `b[k]=b[k+1]` |
| 删除后直接 i++ | 跳过移入当前位置的元素 | 删除后 i-- 再 break |
| int sum 累加大整数 | 32 位整数溢出 | long sum |
| 忽略 replaceNegatives 返回值 | 修改数组不等于输出数量 | 接收并输出返回值 |
| 查找失败、退出缺少提示 | 未逐项核对需求 | 完善对应分支 |
| printf 未换行 | 下一轮菜单与结果粘连 | 使用 `%n` |

Java int 溢出会按 32 位补码回绕；C/C++ 有符号整数溢出通常属于未定义行为。两个 2,000,000,000 相加的正确结果是 4,000,000,000，int 累加则得到 -294,967,296。事后转换 double 无法修复已发生的溢出。

## 8. 边界测试清单

- [ ] 长度下限、上限及非法长度重试
- [ ] 空数组（题目允许时）与单元素数组
- [ ] 全负数、全零、全相同元素
- [ ] 最大最小值重复，保留首次下标
- [ ] 查找无匹配、多匹配、全部匹配
- [ ] 连续重复元素去重
- [ ] 副本独立且原数组不变
- [ ] 连续逆序两次恢复原顺序
- [ ] 连续替换负数，第二次返回 0
- [ ] 大整数累加、平均值小数格式
- [ ] 非法菜单选项、正常退出、状态持续保留

## 9. 最终成绩

| 练习 | 历次得分 | 最终得分 |
|---|---|---:|
| 学生成绩统计器 | 9.5 | 9.5/10 |
| 目标值定位器 | 6 → 9.5 | 9.5/10 |
| 逆序副本生成器 | 3.5 → 8.5 → 10 | 10/10 |
| 稳定去重处理器 | 7 → 9.5 | 9.5/10 |
| ArrayLab Boss | 8 → 10 | 10/10 |
| 合计 | 全部通过 | 48.5/50 |

评分为此前教学验收记录，主要依据静态审查与逻辑推演；本笔记中的参考代码未实际编译运行。

## 10. 复习重点

1. 数组索引先推导，再编写循环。
2. 牢记 Java 值传递，区分修改对象与重新赋值引用。
3. 区分原地算法和返回独立副本的算法。
4. 使用逻辑长度管理有效数据；删除元素后检查循环下标。
5. main 负责调度，方法负责独立功能；不要忽略返回值。
6. 先检查边界和数据类型，再逐项核对题目要求。
7. 变量名表达含义，例如 maxIndex、count、result；保持统一缩进与空格。

**Day 3 已完成，整体进度 3/14（21.4%）。**

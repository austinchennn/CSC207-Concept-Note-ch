## 内存
a = new String("a")和直接 a =  “a”
在内存里有什么区别

`String a = "a"`会在字符串常量池（String Constant Pool）优化与复用
`String a = new String("a")` 每次都会强行在堆内存中开辟新空间, 但是堆里这个新对象的内部，再指向常量池中 `"abc"` 的实际字符数组（如果有的话）。而你的变量 `s3` 最终指向的是**堆内存里的这个新对象**。

> [!example]
> 
> ```
String s1 = "abc";
String s2 = "abc";
System.out.println(s1 == s2); //  输出 true！说明它们在内存中是同一个对象
> ```
> V.S
> ```
String s3 = new String("abc");
String s4 = new String("abc");
System.out.println(s3 == s4); // ❌ 输出 false！它们在堆中的地址完全不同
> ```
> 
#### 已经定义了
```
int a = 42;   
int b = 13;   
int b = a; //错误 java: 已在方法 main(java.lang.String[])中定义了变量 b
```
---
##  Primitive types V.S  Class types
重点 <u>All primitive types are immutable.  
There are mutable class types and immutable class types.</u>

---
# Java 字符串 (String & StringBuilder) 

## 1. 核心概念对比

|**特性**|**String**|**StringBuilder**|
|---|---|---|
|**可变性**|❌ **不可变 (Immutable)**|✅ **可变 (Mutable)**|
|**数据类型**|引用类型 (Reference Type)|引用类型 (Reference Type)|
|**初始化**|`String s = "abc";` (支持简写)|`StringBuilder sb = new StringBuilder("abc");`|
|**修改性能**|慢 (每次修改都会创建新对象)|快 (直接在原对象上修改)|

---

## 2. String 常用操作 (附 Python 对比)

因为 `String` 不可变，所有修改操作都会**返回一个新字符串**。

- **创建：** `String s = "Hello";` _(双引号)_
    
- **拼接：** `String s3 = s1 + s2;` 或 `s1.concat(s2);` _(实战中强烈推荐直接用 `+`)_
    
- **常用方法（高频精选）：**
    
    - **⚠️ 内容比较：** `s1.equals(s2);` _(等同于 Python 的 `s1 == s2`。在 Java 中**绝对不要**用 `==` 比较字符串内容！)_
        
    - **获取长度：** `s.length();` _(等同于 Python 的 `len(s)`，注意 Java 里带括号)_
        
    - **索引截取：**
        
        - `s.charAt(2);` _(等同于 Python 的 `s[2]`)_
            
        - `s.substring(2, 4);` _(等同于 Python 的 `s[2:4]`)_
            
    - **检查前后缀：**
        
        - `s.startsWith("He");` _(等同于 Python 的 `s.startswith("He")`)_
            
        - `s.endsWith("lo");` _(等同于 Python 的 `s.endswith("lo")`)_
            
    - **查找与替换：**
        
        - `s.indexOf("l");` _(等同于 Python 的 `s.find("l")`，找不到返回 `-1`)_
            
        - `s.replace("e", "a");` _(等同于 Python 的 `s.replace("e", "a")`)_
            
    - **格式转换：**
        
        - `s.trim();` _(去除首尾空白，等同于 Python 的 `s.strip()`)_
            
        - `s.toLowerCase();` / `s.toUpperCase();` _(等同于 Python 的 `s.lower()` / `s.upper()`)_
            
    - **分割：** `s.split(",");` _(按逗号分割，返回一个字符串数组 `String[]`，等同于 Python 的 `s.split(",")`)_
        
---
## 3. StringBuilder 核心方法

当需要频繁修改字符串（比如在循环中反复拼接）时使用，直接在原对象上操作。

```
StringBuilder sb = new StringBuilder("ban"); 
// ⚠️ 注意：不能写成 StringBuilder sb = "ban"; 会报错

sb.append("phone");      // 追加文本 (在末尾)
sb.insert(3, "ana");     // 插入文本 (在指定索引处)
sb.setCharAt(3, 'o');    // 替换单个字符 (注意第二个参数必须是 char)
sb.reverse();            // 反转整个字符串
```
---
## 4. 补充知识：char 类型

- **概念：** Java 中的基本数据类型 (Primitive type)，仅用于存储**单个字符**。
    
- **语法：** 必须使用**单引号** `''`。
    
- **示例：** `char c = 'x';` _(对比 String 的 `"x"`)_
## 5. `""`和`''`
### 1. 双引号 `""` —— 代表 `String`（字符串）

- **数据类型：** 属于**引用数据类型**（它是一个对象类）。
    
- **内容长度：** 可以包含 **0 个**、**1 个**或**多个**字符。

- **示例代码：**
```
    String s1 = "Hello";  // 多个字符
    String s2 = "A";      // 哪怕只有一个字符，只要用双引号包着，它也是 String
    String s3 = "";       // 空字符串（包含 0 个字符），完全合法
```
### 2. 单引号 `''` —— 代表 `char`（字符）

- **数据类型：** 属于**基本数据类型**（Primitive type）。
    
- **内容长度：** **必须且只能包含 1 个字符**（绝对不能是 0 个，也不能是 2 个及以上）。
    
- **示例代码：**
    

Java

```
    char c1 = 'A';        // 正确：单个字符
    char c2 = '中';       // 正确：Java 支持 Unicode，所以一个中文字符也是合法的 char
    char c3 = '\n';       // 正确：转义字符（如换行符），在底层也算作一个字符
```
---
### ⚠️ 常见的易错点
1.  **单引号里不能为空：**
```
char c = ''; // ❌ 编译报错！单引号里必须填入确切的一个字符，哪怕是一个空格 ' ' 也行，但不能什么都不写。
```
2.  **单引号里不能有多个字符：**
```
char c = 'AB'; // ❌ 编译报错！有多个字符必须用双引号 String。
```
3.  **类型不能混用（即使看起来内容一样）：**
```
    char c = "A"; // ❌ 编译报错！"A" 是 String 对象，不能赋值给基本类型 char。
    String s = 'A'; // ❌ 编译报错！'A' 是 char，不能直接赋值给 String。
```
### 核心区别对比

| **特性**      | **"A" (单字符的 String)**                         | **'A' (单字符的 char)**                      |
| ----------- | --------------------------------------------- | ---------------------------------------- |
| **底层本质**    | **对象 (Object)**                               | **基本数据类型 (Primitive)**                   |
| **内存占用**    | 很大（包含对象头、字符数组等几十个字节）                          | 极小（固定只占 2 个字节）                           |
| **能否调用方法？** | **能。** 可以使用 `"A".toLowerCase()` 等所有 String 方法 | **不能。** 基本类型没有方法，`'A'.toLowerCase()` 会报错 |
| **相等比较**    | 必须用 `equals()`，例如 `"A".equals(s)`             |                                          |

`String`（字符串）和 `char`（字符）可以直接进行加法
###### 加法 (`+`)：变成“拼接”

当 `String` 和任何其他类型（包括 `char`）使用 `+` 号相连时，`+` 号就不再是数学运算了，而是变成了**字符串拼接符**。

- **运行逻辑：** Java 会自动把 `char` 转换成文本，然后拼接到 `String` 的后面或前面，最后返回一个**全新的 String**。
    
- **代码示例：**
```
    String s = "Hello";
    char c = '!';
    
    String result1 = s + c; // 结果是 "Hello!"
    String result2 = c + s; // 结果是 "!Hello"
```
---
## Class

1. 只要没有发生“名字冲突”，`this.` 就可以完全省略
---
## Array
【重点】
1. JAVA不支持负Index和Slicing
2. 数组的大小永远不能改变。
3. 边界`int[n]`的index只有0到n-1（n取不到）
---
### 1. 创建与初始化 (Initialization)

Java 是强类型语言，创建数组时必须声明**类型**和**长度**（或初始元素）。

|**操作**|**Java []**|**Python list**|
|---|---|---|
|**声明空数组 (定长)**|`int[] arr = new int[5];` (默认全为0)|`arr = [0] * 5`|
|**直接赋值初始化**|`int[] arr = {1, 2, 3};`|`arr = [1, 2, 3]`|
|**不同类型混合**|❌ 绝对不行，`int[]` 只能放整数|✅ 可以，`arr = [1, "A", True]`|

---
### 2. 基本属性与访问 (Basic Access)

Java 数组本身**没有任何方法**（不能排序、不能查找、不能直接打印内容），它只有一个唯一的属性：`length`。

|**操作**|**Java []**|**Python list**|
|---|---|---|
|**获取长度**|`arr.length` _(注意：没有括号，它是属性不是方法)_|`len(arr)`|
|**读取元素**|`arr[0]`|`arr[0]`|
|**修改元素**|`arr[0] = 99;`|`arr[0] = 99`|
|**读取最后一个元素**|`arr[arr.length - 1]`|`arr[-1]` _(Java 绝对不支持负数索引！)_|
【重点】
1. Java 的原生数组只有`.length`绝对没有 `size`或 `size()`
2. **集合 (Collections)** `ArrayList`, `HashMap`有 `size()`,没有`.length
3. String只有**`.length()`**
---
### 3. 遍历数组 (Iteration)

因为原生数组缺乏内置方法，遍历成了操作原生数组最核心的手段。Java 提供了两种 `for` 循环方式：
**方式一：基础 for 循环 (需要用到索引时使用，等同于 Python 的 `for i in range(len(arr))`)**
```
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

**方式二：增强 for 循环 (等同于 Python 的 `for item in arr`)**
```
// 读作：for each num in arr
for (int num : arr) { 
    System.out.println(num);
}
```

---

帮你把这段笔记重新整理并修复了排版格式，去掉了多余的符号，看起来更清晰直观：

## 混合类型存储 (`Object[]`)

Java 数组默认只能存同一种类型，但可以通过继承机制绕过此限制。

- **原理**：Java 中所有类都是 `Object` 的子类。
- **实现**：声明一个 `Object[]`，即可塞入任何对象。
```
Object[] arr = new Object[5];
arr[0] = new String("Text"); // 存入字符串
arr[1] = new Integer(1872);  // ⚠️ 基本类型必须使用“包装类”(Wrapper class)
arr[1] = 1872; // 💡 自动装箱 (Autoboxing)：直接写基本类型即可
arr[2] = new int[50];        // 存入另一个数组 (数组也是对象)
```

### 取值与强制类型转换 (Casting)

从 `Object[]` 取出元素时，Java 只知道它是 `Object`。若要使用原类型的特有方法，**必须强转**。
- **正确强转**：
```
String s = (String) arr[0]; // 明确告诉 Java 这是一个 String
```

- **常见报错 (`ClassCastException`)**：
    如果强转类型错误（比如把存进去的 `Integer` 强转成 `String`），编译不报错，但**运行时会直接崩溃**。
---
## 3. 多维数组 (2D Arrays)

- **本质**：数组的数组（Array of arrays）。
    
- **常规声明**（创建规则的矩形）：

```
int[][] table = new int[50][3]; // 50行，每行3列
table[49][2] = 123;             // 合法赋值 (最大索引: [49][2])
// table[2][49] = 123;          // ❌ 报错：越界
```
---

## 4. 不规则数组 (Irregular/Jagged Arrays)

得益于“数组的数组”这一本质，二维数组的每一行（内层数组）长度可以完全不同。

- **创建步骤**：
    1. 先只定义外层大小（行数）。    
    2. 分别为每一行分配不同长度的内层数组。
```
int[][] irregular = new int[3][]; // 仅指定有 3 行

irregular[0] = new int[6];        // 第 0 行长度为 6
irregular[1] = new int[99];       // 第 1 行长度为 99
irregular[2] = new int[10];       // 第 2 行长度为 10
irregular[2] = new String[10]; //报错
irregular[2] = new int[11]; //重新附值，不报错
irregular[1][8] = 170;            // 正常赋值
```
`irregular[0]`外层的type是`int[]` （一维整型数组）

## 5. 数组默认值。

**基本数据类型默认是“零”或“假”，所有对象引用类型默认全是 `null`。**
##### 1. 常见基本类型数组 (Primitive Arrays)

|**数组类型**|**默认值**|**说明**|
|---|---|---|
|`int[]`, `byte[]`, `short[]`, `long[]`|**`0`**|所有整数类型都是 0|
|`double[]`, `float[]`|**`0.0`**|所有浮点（小数）类型都是 0.0|
|`boolean[]`|**`false`**|布尔值默认是假|
|`char[]`|**`\u0000`**|也就是空字符（不可见字符，打印出来像一个空格）|

##### 2. 引用类型数组 (Reference Arrays)

**只要不是上面的那 8 种基本类型，其他所有的数组默认值统统都是 `null`！**

|**数组类型**|**默认值**|**说明**|
|---|---|---|
|`String[]`|**`null`**|字符串本质上也是对象|
|`Object[]`|**`null`**|万物皆对象，没有任何引用指向时就是 null|
|`Scanner[]`, `Monster[]`|**`null`**|任何你自己写的类或者 Java 自带的类|
|`int[][]` (多维数组)|**`null`**|二维数组的本质是“装数组的数组”，所以外层装的也是引用（地址），默认还是 null|


# Aliases

1. 有引用类型才有Alias

2. 原始类型没有Alias, variable直接装value

3. 改一个另一个跟着改这件事只在mutable的object身上才会发生

	- Example: String就不会
```
		String name = new String("Justin Trudeau");
		String primeMinister = name;
		primeMinister = primeMinister.replace('u', 'U');
		System.out.println(name);System.out.println(primeMinister);
```

The output from this code is:
```
Justin TrudeauJUstin TrUdeaU
```
因为`primeMinister.replace('u', 'U');`返回了一个新的String


## Copy
#### Deep Copy vs Shallow Copy

##### 1. 浅拷贝 (Shallow Copy)

- **核心论点：** 只复制最外层的**引用（内存地址）**，不复制底层真实的内部对象。新旧数组依然“共享”同一批底层对象。
    
- **对于 Immutable（不可变类型，如 `String`）：** 安全。因为底层对象改不了，不会发生连带影响。
    
- **对于 Mutable（可变类型，如二维数组、自定义对象）：** 极度危险！通过一个变量修改了底层数据，另一个变量也会跟着变。
    

**代码验证（二维数组的浅拷贝灾难）：**

Java

```
int[][] table = {
    {11, 22},
    {5, 10}
};
int[][] copy = new int[2][2];

// 浅拷贝过程：仅仅把 table 里存的内存地址交给了 copy
for (int i = 0; i < table.length; i++) {
    copy[i] = table[i]; 
}

// 🟢 场景 A：修改最外层的引用（换绑新遥控器）
copy[0] = new int[]{0, 0}; 
// 结果：互不影响。copy[0] 指向新数组，table 没变。

// 🔴 场景 B：修改底层的可变数据（操作共享的电视机）
copy[1][1] = -99999; 
// 结果：一改全改！table[1][1] 也变成了 -99999。
// 原因：table[1] 和 copy[1] 死死捏着同一个内层数组 {5, 10} 的钥匙。
```

---

##### 2. 深拷贝 (Deep Copy)

- **核心论点：** 为了避免浅拷贝的“一改全改”灾难，必须在每一层级都 **`new` 出全新的对象**，将值逐个复制过去。
    
- **结果：** 内存中存在两套完全独立的结构，怎么修改都互不干扰。

# 方法传参的“副作用” (Side Effects)

**Java 传参唯一铁律：永远是“值传递” (Pass by Value)！** 区别在于传的“值”究竟是什么。

##### 1. 传基本类型 (Primitive Types)

- **如：** `int`, `double`, `boolean` 等。
    
- **原理：** 传的是数据的“复印件”。
    
- **副作用：** **无。** 在方法里怎么修改，都绝对不会影响外面的原变量。
    

##### 2. 传引用类型 (Reference Types)

- **如：** `数组`, `ArrayList`, `自定义对象` 等。
    
- **原理：** 传的是内存地址的副本（相当于给方法“配了一把新钥匙”）。
    
- **副作用：** **有！** 方法拿着新钥匙开门，修改了屋里（内存中）的真实数据，外面拿着老钥匙去看，数据也跟着变了。

---

> [!example]
> ```
// 1. 基本类型传参 (安全)
int num = 10;
change(num); 
// 结果：num 依然是 10
// 2. 引用类型传参 (危险)
int[] arr = {10, 20};
change(arr); 
// 结果：arr[0] 会变成 999！
// ====== 方法定义 ======
public static void change(int x) { x = 999; }
public static void change(int[] a) { a[0] = 999; }
> ```
> 如果你必须把数组传给方法进行处理，但又**不想破坏原数组**，必须在方法内部（或传入前）做
> 个**深拷贝 (Deep Copy)**，在新数组上进行操作。


重点 <u>All primitive types are immutable.  There are mutable class types and immutable class types.</u>

### String 的拷贝机制与内存本质

**核心结论：** 因为 `String` 是**不可变 (Immutable)** 的，所以在绝大多数情况下，你**不需要**对它做深拷贝，直接用等号赋值（浅拷贝）最安全、最省内存。

##### 1. 方式一：直接赋值 (浅拷贝)

```
String s1 = "Hello";
String s2 = s1; 
```

- **内存本质：** `s1` 和 `s2` 这两个遥控器指向**同一个**字符串对象。
    
- **为什么安全：** 因为 `String` 砸不烂。如果你后续写 `s2 = "World";`，`s2` 会指向新对象，`s1` 依然是 `"Hello"`，互不影响。
    

##### 2. 方式二：使用 `new String()` (深拷贝)
```
String s1 = new String("Hello");// or String s1 = "Hello"
String s2 = new String(s1); // ✅ 完全合法！
```

- **内存本质：** 这会在堆内存中硬生生劈开两块**完全独立**的空间。`s1` 和 `s2` 的屏幕上都写着 `"Hello"`，但它们是两台不同的电视机（内存地址不同）。
    
- **考试/面试必考细节：** 如果用 `==` 比较它们的地址：`s1 == s2` 结果是 **`false`**。
    
    如果用 `.equals()` 比较它们的值：`s1.equals(s2)` 结果是 **`true`**。
    
- **工作建议：** 除非为了应付特殊的底层算法，否则平时**极度不推荐**这么写，因为连续 `new` 会在堆里造出大量重复的字符串，白白浪费内存空间。
---
## for loops

## 三大组件

一个完整的 `for` 循环本质上是由三个用分号 `;` 隔开的独立部分组成的：

```
for (【1. 初始化区块】 ; 【2. 终止条件(必须是boolean)】 ; 【3. 迭代步进区块】) {
    // 【4. 循环体】
}
```

**铁律：** 只要保留那**两个分号**，这三个部分你**爱写什么写什么，爱填不填，甚至可以全空着**！

---
## 常规与变体

这是日常开发中最常见的三种等效写法，全部合法，完美等价于 Python 的 `for i in range(10)`

**1. 标准完全体 (日常最推荐)**

全部写在括号里，变量 `i` 的生命周期只在循环内，极其安全。

```
for (int i = 0; i < 10; i++) {
    System.out.println(i);
}
```

**2. 初始化外置型**

把变量声明在外面。好处是循环结束后，你依然可以使用变量 `i`。

```
int i;
for (i = 0; i < 10; i++) {
    System.out.println(i);
}
```

**3. 步进内置型**

把第三部分的 `i++` 挪进大括号的最末尾。本质上它退化成了一个 `while` 循环。

```
int i;
for (i = 0; i < 10;) { // 注意：第二个分号必须保留！
    System.out.println(i);
    i++;
}
```

---

## 骚操作与多变量

考官在 Q2 里放了 5 个正确的骚操作，重点考察你的语法边界测试。

**1. 缺失初始化的隐式继承**
因为外面已经给 `i` 赋了值，所以 `for` 的第一部分可以直接空着。
```
int i = 1;
for (; i <= 5 ; i++) {  // ✅ 正确
    System.out.println(i + ", " + (6-i));
}
```

**2. 前置递增作为初始化表达式**
第一部分只要是一个“合法的表达式语句”就行，`++i` 会在循环开始前执行一次，让 `i` 变成 1。
```
int i = 0;
for (++i; i <= 5 ; i++) { // ✅ 正确，第一部分执行 ++i
    System.out.println(i + ", " + (6-i));
}
```

**3. 多变量双管齐下 (高级用法)**
你可以在第一部分声明多个**同类型**变量，在第三部分同时更新多个变量，中间用**逗号 `,`** 隔开！
```
for (int i = 1, j = 5; i <= 5 && true; i++, j--) { // ✅ 正确
    System.out.println(i + ", " + j);
}
```
## 报错陷阱
**❌ 陷阱 1：变量未初始化就使用**
```
int i; 
for (; i <= 5 ; i++) { ... }
// 致命错误：第一部分空了，导致 i 根本没有初值就去判断 i <= 5，Java 直接拦死 (Variable 'i' might not have been initialized)。
```

**❌ 陷阱 2：无效的初始化表达式**
```
int i = 0;
for (i; i <= 5 ; i++) { ... }
// 致命错误：第一部分必须是赋值、方法调用或递增/减。单独写一个变量名 "i;" 在 Java 中不是合法的语句 (Not a statement)。
```

**❌ 陷阱 3：C/C++ 遗毒 —— 逗号条件判断**
```
for (int i = 1, j = 5; i <= 5, true; i++, j--) { ... }
// 致命错误：在 Java 的条件判断部分（第二个坑位），绝对不允许多个条件用逗号隔开！必须用 && 或 || 连接。
```

---
### i++与++i
##### 对比

假设初始状态都是 `int i = 5;`
#### 1. `++i` (先加，后用)

- **动作顺序：** 变量 `i` 先在自己肚子里 +1（变成了 6），然后再把崭新的自己交出去参与其他运算。
```
    int i = 5;
    int a = ++i; 
    
    // 结果：i 变成了 6，a 也是 6。
    // (因为 i 先把自己变成了 6，然后把 6 赋值给了 a)
```
#### 2. `i++` (先用，后加)

- **动作顺序：** 变量 `i` 先把自己**当前的老值**交出去参与运算，等别人用完之后，自己再偷偷在肚子里 +1。

```
    int i = 5;
    int a = i++; 
    
    // 结果：a 抢到了老值 5，然后 i 才变成了 6。
    // (因为 i 先把 5 赋值给了 a，然后自己才去 +1)
```
**真相是：如果 `i++` 或 `++i` 是独立成句的（没有把结果赋值给别人），那它们俩的效果 100% 是一模一样的！**

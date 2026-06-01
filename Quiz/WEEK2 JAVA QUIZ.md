---
tags:
  - Quiz
  - hashmap
---

### 静态Static
- 静态方法里不能直接调用非静态方法
- 非静态方法只能用过实例使用
- 非静态方可以修改static的变量，但是静态方法不能修改非静态变量（编译错误）
- 静态类方法中没有"this"
---
### Hashmap

##### 1. 什么是 桶 (Bucket)？

- **官方定义**：HashMap 底层数组（`Node<K,V>[] table`）中的**每一个格子**，就叫做一个“桶”。
    
- **本质**：它是一个存储特定数据的**入口地址**。
    
- **形象比喻**：就像一排带编号的**储物柜**（默认初始有 16 个柜子）。当你存东西时，Java 会根据算法计算出你该把东西放进哪一个编号的柜子里。
    

##### 2. 什么是 哈希碰撞 (Hash Collision)？

- **官方定义**：两个**不同的 Key**，通过哈希函数计算出了**相同的哈希值**（或者映射到了同一个数组索引/同一个桶）。
    
- **触发场景**：正如你上一题看到的例子：
    
    - `pointOne(1, 2)` $\rightarrow$ 算出来进 3 号桶。
        
    - `pointFour(2, 1)` $\rightarrow$ 算出来也进 3 号桶。
        
    - 两个不同的对象，同时盯上了同一个柜子，这就叫**哈希碰撞**。
        
- **注意点**：哈希碰撞是**不可避免**的。因为哪怕 Java 的哈希算法再完美，数组的长度也是有限的（比如默认 16），无限的数据映射到有限的格子里，必然会撞车。
##### 3. 发生碰撞了，HashMap 怎么处理？（拉链法 / Chaining）

Java 并没有采用“谁来得早谁占着，把后来者赶走”的策略，而是采用了“拉链法”：

1. **链表化**：当 `pointFour` 发现 3 号桶已经被 `pointOne` 占了时，它不会顶替它，而是像挂钩一样，**挂在 `pointOne` 的后面**，在当前桶形成一条**链表**。
    
2. **红黑树化（进化）**：如果运气太差，大家都往同一个桶里挤（同一个桶的链表长度 **$> 8$**，且数组总容量 **$\ge 64$**），为了防止链表太长导致查找变慢，这个桶里的链表就会自动进化成一棵**红黑树（Red-Black Tree）**，将查找效率从 $O(n)$ 瞬间提升到 $O(\log n)$。
##### 4.**“如果你重写了 `equals()` 方法，你就必须、绝对、一定要重写 `hashCode()` 方法！”**

**为什么？** 假如你重写了 `equals`，规定“只要 X 和 Y 相等，就是同一个对象”。 但是如果你**忘了重写** `hashCode`，Java 还是按内存地址把它们丢进了**不同的桶里**。 结果就是：你去 HashMap 里找 (1, 2) 的时候，系统去 A 桶翻了一下没找到，直接告诉你“没有！”，但其实它被藏在 B 桶里。你的 HashMap 直接就失效出 Bug 了！

##### 5.hashcode()与equal()（只是契约）

```
Coordinate p1 = new Coordinate(1, 2); // X=1, Y=2
Coordinate p2 = new Coordinate(2, 1); // X=2, Y=1
Coordinate p3 = new Coordinate(1, 2); // ⚠️ 注意：数据和 p1 一模一样

HashMap<Coordinate, String> map = new HashMap<>();
```

_题目已知：我们重写了 `hashCode()`，规则是 `return X + Y;`。并且我们也正确重写了 `equals()`，规则是 `只要 X 和 Y 分别相等，就 return true`。_

现在我们开始往 `HashMap` 里 `put` 东西，看看底层到底发生了什么：
#### 第一步：存入 `p1(1, 2)`

1. **触发 `hashCode()`**：1 + 2 = 3。
    
2. **找桶**：Java 跑到 3 号桶一看，空空如也。
    
3. **入驻**：根本不需要用到 `equals()`，直接把 `p1` 扔进去。
    
#### 第二步：存入 `p2(2, 1)` —— 【证明为什么需要 equals 的时刻到了！】

1. **触发 `hashCode()`**：2 + 1 = 3。
    
2. **找桶**：Java 跑到 3 号桶，发现**撞车了**！桶里已经有个 `p1` 坐在那了。
    
3. **灵魂拷问**：系统心里犯嘀咕：“新来的 `p2` 和桶里的 `p1`，到底是同一个对象，还是恰好 `hashCode` 一样的两个不同对象？”
    
4. **触发 `equals()`**：系统立刻调用 `p2.equals(p1)`。
    
    - 比较 X：`p2.X (2) == p1.X (1)` 吗？不是。
        
    - 结果：`return false;`
        
5. **判定**：哦，原来是两个不同的人恰好分到了同一个市。没关系，拉起链表，把 `p2` 挂在 `p1` 后面！
_(如果没有 `equals`，系统看到 `hashCode` 都是 3，可能就会瞎眼把 `p1` 的数据直接覆盖掉，酿成大错！)_
#### 第三步：存入 `p3(1, 2)` —— 【覆盖旧值的时刻】

1. **触发 `hashCode()`**：1 + 2 = 3。
    
2. **找桶**：跑到 3 号桶，发现里面有 `p1` 和 `p2`。
    
3. **触发 `equals()` 遍历链表**：
    
    - 先用 `p3` 问 `p1`：`p3.equals(p1)`？X 都是 1，Y 都是 2，**返回 `true`**！
        
4. **判定**：既然 `equals` 是 `true`，说明你是同一个人（Key 重复了）！
    
5. **结果**：停止遍历，不再往下挂链表，而是直接把 `p1` 身上绑定的旧 Value，替换成 `p3` 带来的新 Value。
### 总结 

1. `hashCode` 决定对象进哪一个**桶**。
    
2. `equals` 决定对象在桶里（链表上）是不是**同一个**。
    
3. 如果 `hashCode` 不同，绝对不调用 `equals`（为了性能）。
    
4. 如果 `hashCode` 相同，**必须**调用 `equals` 进行最终裁决（为了准确）。

---
### Comparable and Comparator
- `Comparable` 和 `Comparator` 都是 Java 提供用来**给对象排序的接口**（不是抽象类）
- `Comparable`实现`compareTo(T o)`;`Comparator`实现`compare(T o1, T o2)`
### `Comparable`
- **本质：** 意思是“我这个类**自己具备**和别人比较的能力”。
    
- **比喻：** 比如“按年龄排”是人类的自然规律。每个球员内部自己就知道怎么跟别人比年龄。
    
- **位置：** 写在**类内部**（比如 `implements Comparable`）。
    
- **核心方法：** `compareTo(T o)` （只有 **1** 个参数，因为是“我 (`this`)”和“别人 (`o`)”比）。
```
// 球员自己实现了 Comparable，把“按年龄升序”刻在了基因里
public class Player implements Comparable<Player> {
    int age;

    public Player(int age) { this.age = age; }

    @Override
    public int compareTo(Player other) {
        // 核心口诀：(我的 - 别人的) 就是升序！
        return this.age - other.age; 
    }
}

// 使用时，Java 自动调用它们的基因进行排序：
Collections.sort(playerList); 
```
### `Comparator`
- **本质：** 意思是“我不需要改变球员的基因，我直接从外面**请个裁判**来按新规则排”。
    
- **比喻：** 球员还是那些球员，今天教练说“按身高排”，明天说“按进球数排”。你不需要去修改球员类的代码，只需要换不同的裁判即可。
    
- **位置：** 写在**类外部**（通常新建一个裁判类，或者写个匿名内部类）。
    
- **核心方法：** `compare(T o1, T o2)` （有 **2** 个参数，因为是裁判在比较球员 1 和球员 2）。
```
// 球员类保持纯洁，什么都不用 implements
public class Player {
    int score; // 进球数
}

// 外部请来的裁判 A：专门按进球数降序排列
public class ScoreJudge implements Comparator<Player> {
    @Override
    public int compare(Player p1, Player p2) {
        // 核心口诀：(别人的 - 传进来的第一个) 就是降序！
        return p2.score - p1.score; 
    }
}

// 使用时，必须把数据和裁判一起交给 Java：
Collections.sort(playerList, new ScoreJudge()); 
```

| **对比项**  | **Comparable (内部基因)**   | **Comparator (外部裁判)**        |
| -------- | ----------------------- | ---------------------------- |
| **所属包**  | `java.lang` (核心包)       | `java.util` (工具包)            |
| **实现位置** | 必须修改对象自己的类              | 在对象类外部单独写                    |
| **重写方法** | `compareTo(obj)` (1个参数) | `compare(obj1, obj2)` (2个参数) |
| **灵活性**  | 死板。一套代码只能有一种固定排序。       | 极高。可以随时写无数个裁判（按名字、按分数...）    |
### 返回值：
无论是 `compareTo` 还是 `compare`，它们都必须返回一个 `int` 整数（正数、负数或 0）。
- **返回负数 (`-1`)：** 意思是前面的排在前面（保持原样）。
    
- **返回正数 (`1`)：** 意思是前面的比后面的大，**必须交换位置**！
    
- **返回 `0`：** 意思是一样大，不交换。

### `Comparable`中的泛型

```
class Review implements Comparable {
    int score;

    @Override
    // 注意这里的参数：只能是 Object
    public int compareTo(Object obj) {
        // 1. 强制转换：因为是 Object，你必须拿个新变量去强转（我们刚讲过）
        Review other = (Review) obj; 
        
        // 2. 比较逻辑
        return this.score - other.score;
    }
}
```

**致命危险：**

如果你在外面写了 `review1.compareTo("我是字符串")` 或者 `review1.compareTo(new Apple())`，**编译器完全不会报错**。

直到程序真正跑起来，执行到 `(Review) obj` 这一行时，因为字符串没法强转成 Review，程序直接抛出 `ClassCastException` 崩溃死机。

### 2. 如果写 `Comparable<Review>`（现代规范写法）

加了尖括号，就等于和编译器签了份死契约：“我这个 Review 类的对象，**只允许**和另一个 Review 对象比大小，别的东西滚蛋。”
```
class Review implements Comparable<Review> {
    int score;

    @Override
    // 注意这里的参数：直接变成了 Review
    public int compareTo(Review other) {
        // 1. 零强转：拿来直接用，清爽无比
        return this.score - other.score;
    }
}
```

**绝对安全：**
如果你敢在外面写 `review1.compareTo("我是字符串")`，**代码当场爆红**，IDE 根本不让你点运行。问题在写代码的阶段就被拦截了。

### 总结：本质是什么？

本质上，泛型（`<T>`）就是编译器提供的一个**门卫**。

- **消除冗余：** 帮你省去了在代码里写一堆繁琐且丑陋的 `(Review) obj` 强转代码。
    
- **提前拦截：** 把极其危险的运行时错误（Runtime Error），变成了写代码时一眼就能看出来的编译时错误（Compile-time Error）。

---
## 关键字 `final`
Java 中的 `final` 关键字用于限制修改，可以应用于变量、方法和类：
final 变量在初始化后不能被重新赋值：

```java
final int MAX_SIZE = 100;
```

例如，上面的 `MAX_SIZE` 不能被改变。这通常与 `static` 结合使用，用于定义常量。

注意，如果一个 `final` 变量引用的是一个对象，那么引用本身不能被改变，但它所指向的对象**仍然可以被修改**：
```java
final List<String> names = new ArrayList<>();
names.add("Alice"); // 允许——我们在修改对象
names = new ArrayList<>(); // 不允许——重新赋值是被禁止的
```

- final 方法不能被子类重写
- final 类不能被子类化。

 使用 `final` 有助于强制不可变性和设计约束，可能使你的代码更安全、更易于推理


## super
- **在初始化子类之前，必须先完成父类的初始化。**
### 场景一：父类有无参构造，子类“隐式”调用（一切顺利）

如果父类有一个不需要传参的构造函数，你在写子类的时候就算什么都不管，Java 也会在底层偷偷帮你调 `super()`。

```
class Parent {
    // 父类的无参构造函数
    public Parent() {
        System.out.println("1. 父类 Parent 被初始化了");
    }
}

class Child extends Parent {
    public Child() {
        // 你没有写 super(); 
        // 但编译器会自动在这一行的位置插入 super();
        System.out.println("2. 子类 Child 被初始化了");
    }
}

class Main {
    public static void main(String[] args) {
        new Child(); 
        // 运行结果：
        // 1. 父类 Parent 被初始化了
        // 2. 子类 Child 被初始化了
    }
}
```

### 【重点】：父类只有带参构造，子类不写 super（编译直接爆红）

这是新手最容易踩坑的地方。一旦你在父类里自己写了一个带参数的构造函数，Java 就不再提供默认的无参构造函数了。这时候子类如果还是指望 Java 隐式调用 `super()`，就会找不到目标。

```
class Employee {
    String name;

    // 父类只有一个带参数的构造函数，没有无参构造！
    public Employee(String name) {
        this.name = name;
    }
}

class Manager extends Employee {
    int teamSize;

    // ❌ 下面这段代码在 IntelliJ IDEA 里会直接标红报错
    public Manager(int teamSize) {
        // 编译器在这里试图偷偷插入 super();
        // 但是 Employee 类根本没有 public Employee() {} 这个方法！
        // 报错：There is no default constructor available in 'Employee'
        this.teamSize = teamSize;
    }
}
```

### 场景三：修复场景二的代码（显式调用，传参解围）

既然父类强硬要求“必须给我一个名字（name）才能完成初始化”，那子类在构造的时候，就必须老老实实地用 `super(参数)` 把值递上去，而且**必须写在第一行**。

Java

```
class Employee {
    String name;
    public Employee(String name) {
        this.name = name;
    }
}

class Manager extends Employee {
    int teamSize;

    // ✅ 正确做法：子类的构造函数要把父类需要的参数也接过来
    public Manager(String name, int teamSize) {
        // 必须在第一行显式调用 super 并传参，把 name 递给父类
        super(name); 
        // 父类初始化完之后，再初始化子类自己的属性
        this.teamSize = teamSize; 
    }
}
```

### 场景四：连子类构造函数都省略了（极端偷懒但合法）

就像你笔记里最后一段提到的：如果父类有无参构造，且子类不需要特别的初始化，你甚至可以完全不写子类的构造函数。

```
class Animal {
    public Animal() {
        System.out.println("这是一只动物");
    }
}

// 子类里面空空如也
class Dog extends Animal {
    // 编译器会自动帮你塞进以下代码：
    // public Dog() {
    //     super();
    // }
}

class Main {
    public static void main(String[] args) {
        new Dog(); // 依然会正常输出："这是一只动物"
    }
}
```

**核心总结：** `super()` 就像是在走流程填表，父类的表没填完，子类的表就不让动笔。
- 如果父类的表是空白表（无参），Java 帮你代填；
- 如果父类的表要求必须填名字和年龄（带参），你就必须亲自用 `super(名字, 年龄)` 写在第一行填好。
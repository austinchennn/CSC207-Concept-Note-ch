
+ Even More Java  
1. What are the "Java lookup rules" and how do they work?  
2. What is a generic class?  
3. What makes a class abstract? A method?  
4. What is a Java interface?  
5. How can we decide whether we should use an interface or an abstract  
class?  
6. What is the Java Collections Framework?  
7. List three classes in the Java Collections Framework.  
8. What behaviours do you expect to see in an implementing class of Set, Queue, and List?  
9. Why would we want to write  
	`List<String> x = new ArrayList<>();` instead of  `ArrayList<String> x = new ArrayList<>();`?  

10. When might you want to use List as the type of a variable instead of something more specific?  
11. When might you NOT want to use List as the type of a parameter in a method's signature?  
12. Give an example of how you can bring information from a file into your program and how to write information back to that file.


**1. Java 查找规则是什么？它们是如何工作的？ (Lookup Rules)**

- **规则是什么:** 决定程序在运行时应该调用哪个对象的方法（即动态方法分派）。
    
- **如何工作:** 查找始于对象的**实际类型**（动态类型，即 `new` 出来的那个类）。如果该类重写或定义了该方法，则直接执行；如果没有，则沿着继承树向父类逐级查找，直到找到匹配的方法。
##### 代码示例

```
class Animal {
    public void makeSound() {
        System.out.println("【Animal】动物发出未知声音");
    }

    public void sleep() {
        System.out.println("【Animal】动物在睡觉 zzz...");
    }
}

class Dog extends Animal {
    // Dog 重写了父类的 makeSound 方法
    @Override
    public void makeSound() {
        System.out.println("【Dog】汪汪汪！");
    }
    
    // 注意：Dog 没有重写 sleep 方法
}

public class Test {
    public static void main(String[] args) {
        // 声明类型是 Animal，但实际 new 出来的（动态类型）是 Dog
        Animal myAnimal = new Dog(); 

        // 场景 1：调用被重写的方法
        myAnimal.makeSound(); 

        // 场景 2：调用未被重写的方法
        myAnimal.sleep(); 
    }
}
```

##### 运行结果与查找过程解析

**1. 执行 `myAnimal.makeSound();` 时，输出结果是：`【Dog】汪汪汪！`**

- **查找过程：** Java 运行时首先看 `myAnimal` 的**实际类型**（它是 `new Dog()`）。
    
- Java 问：“`Dog` 类里有没有定义 `makeSound()` 方法？”
    
- **结果：** 有！所以直接执行 `Dog` 类里的方法。父类 `Animal` 的同名方法被忽略了。
    

**2. 执行 `myAnimal.sleep();` 时，输出结果是：`【Animal】动物在睡觉 zzz...`**

- **查找过程：** Java 依然首先看**实际类型**（`Dog`）。
    
- Java 问：“`Dog` 类里有没有定义 `sleep()` 方法？”
    
- **结果：** 没有。
    
- **向上查找：** Java 顺着继承树往上找，来到父类 `Animal`。
    
- Java 问：“`Animal` 类里有没有 `sleep()` 方法？”
    
- **结果：** 有！于是执行父类 `Animal` 里的方法。
    

**总结：** 无论变量左边的声明类型是什么（`Animal`），Java 永远从右边 `new` 出来的那个真实对象（`Dog`）开始找方法。这就是动态方法分派的核心机制。

---

**2. 什么是泛型类？ (Generic Class)**

- **定义:** 允许在定义类、接口或方法时使用“类型参数”（如 `<T>`, `<E>`）的类。
    
- **作用:** 它像一个模板。在实例化时指定具体类型（如 `List<String>`），能提供编译时的类型安全检查，避免运行时的强制类型转换和 `ClassCastException` 异常。
    

**3. 什么使一个类成为抽象类？一个方法成为抽象方法？ (Abstract Class & Method)**

- **抽象类:** 被 `abstract` 关键字修饰的类。它的核心特征是**不能被实例化**（不能 `new`）。它可以包含普通方法、状态字段以及抽象方法。
    
- **抽象方法:** 被 `abstract` 修饰，只有方法签名（定义），**没有方法体**（大括号 `{}`）。它强制要求任何继承该类的非抽象子类必须提供该方法的具体实现。
    

**4. 什么是 Java 接口？ (Java Interface)**

- **定义:** A collection methods where some are abstract and some may have default implementations specified. 接口是一种完全抽象的“契约”或行为规范（使用 `interface` 关键字）。
    
- **特征:** 传统上，它只定义方法签名而不提供实现（Java 8+ 允许有 `default` 和 `static` 方法）。类通过 `implements` 关键字来实现接口，一个类可以实现多个接口（解决 Java 单继承限制）。

1. 不能有普通的实例变量 (No Instance Variables)
2. 不能被实例化 (Cannot be Instantiated); 但可以作为Reference Type
	`List<String> ls = new List<>(); // Fails`
	**`List` 是一个接口（Interface）**，而不是一个具体的类（Class）。
	
这个为什么fail
3. 不能有构造函数 (No Constructors)
4. 不能有静态代码块 (No Static Blocks)
    
**5. 我们该如何决定应该使用接口还是抽象类？ (Interface vs. Abstract Class)**

- **选择接口:** 当你需要定义“**能做什么 (CAN-DO)**”的行为，或者当没有继承关系的毫无关联的类需要共享同一套规范时。
    
- **选择抽象类:** 当你想表达“**是什么 (IS-A)**”的强关联继承关系，并且子类之间需要共享部分通用的代码实现或成员变量（状态）时。
    

**6. 什么是 Java 集合框架？ (Java Collections Framework)**

- **定义:** Java 提供的一套标准化架构，包含了一系列用于存储、操作和管理对象集合的接口、实现类和算法。
    

**7. 列出 Java 集合框架中的三个类。 (List three classes)**

1. `ArrayList` (基于动态数组)
    
2. `LinkedList` (基于双向链表)
    
3. `HashSet` (基于哈希表的集合)

**8. 你期望在 Set、Queue 和 List 的实现类中看到什么行为？ (Behaviours of Set, Queue, and List)**

- **Set:** 不允许存储重复元素。通常无序（除了 `TreeSet` 等特定实现）。
    
- **Queue:** 遵循特定的排队规则。通常是 FIFO（先进先出），常用于任务排队或缓冲。
    
- **List:** 允许存储重复元素。保留元素的插入顺序，允许通过索引（位置序号）精确访问、插入或修改元素。
    

**9. 为什么我们想写 `List<String> x = new ArrayList<>();` 而不是 `ArrayList<String> x = new ArrayList<>();`？**

- **原因:** 遵循“**面向接口编程**”原则。这样写把变量 `x` 限制为只能使用 `List` 接口中定义的方法。这使得代码解耦，如果以后你想把底层结构从 `ArrayList` 换成 `LinkedList`，只需改等号右边即可，依赖 `x` 的后续代码完全不受影响。

**9.1 怎么选`ArrayList`和`LinkedList`：**

| **操作场景**               | **ArrayList (动态数组)** | **LinkedList (双向链表)** | **胜出者**        |
| ---------------------- | -------------------- | --------------------- | -------------- |
| **随机访问 (按索引 get/set)** | $O(1)$               | $O(N)$                | **ArrayList**  |
| **尾部追加 (add)**         | 均摊 $O(1)$            | $O(1)$                | **平局**         |
| **头部增删**               | $O(N)$               | $O(1)$                | **LinkedList** |
| **中间增删 (给索引)**         | $O(N)$               | $O(N)$ 找 + $O(1)$ 删   | **取决于具体位置**    |

**10. 何时你可能想使用 List 作为变量类型，而不是更具体的类型？**

- **场景:** 当你的代码只需要使用通用的列表操作（如 `add()`, `get()`, `remove()`, `size()`），而不需要依赖某个特定实现类（如 `ArrayList`）的独有方法（如 `trimToSize()`）或特定的内部机制时。
    

**11. 何时你可能不想使用 List 作为方法签名中的参数类型？**

- **场景 1 (需要特定性能):** 如果方法强依赖于 `LinkedList` 极高的首尾增删效率，为了防止传入 `ArrayList` 导致性能瓶颈，参数类型应设为 `LinkedList`。
    
- **场景 2 (过度限制):** 如果你的方法仅仅是遍历元素（用 `for-each`），使用更顶层的父接口 `Collection` 甚至 `Iterable` 会更好，这样方法不仅能接收 `List`，还能接收 `Set` 等其他集合。
    

**12. 举例说明如何将文件中的信息带入程序，以及如何将信息写回该文件。 (File I/O Example)**

在现代 Java (NIO.2) 中，最简洁的方法如下：

- **读文件 (Read):**
    
    Java
    
    ```
    import java.nio.file.*;
    import java.util.List;
    
    // 将文件的所有行读取到一个 List 集合中
    List<String> lines = Files.readAllLines(Paths.get("data.txt"));
    ```
    
- **写文件 (Write):**
    
    Java
    
```
    // 将 List 中的内容覆盖写回文件
    Files.write(Paths.get("data.txt"), lines);
```





### 方法重写（Overriding）**和**变量隐藏（Shadowing）
# Java 核心易错点：方法重写 vs 变量隐藏

在 Java 的继承体系中，子类和父类如果拥有同名的**方法**和**变量**，Java 虚拟机的处理机制是完全不同的。

**核心心法（一定要背下来）：**
* **调方法，看右边（看内存里 `new` 出来的实际对象类型）。**
* **拿变量，看左边（看等号左边声明的引用类型）。**
---

## 1. 方法的重写 (Overriding) —— 动态绑定 (Dynamic Binding)

* **概念**：子类重新实现了父类中同名、同参数的方法。
* **规则**：运行时，Java 会顺着继承链往下找，**调用实际对象（最底层）**的方法，完全无视变量声明的类型。

## 2. 变量的隐藏 (Shadowing / Hiding) —— 静态绑定 (Static Binding)

* **概念**：子类定义了和父类同名的变量。此时，父类的变量并没有被覆盖，而是被“隐藏”了。这两个变量在内存中是**独立并存**的。
* **规则**：编译时，Java 直接根据**引用变量声明的类型**来决定访问哪个变量。
* **最佳实践**：极度不推荐在父子类中定义同名的公开变量，这会引起极大的混乱。

---

## 3. 经典代码示例测试

假设我们有如下代码：
```java
// 父类
class Parent {
    public int x = 10;

    public void printInfo() {
        System.out.println("I am Parent.");
    }
}

// 子类
class Child extends Parent {
    // 隐藏了父类的 x
    public int x = 20;

    // 重写了父类的方法
    @Override
    public void printInfo() {
        System.out.println("I am Child.");
    }

```

### 测试场景 1：正常的子类引用

Java

```
Child c = new Child();

System.out.println(c.x);       // 输出：20 (看左边，类型是 Child)
c.printInfo();                 // 输出：I am Child. (看右边，实际是 Child)
```

### 测试场景 2：多态引用（面试最爱考）

Java

```
// 声明类型是 Parent，实际对象是 Child
Parent p = new Child();

System.out.println(p.x);       // 输出：10 ⚠️注意！变量看左边，声明类型是 Parent，所以拿 Parent 的 10。
p.printInfo();                 // 输出：I am Child. (方法看右边，实际 new 的是 Child，所以调 Child 的方法)
```

### 测试场景 3：强制类型转换
```
Child c2 = new Child();
// 把 c2 强转成父类类型
Parent p2 = (Parent) c2;

System.out.println(p2.x);      // 输出：10 (看左边，现在被当作 Parent 看了)
p2.printInfo();   
```




## 2. 为什么需要强制类型转换 (Casting for the Compiler)

* **核心原理**：Java 编译器在检查代码合法性时，**只看引用变量的类型**，不跟踪实际 `new` 出来的对象类型（因为编译器不运行代码，只做静态检查）。

* **反面教材（编译报错）**：
  ```java
  Object o = new String("hello");
  char c = o.charAt(1); // ❌ 报错！
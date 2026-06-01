## 编程语言执行模式对比：解释型 vs. 编译型 vs. 混合型
---

### 1. 解释型 (Interpreted) —— 以 Python 为例

- **基本定义**：Python 本身是一个解释器程序。
    
- **执行方式**：它会逐个语句地翻译并执行程序中的内容。
    
- **核心特点**：一次只处理 **一个语句 (one statement at a time)**。
    

### 2. 编译型 (Compiled) —— 以 C 为例

- **基本定义**：C 编译器会将**整个 C 程序**翻译成一个可执行文件。
    
- **底层实现**：该翻译后的程序包含**机器码**，能够直接在操作系统上原生运行 (runs natively)。
    
- **核心特点**：在运行前完成整体翻译，执行效率通常极高。
    

### 3. 混合型 (Hybrid) —— 以 Java 为例

- **中间翻译**：Java 程序利用编译器先被翻译成**字节码 (Bytecode)**，这是一种类似于汇编代码的中间表示形式。
    
- **虚拟机执行**：**Java 虚拟机 (JVM)** 是一个可执行程序，负责运行这些中间字节码。
    
- **动态执行与优化**：JVM 同样逐条语句地翻译并执行字节码，且在运行时会进行各种**性能优化**。
    

## Java 编译与执行过程 (Hybrid 模式)

Java 采用的是**混合型 (Hybrid)** 模式，它结合了编译型和解释型的特点。

---

### 1. 编译阶段 (Compilation)

这是将人类可读的代码转换为机器可读的中间形式的过程。

- **输入文件**：`HelloWorld.java`（源代码）。
    
- **工具**：`javac`（Java Compiler）。
    
- **命令**：`javac HelloWorld.java`
    
- **产物**：`HelloWorld.class`（**字节码 Bytecode**）。
    
- **原理**：Java 编译器将程序编译为一种类似于汇编代码的**中间表示形式 (Intermediate Representation)**。
    

### 2. 执行阶段 (Execution)

这是由虚拟机运行中间代码并产生结果的过程。

- **工具**：`java`（Java Virtual Machine, **JVM**）。
    
- **命令**：`java HelloWorld`
    
- **工作方式**：
    
    - JVM 是一个可执行程序，负责运行编译好的字节码。
        
    - 它在运行时会**逐条语句 (one statement at a time)** 翻译并执行字节码程序。
        
    - **动态优化**：JVM 在运行过程中还会寻找优化的机会，以提升程序的执行效率。
        

---
## 面向对象设计 (OOD) 关键步骤笔记

基于“截屏2026-05-12 18.50.21.png”中的教学内容，从需求描述到系统建模的转化可以归纳为以下四个核心步骤：

---

### 1. 识别核心名词 (Identify Important Nouns)

- **操作建议**：在问题描述（Problem Description）中为名词画下划线。
    
- **筛选标准**：重点关注那些可能成为**类 (Classes)** 的名词，或者描述类需要负责存储的信息的名词。
    

### 2. 选定潜在类 (Choose Potential Classes)

- **操作建议**：从上一步识别出的所有名词中，筛选并写下那些足以作为独立实体存在的**潜在类**。
    

### 3. 识别职责动词 (Identify Verbs)

- **操作建议**：圈出描述具体任务或动作的动词。
    
- **设计对应**：这些动词定义了类需要负责完成的**职责 (Responsibilities)**，在代码层面通常体现为**方法 (Methods)**。
    

### 4. 识别名词属性 (Identify Properties)

- **操作建议**：寻找那些用于描述或修饰其他重要名词的名词。
    
- **设计对应**：这些名词揭示了重要名词的特征，在类定义中构成了**实例变量 (Instance Variables)** 或属性。
    

### 设计元素对照表

|**文本语法成分**|**建模概念**|**代码实现示例**|**说明**|
|---|---|---|---|
|**名词 (Nouns)**|**类 (Class)**|`class Player`|系统中的核心实体主体|
|**动词 (Verbs)**|**职责 (Method)**|`public void move()`|对象能够执行的动作与任务|
|**属性名词**|**变量 (Variable)**|`private String name`|描述对象状态的具体数据|

**An open arrowhead  indicates inheritance. 

**A solid arrowhead  indicates composition

---
### Encapsulation

- **Service Concept**: Think of your class as providing a service.
    
- **Interface**: Provide access to information through a well-defined interface: the public methods of the class.
    
- **Implementation Hiding**: We hide the implementation details.
    
- **Advantage**: We can change the implementation to improve speed, reliability, or readability—and no other code must change!
    

---

### Class Design Tips

- **Privacy**: Everything should be as private as possible.
    
- **Instance Variables**: Always make instance variables private. Always!
    
- **API Construction**: Create public methods only when necessary as these provide an API.
    
- **Helper Methods**: If you create any helper methods, make them private.
    

---

### Inheritance Hierarchy

- **Root Class**: All classes form a tree called the inheritance hierarchy, with Object at the root.
    
- **Parent-Child Rules**: Class Object does not have a parent; all other Java classes have one parent.
    
- **Default Inheritance**: If a class has no parent declared, it is a child of class Object.
    
- **Guaranteed Methods**: Class Object guarantees that every class inherits methods `toString`, `equals`, and some others.
    

---

### The Java Memory Model

- **Multi-Part Objects**: Suppose class Child extends class Parent. An instance of class Child contains a Child part, a Parent part, and a Grandparent part, all the way up to Object （假设 Child 类继承自 Parent 类。Child 类的实例包含：Child 部分、Parent 部分、祖父部分等，一直向上追溯到 Object。）.
    
- **Substitution**: An instance of Child can be used anywhere that a Parent is legal, but not the other way around.
- 
Couterexample:
```
void printBaseInfo(ArrayList<Person> list) { ... }
```
即使 `Student` 继承自 `Person`，你也不能把 `ArrayList<Student>` 传给这个方法。编译器会直接报错。`ArrayList<Student>` 并不是 `ArrayList<Person>`

---
### Overriding and Shadowing

- **Method Overriding**: A parent's method is overridden by a subclass's method to specialize behavior.
    
- **Method Lookup**: Java calls the lowest method in the object regardless of the reference type.
    
- **Variable Shadowing**: A parent's instance variable is shadowed by a subclass's instance variable of the same name.
    
- **Variable Lookup**: Java uses the type of the reference to choose which variable to use.
    
- **Final Keyword**: If a method must not be overridden in a descendant, declare it final.
    

---

### Casting and Type Checking

- **Compiler Logic**: The Java compiler uses the type of the reference to determine whether a statement is valid.
    
- **Variable vs. Object Type**: The compiler does not keep track of object types, only variable types.
    
- **Casting Definition**: We call it casting when we change the type of an expression.
    
- **Safety Check**: At runtime, we can use the operator `instanceof` to determine whether an object really is an instance of the specified type.

### 📝 Java OOP 笔记：类型转换 (Casting)

#### 1. 核心代码示例

Java

```
Person x = new Student(...);  // 向上转型：Student 对象被视为 Person 角色
Student y = (Student) x;     // 向下转型：显式告诉编译器 x 实际上是 Student
```

2. 为什么需要写 `(Student)`？

- **编译器的视角**：如果没有这个词，编译器会认为变量 `x` 的类型就是 `Person` 。
    
- **类型安全检查**：由于编译器无法预知 `x` 在运行时是否真的引用了一个 `Student` 对象，因此它不允许直接将 `x` 赋值给 `Student` 类型的变量 `y` 。
    
- **临时身份切换**：通过添加 `(Student)`，我们可以在**这一行代码中**将 `x` 的表达式类型改为 `Student` 。
    
- **恢复原状**：完成这一行赋值后，变量 `x` 依然保持其原本声明的 `Person` 类型 。
    

3. 什么是类型转换 (Casting)？

- 当我们显式地改变一个表达式的类型时，就称之为类型转换 。
    

4. 类型转换的先决条件与限制

- **合法性基础**：只有当变量引用的对象**继承**或**直接拥有**目标类型的方法和变量时，转换才是合法的 。
    
- **实例分析**：
    
    - **成功案例**：因为 `x` 引用的对象是通过调用 `Student` 构造函数创建的，它拥有 `Student` 的所有成员，所以可以安全地转为 `Student` 。
        
    - **失败案例**：我们**不能**将 `x` 转换为 `Integer` 或 `PartTimeStudent`（除非该对象确实是 `PartTimeStudent` 的实例），因为这些类中包含该对象并不具备的成员 。
        

---

### 💡 深度理解（结合课程背景）

- **向上转型（Upcasting）** 是自动的、安全的，因为子类“是一个”父类。
    
- **向下转型（Downcasting）** 是手动的、有风险的。它就像是给编译器戴上了一副“特制眼镜”，让它看清 `Person` 护照下隐藏的 `Student` 本型。
    
- **考试提醒**：如果对象在堆内存中的本体不包含目标类型的成员，强转会在运行时抛出 `ClassCastException` 错误。因此，通常建议配合 `instanceof` 进行检查。

---

### Primitive vs. Reference Types

- **Reference Types**: The variable contains the address where the instance of a class is actually stored.
    
- **Primitive Types**: Variables like `int` store their actual values, not an address that points to a value.
    
- **Wrapper Classes**: Each primitive type has a wrapper class (e.g., `int` has `Integer`).
    
- **Autoboxing/Unboxing**: Java can autobox (automatically make an object to store the primitive) or unbox (treat the value inside an object like its corresponding primitive).
    
- **Comparison**: `==` compares whatever is stored with the variable name; for primitives, it compares the value 2, while for references, it compares the address.

---

### 📝 Java OOP 笔记：类型转换 (Casting) 全解析

#### 1. 什么是类型转换 (What is Casting?)

- **定义**：类型转换是指改变一个表达式的类型 。
    
- **前提**：只有当变量所指向的对象确实拥有目标类型的属性和方法时，转换才是合法的 。
    

#### 2. 为什么需要向上转型 (Why Upcasting / Parent Reference?)

即为什么写 `Person p = new Student();`：

- **替换原则 (Substitution Principle)**：子类的实例可以在任何父类合法的地方使用 。
    
- **多部分对象模型**：子类对象在内存中包含了自己的部分、父类部分，一直向上追溯到 `Object` 。
    
- **灵活性**：这允许我们编写通用的代码（例如一个处理 `Person` 的列表），而不需要为每个子类编写重复的逻辑。
    

#### 3. 考试重难点：编译器的“近视”逻辑 (Compiler Constraints)

这是考试中最容易出错的地方：

- **规则**：Java 编译器根据**引用的类型**（变量声明的类型）来确定语句是否有效 。
    
- **局限**：编译器不跟踪对象的实际类型，只看变量类型 。它不运行代码，只看静态声明 。
    

> **❌ 典型报错案例：**
> 
> Java
> 
> ```
> Person p = new Student(); 
> p.getStudentId(); // 编译错误 (Compile Error)!
> ```
> 
> **原因分析**： * 即使 `p` 实际指向的是一个 `Student` 对象，但它的“角色”（引用类型）是 `Person` 。 * 编译器会在 `Person` 类里寻找 `getStudentId()` 方法 。 * 如果在 `Person` 或其父类（如 `Object`）中找不到该方法，编译器就会报错，因为它无法保证 `p` 运行阶段一定是指向 `Student` 。

#### 4. 如何解决：向下转型 (Downcasting)

当你需要调用子类特有的方法时，必须显式地进行类型转换：

- **语法**：`((Student) p).getStudentId();` 。
    
- **生效范围**：这种转换通常只在那一行代码中生效，转换后变量会变回原来的父类类型 。
    

#### 5. 运行时安全检查 (Runtime Safety)

- **风险**：强制转换是危险的，如果对象实际类型不匹配，程序会崩溃 。
    
- **工具**：使用 `instanceof` 操作符在运行时检查对象的真实身份 。
    
    - **示例**：`if (p instanceof Student) { ... }` 。
        

---

我完全明白你的意思：如果子类引用既能用子类方法，又能进父类参数的方法，那为什么还要费劲写成 `Person p = new Student();` 呢？

其实，父类引用（Parent Reference）不仅仅是为了“合法”，它的真正价值在于**代码的解耦（Decoupling）**和**变量的复用性**。有些操作确实是**只有**声明为 `Person p` 才能做到，而 `Student p` 做不到的。

---

### 1. 变量的“再利用”能力 (Re-assignment)

这是最直观的区别。一个变量的类型决定了它**未来**能装什么。

- **声明为 `Person p`**： 这个变量 `p` 现在装的是 `Student`，但稍后它可以改装 `Teacher`、`Staff` 或任何 `Person` 的子类 。
    
    Java
    
    ```
    Person p = new Student(); 
    // ... 执行一些逻辑 ...
    p = new Teacher(); // ✅ 完全合法！变量 p 的容器足够大。
    ```
    
- **声明为 `Student p`**：
    
    这个变量 `p` 被永远锁死在了 `Student` 类型上。
    
    Java
    
    ```
    Student p = new Student();
    p = new Teacher(); // ❌ 编译报错！Student 容器装不下 Teacher。
    ```
    

**意义**：当你编写逻辑时，如果这个变量在不同条件下可能指向不同的子类，你就**必须**声明为父类引用。

---

### 2. 处理“遮蔽” (Shadowing) 的特定行为

这是 Java 内存模型中一个非常硬核的技术区别。

讲义提到：**变量是看引用类型（Reference Type）来选择的** 。

- 假设 `Person` 和 `Student` 都有一个变量 `int x`。
    
- 如果你写 `Person p = new Student();`，那么 `p.x` 访问的是 **Person 的 x** 。
    
- 如果你写 `Student p = new Student();`，那么 `p.x` 访问的是 **Student 的 x**。
    

**意义**：如果你在特定场景下**明确需要访问父类被遮蔽的变量**，使用父类引用是直接访问它的唯一方式 。

---

### 3. 强制实现“最小权限原则” (Encapsulation)

讲义强调：类应该被视为提供一种“服务”，且应隐藏实现细节 。

- 如果你写 `Person p = new Student();`，你是在告诉代码的其他部分：“我只把这个对象当成一个‘人’来用，我不需要也不允许你调用学生特有的复杂功能。”
    
- 这是一种**防御性编程**：它防止了程序员在不该使用子类特有功能的地方误用它们。这符合“一切应尽可能设为私有”的设计理念 。
    

---

### 4. 统一的接口与 API 保证

`Object` 类作为所有类的根，保证了每个类都拥有 `toString` 和 `equals` 等方法 。

当我们使用父类（或 Object）引用时，我们是在利用 Java 的**动态绑定（Dynamic Binding）**：

- **编译器保证**：只要是 `Person`，就一定有某方法 。
    
- **运行期惊喜**：Java 会自动调用对象中“最底层”的覆盖方法 。
    



# instanceof
`Person p = new Student();`，以下是使用 `instanceof` 的具体结果：
### 1. `p instanceof Student` 会返回 `true`

- **原因**：虽然 `p` 的声明类型是 `Person`，但它在内存中实际指向的是一个通过 `new Student()` 创建的对象 。
    
- **用途**：这是进行**向下转型 (Downcasting)** 之前的标准安全检查 。只有确认结果为 `true` 后，你才能放心地将 `p` 强转为 `Student` 以调用其特有方法 。
### 2. `p instanceof Person` 会返回 `true`

- **原因**：根据 Java 的继承规则，子类实例也是父类的一个实例 。
    
- **内部逻辑**：在内存模型中，`Student` 对象内部包含了一个完整的 `Person` 部分 。
### 3. `p instanceof Object` 会返回 `true`

- **原因**：在 Java 中，`Object` 是所有类的根类 。
    
- **内部逻辑**：所有 Java 对象（除了 `Object` 自身）都直接或间接地继承自 `Object`



除了 `instanceof` 之外，Java 确实提供了另一个方法来获取对象的类信息：**`getClass()` 方法** 。

对于你的例子 `Person p = new Student();`，调用 `p.getClass()` 的结果是 **`Student`** 。

---

###  getClass()` vs. `instanceof`

#### 1. 获取方式

- **`getClass()`**：这是 `Object` 类中定义的一个方法，因此所有 Java 类都继承并拥有该方法 。
    
- **返回值**：它返回一个 `Class` 对象，代表该实例在**运行时 (Runtime)** 的真实类型 。

#### 2. 为什么结果是 Student 而不是 Person？

- **内存模型**：当你使用 `new Student()` 时，Java 在内存中实例化的是一个 `Student` 对象 。
    
- **多部分对象**：该对象包含 `Student` 部分、`Parent`（Person）部分以及 `Object` 部分 。
    
- **运行期行为**：`getClass()` 总是指向这个“多部分对象”中最底层的、最具体的类（即该对象的“出生证明”）。
    
### 总结

- **编译器眼中的 `p`**：是一个 `Person`（决定了你直接能点出什么方法）。
    
- **`getClass()` 眼中的 `p`**：是一个 `Student`（揭示了内存中真实的物理类型）。
    理解 `Person p = new Student();` 的本质逻辑，关键在于区分 **“引用的身份（编译器视角）”** 与 **“对象的本体（运行期视角）”**。

这行代码其实是把一个复杂的对象“塞”进了一个较小的窗口里。

---
理解 `Person p = new Student();` 的本质逻辑，关键在于区分 **“引用的身份（编译器视角）”** 与 **“对象的本体（运行期视角）”**。

---

## 1. 左边 (Person p)：引用的身份（Reference Type）

**本质：这是编译器眼里的“护照”。**

- **编译器的“近视”**：Java 编译器在检查代码合法性时，不运行代码，它只看变量声明的类型 。
    
- **权限限制**：当 `p` 被声明为 `Person` 时，它在代码中扮演的就是 `Person` 的角色。编译器只允许你通过 `p` 调用 `Person` 类（或其父类 `Object`）中定义的方法 。
    
- **什么时候被认为是 Person？**
    
    - **在编译阶段**：当你试图写 `p.someMethod()` 时 。
        
    - **在变量赋值时**：如果你想把 `p` 赋值给另一个 `Person` 变量，编译器会通过 。
        
    - **在访问成员变量时**：如果父子类有同名变量（遮蔽），Java 会根据引用类型 `Person` 来选择变量 。
        
---

## 2. 右边 (new Student())：内存里的本体（Object Type）

**本质：这是在堆内存（Heap）中真实存在的“实体”。**

- **多部分对象模型**：根据 Java 内存模型，一个 `Student` 实例在内存中实际上由多个部分组成：一个 `Student` 部分、一个 `Parent` (Person) 部分，以及最顶层的 `Object` 部分 。
    
- **真实的能力**：虽然它被贴上了 `Person` 的标签，但它在内存里确实拥有 `Student` 的所有方法和变量 。
    
- **什么时候被认为是 Student？**
    
    - **在运行阶段（Runtime）**：当你调用一个被子类重写（Override）的方法时，Java 会寻找对象中最底层的实现。即：即便 `p` 是 `Person` 类型，只要它实际指向 `Student` 对象，运行的就是 `Student` 的方法 。
        
    - **使用 `instanceof` 或 `getClass()` 时**：这些工具会绕过编译器的“护照”，直接检查内存里的本体 。
        
    - **类型转换（Casting）时**：当你通过 `(Student) p` 告诉编译器“看清楚点，他其实是学生”时，它会被重新认作 `Student` 。
        

---

## 3. 本质逻辑总结表

|**维度**|**左边：Person p (引用)**|**右边：new Student() (对象)**|
|---|---|---|
|**定义**|**引用类型 (Reference Type)**|**对象类型 (Object Type)**|
|**负责时期**|**编译期 (Compile-time)**：决定哪些代码能写|**运行期 (Runtime)**：决定代码怎么执行|
|**决定内容**|决定了你能“点”出什么方法（API 范围）|决定了该方法的具体行为（Override 逻辑）|
|**内存表现**|存储的是一个地址（指针）|存储的是包含多层父类数据的真实对象|

**一句话总结：** 左边的 `Person` 决定了你**能做什么**（编译器检查），而右边的 `Student` 决定了你具体**怎么做**（运行时执行）。


---

#### 1. 包装类 (Wrapper Classes)

Java 为每种基本数据类型都提供了一个对应的引用类型，即包装类 。

- **示例**：整数可以存储为基本类型值（`int a = 2;`）或存储为 `Integer` 对象（`Integer b = new Integer(2);`） 。
    
- **为什么需要两者？**
    
    - **性能**：基本类型占用的内存更少，但它们没有构造函数或方法 。
        
    - **功能**：如果要将值存储在**泛型集合**（如 `ArrayList`）中，必须使用包装类（引用类型）。

##### 1. 错误用法（直接使用基本类型）

如果你尝试定义一个存储 `int` 的列表，Java 编译器将无法通过：

Java

```
// ❌ 编译错误！ArrayList 不支持基本类型 int
ArrayList<int> list = new ArrayList<int>(); 
```

**原因**：泛型设计要求类型参数必须是类（即 `Object` 的子类），而基本类型（如 `int`, `double`, `boolean`）在 Java 内存模型中不属于对象
        
- **转换**：基本类型与包装类之间可以相互转换，Java 通常会自动完成这一过程 。
    

---

#### 2. 自动装箱与拆箱 (Autoboxing and Unboxing)

Java 编译器能够根据上下文自动在基本类型和包装类之间进行转换 ：

- **自动装箱 (Autoboxing)**：自动创建一个对象来存储基本类型值 。
    
- **自动拆箱 (Unboxing)**：将包装类对象中的值当作基本类型来处理 。
    
### 1. 自动装箱 (Autoboxing) 举例

当 Java 发现你需要一个对象，但你只提供了一个基本类型值时，它会**自动创建一个对象来存储该值** 。

- **场景：泛型集合赋值** 由于 `ArrayList` 只能存储引用类型 ，如果你尝试添加一个 `int`：
    
    Java
    
    ```
    ArrayList<Integer> list = new ArrayList<>();
    int num = 10;
    list.add(num); // 此处发生自动装箱 
    ```
    
    编译器实际上在幕后执行了 `list.add(new Integer(num))` 。
    
- **场景：直接赋值**
    
    Java
    
    ```
    Integer y = 2; // 此处发生自动装箱 
    ```
    
    编译器自动创建了一个 `Integer` 对象来包裹数值 `2` 。
    

---

### 2. 自动拆箱 (Unboxing) 举例

当 Java 发现你需要一个基本类型数值，但你提供的是一个包装类对象时，它会**把对象里的值提取出来使用** 。

- **场景：混合比较** 根据讲义中的代码示例 ：
    
    Java
    
    ```
    int x = 2;
    Integer z = new Integer(2);
    System.out.println(x == z); // 结果为 true 
    ```
    
    **逻辑分析**：在这种情况下，Java 会**自动拆箱 `z`**，提取出其中的数值 `2` 与 `x` 进行比较，而不是比较内存地址 。
    
- **场景：算术运算**
    
    Java
    
    ```
    Integer boxNum = new Integer(5);
    int sum = boxNum + 10; // 此处发生自动拆箱 
    ```
    
    Java 会自动将 `boxNum` 转换为 `int` 类型，以便进行加法运算
---

#### 3. 比较逻辑与代码陷阱 (Comparison & Pitfalls)

基于以下代码背景：

Java

```
int x = 2;
int y = 2;
Integer z = new Integer(2);
Integer w = z;
```

- **`x == y` 为 `true`**：`==` 比较的是变量中存储的内容。由于 `x` 和 `y` 是基本类型，它们存储的是具体数值 `2` 。
    
- **`z.equals(w)` 为 `true`**：`Integer` 类的 `equals` 方法比较的是对象内部存储的**数值** 。
    
- **`z == w` 为 `true`**：两者存储的是同一个 `Integer` 对象的**内存地址** 。
    
- **`x == z` 为 `true`**：发生**自动拆箱**。Java 会自动取出 `z` 内部的值并与 `x` 比较，而不是比较地址 。
    
- **`x.equals(z)` 无法运行 (Will not run) ❌**：
    
    - **原因**：Java 不会自动将基本类型 `x` 装箱成对象来调用 `equals` 方法 。
        
    - **编译器逻辑**：由于 `equals` 方法接收的参数类型是 `Object`，编译器无法确定需要进行装箱操作 。
        

---
关于 String 的 `==` 比较，这是一个非常经典的 Java 考试考点。由于 `String` 是**引用类型 (Reference Type)** ，它的比较逻辑与 `int` 等基本类型完全不同。

以下是根据讲义整理的详细笔记：

---

###  Java OOP 笔记：String 的 `==` 比较

#### 1. 核心逻辑：比较地址而非内容 (Comparing Addresses, Not Content)

- **本质**：对于引用类型，`==` 比较的是变量名中存储的**内存地址 (Address)** 。
    
- **结果**：只有当两个变量指向堆内存中**同一个对象**时，`==` 才会返回 `true` 。
    

#### 2. 讲义案例分析 (见第 17 页)

根据讲义中的内存模型图示 ：

- **代码**：
    
    Java
    
    ```
    String obj1 = "hello"; [cite: 210]
    String obj2 = obj1;     [cite: 212]
    String obj3 = "hi";     [cite: 214]
    ```
    
- **内存表现**：
    
    - `obj1` 和 `obj2` 都存储了相同的地址（例如 `a12b`），它们指向同一个 "hello" 对象 。
        
    - **结果**：`obj1 == obj2` 为 **true**。
        
    - `obj3` 存储了一个不同的地址（例如 `c34d`） 。
        
    - **结果**：`obj1 == obj3` 为 **false**。
        

#### 3. `==` vs. `.equals()`

这是 Java 编程中最重要的一对区别：

- **`==`**：检查两个引用是否指向**同一个内存位置**（即“它们是同一个东西吗？”） 。
    
- **`.equals()`**：检查两个对象中存储的**实际内容**是否相同（即“它们看起来一样吗？”） 。
    

---

### 💡 考试避坑总结

- **字符串字面量 (String Literals)**：
    
    如果你写 `String a = "hello"; String b = "hello";`，Java 为了节省内存通常会让它们指向同一个对象。此时 `a == b` 可能为 `true`。
    
- **`new` 关键字**：
    
    如果你写 `String c = new String("hello");`，Java 会在堆中创建一个**全新**的对象。此时即使内容一样，`a == c` 也一定是 **false**，因为它们的地址不同。
    
- **基本准则**： 在 Java 中，除非你确实想检查两个引用是否指向同一个对象，否则**永远使用 `.equals()` 来比较字符串内容** 。
    

> **注意：** 讲义第 24 页提到的 `x.equals(z)` 报错（其中 `x` 是 `int`）同样适用于 String。基本类型变量没有方法，所以你不能写 `intX.equals(stringY)` 。


---

### 整数池陷阱 (The 127 vs 128 Trap)

“127 vs 128 陷阱”**只针对 `Integer` 包装类**
#### 1. 现象描述

在 Java 中，当你使用自动装箱（Autoboxing）创建 `Integer` 对象时，结果会让你惊讶：

- **127 级别**：`Integer a = 127; Integer b = 127;` -> `a == b` 返回 **`true`**。
    
- **128 级别**：`Integer c = 128; Integer d = 128;` -> `c == d` 返回 **`false`**。
    

#### 2. 本质逻辑：整数缓存 (Integer Caching)

- **为了节省内存**：Java 会预先创建好一批常用的整数对象。
    
- **缓存范围**：默认情况下，Java 会缓存所有从 **-128 到 127** 之间的 `Integer` 对象。
    
- **运作方式**：
    
    - 当你给变量赋值 `127` 时，Java 会直接从缓存池里把那个现成的对象地址给你。因为 `a` 和 `b` 拿到的地址一样，所以 `==` 比较地址时返回 `true` 。
        
    - 当你赋值 `128` 时，它超出了缓存范围。Java 会在堆内存中**新建 (new)** 两个不同的对象。因为地址不同，`==` 比较地址时返回 `false` 。
        

#### 3. 为什么讲义里写 `z == w` 是 true？ (见第 24 页)

讲义中的例子是：

Java

```
Integer z = new Integer(2);
Integer w = z;
```

- **这里 `z == w` 永远是 `true`**：因为你手动把 `z` 的地址赋给了 `w` 。这和整数池无关，这是单纯的**引用赋值**。
    
- **如果是 `new Integer(2)` 两次**：即 `Integer a = new Integer(2); Integer b = new Integer(2);`，那么 `a == b` 会是 **`false`**。因为关键字 `new` 强制在堆中创建了两个不同的新对象。
    

---

### Java 内存模型：值 vs 地址 (Value vs. Address)

#### 1. 基本类型 `int`：直接存值

- **存储内容**：当你声明 `int x = 128;` 时，变量 `x` 在内存（栈）中的那个“小盒子”里直接装的就是数字 **128**。
    
- **不是地址**：它不指向任何地方，它自己就是那个数据。
    
- **比较逻辑**：当你用 `==` 比较两个 `int` 时，Java 只是简单地看两个盒子里的数字是不是一样。
    
    - 因此，对于 `int` 来说，`128 == 128` **永远是 true**，完全没有什么“整数池”或者 127/128 的困扰。
#### 2. 引用类型 `Integer`：存储地址

- **存储内容**：当你声明 `Integer z = new Integer(128);` 时，变量 `z` 的盒子内存放的是一个**内存地址**（比如 `e56f`）。
    
- **本质**：它是一个指针，指向堆内存中那个真正的 `Integer` 对象。
    
- **比较逻辑**：当你对两个 `Integer` 对象用 `==` 时，Java 比较的是那两个**地址**。如果地址不同，哪怕里面包的数字都是 128，结果也是 `false`。
    
---

### 💡 为什么 `int` 没有 127 vs 128 的问题？

你提到的那个“神奇的 128 现象”之所以存在，是因为 Java 为了节省内存，给 **`Integer` 对象** 搞了一个“批发仓库”（整数池/Integer Cache）。

- **对于 `Integer`**：Java 会想：“既然大家经常用 -128 到 127 这些小数字，那我就预先捏好这些对象，谁要用就直接给他们同一个对象的**地址**。”
    
- **对于 `int`**：存一个数字本身就已经非常节省空间了（占用的内存极小），完全没必要搞什么“缓存池”。所以 `int` 永远是实打实的值比较。
    

---

### 总结对比

| **类型**                 | **变量里存的是什么？** | **== 比较的是什么？** | **128 会有特殊情况吗？**    |
| ---------------------- | ------------- | -------------- | ------------------- |
| **`int` (Primitive)**  | **实际数值**      | **数值**是否相等     | **没有**，永远是 true     |
| **`Integer` (Object)** | **内存地址**      | **地址**是否相同     | **有**，超出 127 地址可能不同 |
|                        |               |                |                     |
### 1. `int` 的 `==` 比的是什么？

对于基本类型，`==` 比较的是**变量中存储的实际数值** 。

- **存储内容**：`int` 变量直接存储数值（如 `5` 或 `128`），而不是指向该值的地址 。
    
- **比较逻辑**：当你执行 `x == y` 时，Java 会检查这两个变量对应的“盒子”里装的数字是否完全相同 。
    
- **结果**：如果两个 `int` 都是 `128`，结果永远是 `true`
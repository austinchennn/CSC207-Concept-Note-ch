没问题，这里是补充了完整问题题干的版本：

**Question 1: Which of the following is the best description of what an interface is in Java?**

- **答案：** A collection methods where some are abstract and some may have default implementations specified.
    
- **笔记：** 自 Java 8 起，接口不再仅限于纯抽象方法。接口中可以包含带有具体实现的 default 方法和 static 方法。选项 D（全都是抽象方法）是 Java 8 之前的旧定义。接口不能包含实例变量（只能有 public static final 常量）。
    

**Question 2: We can not create variables whose reference type is that of an Interface.**

- **答案：** False
    
- **笔记：** 引用类型（Reference type，即等号左边的声明类型）**完全可以**是接口，这是多态（Polymorphism）的基础。但是不能是Object type **实例化**接口本身（即不能 new MyInterface()）。
    

**Question 3: A class can only implement one interface**

- **答案：** False
    
- **笔记：** Java 的类不支持多重继承（只能继承一个父类），但**支持实现多个接口**。使用逗号分隔即可，例如 class C implements A, B。
    

**Question 4: A subclass implicitly implements the interface(s) its ancestors implement**

- **答案：** True
    
- **笔记：** 接口契约是可以通过继承体系传递的。如果父类实现了一个接口，子类自动（隐式地）继承该接口的类型和实现，不需要在子类再次声明 implements。
    

**Question 5: The slides refer to Generics as being "fancy type parameters". Which of the following best describes Generics in Java? Choose the best answer.**

- **答案：** Generics allow Java programmers to write code that can operate on any data type while providing compile-time type safety.
    
- **笔记：** 泛型（Generics）的核心价值在于**编译时类型安全（Compile-time type safety）**和**代码复用**。它让你在写代码时不需要强制类型转换（Casting），从而避免运行时的 ClassCastException。
    

**Question 6: An interface can not have an actual implementation of the methods**

- **答案：** False
    
- **笔记：** 理由同 Question 1。现代 Java（Java 8+）允许在接口中使用 default 关键字来提供方法的具体实现。
    

**Question 7: What happens if a class implements two interfaces with the same method? (Both interfaces have abstract methods void m();)**

- **答案：** We need to implement m in class C., since the methods in two interfaces have the same signature, we don't need to imeplement m twice.
    
- **笔记：** 当两个接口都定义了完全相同的方法签名，且都是**抽象方法**（没有具体实现）时，不存在行为冲突。类 C 只需要实现一次 m()，这个实现就能同时满足接口 A 和 B 的契约。
    

**Question 8: What happens if a class implements two interfaces with the same method with a default implementation? (Both interfaces have void m(){};)**

- **答案：** It won't work. We have to override m() to resolve the conflict between A and B.
    
- **笔记：** 这就是著名的“菱形问题（Diamond Problem）”**。如果两个接口对同一个方法都有默认实现（Default implementation），编译器会因为不知道该用哪一个而报错。解决方法是必须在类 C 中**手动重写（Override）这个方法来消除歧义（你也可以在重写时通过 A.super.m() 来显式调用某个接口的实现）。
    

**Question 9: Suppose you are having the following interface (interface DataAnalysis { T getMean(List s); ... }), what would be the best description for it?**

- **答案：** Defines statistical methods that work on a list of any type T, with both input and output using the same generic type.
    
- **笔记：** 从语法上看，这里的泛型 是无界的（Unbounded），没有写成 。因此，该接口定义了一组方法，可以接收任何类型的列表 List，并返回相同的泛型类型 T。

- **错误选项**：Defines an interface where T must be a user-defined class; all methods take a list of variables of class T. 不一定是用户自己创建的类，可以是**任何引用类型（Reference Type）**
    

**Question 10: We can Instantiate Generic Types with Primitive Types**

- **答案：** False
    
- **笔记：** Java 的泛型**不支持基本数据类型（Primitive Types）**，比如 int, double, char 等。由于泛型的类型擦除（Type Erasure）机制，你必须使用它们对应的包装类（Wrapper Classes），比如 Integer, Double, Character。不能写 List，必须写 List。
---
tags:
  - Java
  - OOP
  - Inheritance
  - Quiz
date: 2026-05-25
---
# 📝 Week 4 Quiz: Inheritance (继承)

> [!info] 知识点概览
> 本次测验主要考察 Java 中的继承机制，包括：**构造函数的隐式与显式调用 (`super`)**、**变量隐藏 (Variable Hiding)**、**方法重写 (Method Overriding)** 以及 **访问修饰符 (Access Modifiers)** 的使用限制。

---

## 📌 Question 1: Constructors & `super()`

> [!question] Question 1
> Which of the following code fragments **do not** result in an error? (哪些代码片段不会报错？)

> [!success] 正确选项 (Valid Fragments)

- [x] **Fragment 1:** (默认提供无参构造函数)
```java
public class Parent {}
public class Child extends Parent {}
```
- [x] **Fragment 2:** (父类显式定义了无参构造，子类默认 `super()` 可以找到它)

```Java
public class Parent {
  public int a;
  public Parent() { this.a = 0; }
}
public class Child extends Parent {
  public Child() {}
}
```

- [x] **Fragment 5:** (父类有无参构造，子类的默认 `super()` 可以调用成功)
```Java
public class Parent {
  public int a;
  public Parent() {}
  public Parent(int number) { this.a = number; }
}
public class Child extends Parent {
  public Child(int numb1, int numb2) {}
}
```

- [x] **Fragment 6:** (子类显式调用了父类带参构造函数)
```Java
public class Parent {
  public int a;
  public Parent(int number) { this.a = number; }
}
public class Child extends Parent {
  public Child(int numb) { super(numb); }
}
```

- [x] **Fragment 7:** (子类显式调用 `super()`，且父类存在无参构造)
    

Java

```
public class Parent {
  public int a;
  public Parent() {}
  public Parent(int number) { this.a = number; }
}
public class Child extends Parent {
  private int b;
  public Child(int numb) {
    super();
    this.b = numb;
  }
}
```

> [!fail] 错误选项解析 (Errors)
> 
> - **Fragment 3 & 4 报错原因:** 如果父类中**只定义**了带参数的构造函数（如 `Parent(int number)`），Java 就不会再自动生成无参构造函数了。而子类构造函数第一行会默认隐式调用 `super();`，由于父类找不到无参构造，因此会导致编译错误。
>     

## 📌 Question 2: Access Modifiers & Casting

> [!question] Question 2 Consider this code:
> 
> Java
> 
> ```
> public class Parent {
>     protected char a = 'a';
>     private char b = 'b';
>     public char func() { return this.b; }
> }
> ```
> 
> Which of the following code fragments **do not** result in an error?

> [!success] 正确选项 (Valid Fragments)

- [x] **Fragment 1:** (`a` 是 protected，可以通过 `super.a` 访问)
```Java
public class Child extends Parent {
  private char a;
  public Child() {
    super();
    this.a = super.a;
  }
}
```

- [x] **Fragment 3:** (可以将 `this` 强转为父类类型以访问父类的成员)
```Java
public class Child extends Parent {
  private char a;
  public Child() {
    super();
    this.a = ((Parent) this).a;
  }
}
```

- [x] **Fragment 5:** (直接访问继承自父类的 protected 变量)
```Java
public class Child extends Parent {
  private char c;
  public Child() {
    super();
    this.c = a;
  }
}
```

> [!fail] 错误选项解析 (Errors)
> 
> - **Fragment 2 (`this.a = super().a;`):** 语法错误。`super()` 只能单独作为构造方法的第一行被调用，不能当做对象引用来连写 `.a`。
>     
> - **Fragment 4 (`this.a = ((super) this).a;`):** 语法错误。`super` 是关键字，不能作为类型来进行强制转换。
>     
> - **Fragment 6 (`this.c = super.b;`):** 编译错误。变量 `b` 在 Parent 中是 `private`，子类无法直接访问。
>     
> - **Fragment 7 (`this.c = super.func();`):** 编译错误。子类定义了 `private char a;`，但却给 `this.c` 赋值。由于变量 `c` 并未声明，所以会报错。
>     

## 📌 Question 3: Method Overriding & Variable Hiding

> [!question] Question 3 Consider this code:
> 
> Java
> 
> ```
> public class Parent {
>   protected char c = 'p';
>   public void func() { System.out.println(c); }
> }
> ```
> 
> Which of the following code fragments will print **'p'**?

> [!success] 正确选项 (Prints 'p')

- [x] **Fragment 2:** (调用 `super.func()` 会执行父类的方法，输出父类的变量 `c`)

```Java
public class Child extends Parent {
  private char c = 'c';
  public void func() { super.func(); }
  public static void main(String[] args) { new Child().func(); }
}
```

- [x] **Fragment 3:** (直接显式打印父类的 `super.c`)

```Java
public class Child extends Parent {
  private char c = 'c';
  public void func() { System.out.println(super.c); }
  public static void main(String[] args) { new Child().func(); }
}
```

- [x] **Fragment 4:** (没有重写方法，直接继承。父类的方法在自己的作用域内运行，绑定的是父类的变量 `c`)
```Java
public class Child extends Parent {
  private char c = 'c';
  public static void main(String[] args) { new Child().func(); }
}
```

> [!fail] 错误选项解析 (Errors)
> 
> - **Fragment 1:** 方法体里写了 `super();`。这是语法错误，`super()` 作为构造器调用，**必须且只能**出现在构造函数的第一行，不能在普通方法 (`func`) 中调用。
>     

## 📌 Question 4: Class Implementation (Coding)

> [!question] Question 4 补全 `GradedFruit` 类，使其实现要求：
> 
> 1. 包含两个参数的构造函数 (type, rating)，默认价格为 5（继承自父类逻辑）。
>     
> 2. 包含三个参数的构造函数 (type, price, rating)。
>     
> 3. 重写 `getPrice()`：返回 `原始价格 * rating`。
>     
> 4. 重写 `toString()`：返回格式 `(type, original price, new price)`。
>     

> [!example] Solution Code

Java

```
public class GradedFruit extends Fruit {
  // 注意：虽然题目没让显式声明 rating，但基于底部给出的 get/set 方法，
  // 这里必须有一个 private double rating; 的实例变量
  private double rating; 

  // 1. Two-parameter constructor
  public GradedFruit(String type, double rating) {
    super(type); // 调用父类的 Fruit(String type)，price 默认为 5
    this.rating = rating;
  }

  // 2. Three-parameter constructor
  public GradedFruit(String type, double price, double rating) {
    super(type, price); // 调用父类的 Fruit(String type, double price)
    this.rating = rating;
  }

  // 3. Override getPrice()
  @Override
  public double getPrice() {
    // 原始价格(super.getPrice()) 乘以 评级(this.rating)
    return super.getPrice() * this.rating; 
  }

  // 4. Override toString()
  @Override
  public String toString() {
    // 格式：(type, original price, new price)
    return "(" + this.getType() + ", " + super.getPrice() + ", " + this.getPrice() + ")";
  }

  // --- 题目已提供的自带方法 ---
  public double getRating() {
    return this.rating;
  }

  public void setRating(double newRating) {
    this.rating = newRating;
  }
}
```



# Week 4 Quiz: Polymorphism (多态)

## 1. 引用类型与对象类型的转换 (Upcasting & Downcasting)

> [!summary]
> 
>  **核心原则：**
> 
> - **向上转型 (Upcasting)** 是隐式的、安全的（子类对象直接赋给父类引用）。
>    
> - **向下转型 (Downcasting)** 必须显式强转，且有潜在的运行时风险 (`ClassCastException`)。

**代码示例与分析：**
```Java
Vehicle v1 = new Motorcycle(100, 8000); // 隐式向上转型 (合法)
Vehicle v2 = new Sedan(200);            // 隐式向上转型 (合法)
Motorcycle m1 = new Motorcycle(150, 8500);
Sedan s1 = new Sedan(250);
```

- `Vehicle v3 = m1;` ✅ **合法**。子类转父类，隐式向上转型。
    
- `Vehicle v4 = s1;` ✅ **合法**。同上。
    
- `Vehicle[] vehicles = {v1, v2, (Vehicle) m1, (Vehicle) s1};` ✅ **合法**。强转 `(Vehicle)` 是多余的，但不会报错。
    
- `Sedan s2 = v2;` ❌ **编译报错**。`v2` 的静态类型是 `Vehicle`，父类引用不能直接赋给子类变量，必须显式向下转型 `(Sedan) v2`。
    
- `Sedan s3 = v1;` ❌ **编译报错**。同上。即使写成 `(Sedan) v1`，也会在**运行时**抛出 `ClassCastException`，因为 `v1` 实际指向的是 `Motorcycle` 对象。
    
- `Sedan[] sedans = {(Sedan) v1, (Sedan) v2, s1};` ❌ **运行时报错**。可以通过编译，但 `(Sedan) v1` 在运行时会失败。
    

## 2. 接口与方法调用规则 (Interface & Method Resolution)
> [!summary]
> 
> **核心原则：**
> 
> - **编译看左边，运行看右边。** 能调用什么方法，完全由变量声明时的静态类型（等号左边）**决定；方法具体怎么执行，由实际引用的**动态类型（等号右边）决定。
>     
> - 接口的多态也遵循转型规则。


**代码示例与分析：**
```Java
AlienAnimal mm = new Momo(1000); // 假设 Momo 实现了 Tradable 接口
Pikachu p1 = new Pikachu(500, 2000);
Zealot z1 = new Zealot();
```

- `Tradable t2 = p1;` ✅ **合法**。（前提是它们实现了 `Tradable` 接口）。
    
- `Tradable t1 = mm;` ❌ **编译报错**。虽然 `Momo` 实现了 `Tradable`，但变量 `mm` 的静态类型是 `AlienAnimal`，编译器不知道它是一个可交易对象，需要强转。
    
- `Tradable t1 = (Tradable) mm;` ✅ **合法**。显式转换为接口类型。
    
- `Tradable t1 = (Momo) mm;` ✅ **合法**。先向下转型为 `Momo`，因为 `Momo` 实现了 `Tradable`，随后隐式向上转型给 `t1`。
    
- `Tradable t1 = (Tradable) mm; t1.getHumanPrice();` ✅ **合法**。`Tradable` 接口中定义了 `getHumanPrice()` 方法，因此可以通过编译并执行。
    
- `Tradable t2 = (Momo) mm; t2.sound();` ❌ **编译报错**。⚠️ **高频陷阱**：虽然 `mm` 实际是一个 `Momo`（拥有 `sound()` 方法），但 `t2` 的静态声明类型是 `Tradable`。如果 `Tradable` 接口没有定义 `sound()` 方法，编译器将拒绝调用。
    

## 3. instanceof 关键字与对象同一性 (==)

> [!summary]
> **核心原则：**
> 
> - `instanceof` 检查的是内存中**实际创建的对象类型**（动态类型），以及它的父类或实现的接口。它**无视**变量的静态声明类型。
>     
> - `==` 比较的是引用地址。强转类型**不会**创建新对象，只是换了个视角（类型引用）看同一个对象。

    

**代码示例与分析：**


```java
AlienAnimal mm = new Momo(1000); // 实际对象是 Momo
Tradable t1 = (Tradable) mm;     // t1 和 mm 指向内存中同一个 Momo 对象
```

**以下表达式全部为 `true`：**

- `t1 instanceof Momo` / `mm instanceof Momo` (对象本身就是 Momo)
    
- `t1 instanceof AlienAnimal` / `mm instanceof AlienAnimal` (Momo 继承自 AlienAnimal)
    
- `t1 instanceof Tradable` / `mm instanceof Tradable` (Momo 实现了 Tradable 接口)
    
- `t1 == mm` (两者共享同一个内存地址，指向同一个实例)
    

**以下表达式全部为 `false`：**

- `t1 instanceof Pikachu` / `mm instanceof Pikachu` (它是 Momo，不是 Pikachu)
    
- `t1 instanceof Zealot` / `mm instanceof Zealot` (它是 Momo，不是 Zealot)
这是一个非常好的问题，也是学习 Java 多态（Polymorphism）时最容易绕晕的地方！

你的记忆**一半是对的，一半是错的**。

在 Java 的动态方法分派（Dynamic Method Dispatch）中，编译器（看代码的时候）和 Java 虚拟机（真正运行的时候）有着明确的分工。

我们可以用一句话总结：

👉 **“能不能调”看 Reference Type（声明类型），“具体执行谁”看 Actual Type（实际类型）。**

让我们把这个过程拆解得更清晰一点：

### 第一步：编译期检查（看 Reference Type）

当你在写代码并点击编译（或者 IDE 给你提示错误）时，Java 编译器**只看变量左边的“声明类型（Reference Type）”**，它完全不管你右边 `new` 的是什么。

编译器会拿着这个 Reference Type 去检查：“这个类型里面，到底有没有定义这个方法？”

- **如果 Reference Type 里没有这个方法：** 编译器直接报错（红线），根本不让你运行。
    
- **如果 Reference Type 里有这个方法：** 编译通过，允许你调用。
    

**举例：**

Java

```
class Animal {
    public void eat() { System.out.println("动物吃东西"); }
}

class Dog extends Animal {
    @Override
    public void eat() { System.out.println("狗吃骨头"); }
    
    public void bark() { System.out.println("汪汪汪"); }
}

public class Test {
    public static void main(String[] args) {
        // Reference Type 是 Animal，Actual Type 是 Dog
        Animal myAnimal = new Dog(); 

        // ✅ 编译通过！因为编译器去 Animal 类里看了，发现确实有 eat() 方法。
        myAnimal.eat(); 

        // ❌ 编译报错！编译器去 Animal 类里看了，发现没有 bark() 方法。
        // （虽然内存里真的是一只狗，但编译器很死板，它只认盒子表面的标签 "Animal"）
        // myAnimal.bark(); 
    }
}
```

### 第二步：运行期绑定（看 Actual Type）

如果代码编译通过了，程序开始运行。当 Java 虚拟机（JVM）真正要执行 `myAnimal.eat()` 这行代码时，它**不再看 Reference Type，而是去看内存里真真切切被 `new` 出来的那个对象的 Actual Type（实际类型）**。

这就是我之前笔记里提到的 **“查找始于实际类型”**。

- JVM 发现内存里是个 `Dog` 对象。
    
- JVM 问：“`Dog` 类有没有重写（Override）这个 `eat()` 方法？”
    
- **如果有：** 执行 `Dog` 自己的 `eat()`（这就是多态！）。
    
- **如果没有：** JVM 顺着继承树往上找，去执行父类 `Animal` 的 `eat()`。
    

**结合上面的例子：**

当运行 `myAnimal.eat();` 时，因为实际类型是 `Dog`，且 `Dog` 重写了 `eat()`，所以最终屏幕上打印的是：**“狗吃骨头”**，而不是“动物吃东西”。

### 总结笔记更新：完整的方法查找规则

为了让你的笔记更严谨，我们可以把之前的第 1 点扩展一下：

**1. Java 方法调用的完整规则是什么？ (Method Invocation Rules)**

- **编译期 (Compile-Time) - “能不能调”看声明类型：**
    
    编译器只检查变量左边的**声明类型（Reference Type）**。如果声明类型的类（或接口）中没有这个方法，直接编译报错。
    
- **运行期 (Run-Time) - “具体调哪个”看实际类型：**
    
    如果编译通过，程序运行时（这叫动态分派），查找始终始于内存中 `new` 出来的**实际类型（Actual Type）**。如果该类重写了该方法，则直接执行；如果没有，则沿着继承树向父类逐级查找，直到找到匹配的方法。
    

所以，你的记忆并没有完全错，你记住了编译期的关键步骤！只是在真正执行的那一刻，接力棒交给了“实际类型”。
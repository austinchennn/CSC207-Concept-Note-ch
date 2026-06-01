---
tags:
  - java
  - junit
  - javaswing
---
## 单元测试 (JUnit)

**1. JUnit 测试框架中有哪些常用关键字，它们的含义是什么？**

- `@Test`: 用于标记一个方法是测试方法 。
    
- `@BeforeEach`: 标记的方法会在每一个 `@Test` 方法执行之前被调用，常用于初始化 。
    
- `@AfterEach`: 标记的方法会在每一个测试方法执行之后被调用，用于清理操作 。
    
- `@BeforeAll`: 在该测试类中的所有测试方法执行之前仅运行一次 。
    
- `@AfterAll`: 在该测试类中的所有测试方法执行之后仅运行一次 。
    
- 断言关键字 (Assertions): 例如 `assertEquals`, `assertThrows`, `fail` 等，用于验证程序输出是否符合预期 。
    [[JUnit 5 测试类与注解]]
    [[Mockito 核心组件：@ExtendWith 与 @Mock]]
    

**2. 单元测试的目标是什么？**

- 核心目标是验证当你调用一个函数时，它的行为是否正确 。
    
- 通过编写大量小巧、独立的测试，来报告代码是否通过、失败或存在错误 。
    

**3. 如何测试 private helper method？**

- 课件中并未直接讲解如何专门测试私有辅助方法。IDE（如 IntelliJ）自动生成测试骨架时，主要是针对选定的 `public` 方法生成测试用例 。
    

**4. 什么是断言 (Assertions)？它们能产生什么结果？**

- 断言是用于检验程序运行结果的语句。它可以产生两种类型的结果断言：单一结果断言（Single-Outcome Assertions，例如 `fail();`）和声明结果断言（Stated Outcome Assertions，例如 `assertTrue`, `assertNotNull`） 。
    
- 另外还有用于比对值的相等性断言（如 `assertEquals`）和处理浮点数的模糊相等性断言 。
    

**5. public static variables 如何影响 setup 和 teardown？**

- 提供的课件中没有明确提及 `public static variables` 对环境配置和清理的影响。课件仅说明可以使用 `@Before*` 和 `@After*` 方法来避免重复造轮子，比如为多个测试方法创建或销毁共享的数据结构 。
    

**6. 选择测试用例的方法有哪些？**

- 针对成功的情况进行测试 。
    
- 测试一般情况、格式良好的输入以及边界情况 (boundary cases) 。
    
- 使用经典的测试案例：0、1、多个；奇数、偶数；开头、中间、结尾 。
    
- 检查数据结构的一致性（表示不变量 / representation invariants） 。
    
- 测试非典型行为，例如程序是否处理了无效输入，以及是否抛出了预期的异常 。
    

**7. 我们需要多少个测试类和实际测试用例？**

- 单元测试的模式通常是编写“大量小巧、独立的测试” 。
    
- 你可以将这些测试聚合起来，组合成测试套件 (test suites) 。
    

**8. 如何对测试进行命名、标记和记录？**

- 测试类通常以被测类名加上 "Test" 后缀命名，例如 `CommonUserTest` 。
    
- 在项目根目录下创建一个 `test` 目录，并使其包层级结构与 `src` 保持一致 。
    
- 使用 `@Test` 注解对具体的测试方法进行标记 。
    

**9. 在编写代码前先写测试（测试驱动开发 / TDD）有什么好处？**

- 让测试基于需求编写，而不是基于现有的代码编写 。
    
- 测试能够决定你需要写什么样的代码 。
    
- 这种方法有助于明确需求的定义 。
    
- 能够提供进度方面的切实证据 。
    

## 图形用户界面 (GUIs - Java Swing)

**10. 能够放入 JPanel 中的组件类名称有哪些？**

- `JButton` 。
    
- `JLabel` 。
    
- `JTextField` 。
    
- 其他嵌套的 `JPanel` 。
    

**11. Flow Layout 和 Box Layout 有什么区别？**

- `FlowLayout` 是 JPanel 的默认布局管理器，当你添加组件时，它们会从左到右依次排列流动 。
    
- `BoxLayout` 类似于进阶版的 `FlowLayout`，它支持更灵活的水平和垂直方向布局 。
    

**12. 描述一个登录界面，并解释如何在 JPanel 上放置文本框和按钮来创建它？**

- 可以通过嵌套多个 `JPanel` 来组合这个界面 。
    
- 首先，为每一个输入字段创建一个小面板，比如 `firstNamePanel` 和 `lastNamePanel`。在这些小面板里分别加入 `JLabel`（如 "First Name:"）和输入框 `JTextField` 。
    
- 然后，创建一个按钮面板 `buttonPanel`，将 "Submit" 和 "Cancel" 等 `JButton` 放进去 。
    
- 最后，创建一个 `mainPanel`，将其布局设置成纵向排列（`BoxLayout.Y_AXIS`），把前面建好的输入框面板和按钮面板依次添加进去 。
    

**13. 什么是“listener”？在 Java Swing 的代码中是如何使用的？**

- Listener (监听器) 是一个对象 。
    
- 当它被实例化后，需要被注入 (inject) 到特定的按钮对象中 。
    
- Listener 包含一个 `actionPerformed` 方法，当事件（如点击按钮）发生时，JVM 会自动调用这部分逻辑 。
    
- 在代码实现中，通常使用 `new ActionListener()` 创建一个实现了 `ActionListener` 接口的匿名类 (anonymous class)，并通过 `submit.addActionListener(...)` 附加给按钮组件 。



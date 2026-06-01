---
tags:
  - CSC207
  - Java
  - UnitTesting
  - Mockito
cover: https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSRrwkognTox2V3MUQGQaprYfeyJRVHwJUp0w&s
---
在单元测试中，我们希望测试是**孤立的 (Isolated)**。如果你的代码依赖外部网络 (`StudentApiClient`)，测试就会变得脆弱且缓慢。Mockito 允许我们“模拟”这些外部依赖，让测试在本地“虚构”出环境。

## 1. @ExtendWith(MockitoExtension.class)
* **本质**：这是一个 JUnit 5 的扩展点 (Extension)。
* **作用**：告诉 JUnit 5 在运行测试前，**启动 Mockito 引擎**。
* **如果不加它**：你在类中定义的 `@Mock` 注解将全部失效，Mockito 不会帮你实例化这些假对象，导致它们为 `null`，运行测试时会报 `NullPointerException`。

## 2. @Mock
* **本质**：自动化创建假对象的注解。
* **作用**：无需手动去 `new` 一个假类，Mockito 会通过反射机制，自动创建一个实现该接口的“空壳”对象。
* **行为特征**：
    * 默认情况下，Mock 对象的所有方法都会返回默认值（如 `null`、`0`、`false`）。
    * 它就像一个“哑巴”翻译官，直到你用 `when(...).thenReturn(...)` 给它写好剧本。

---

## 💡 协作模式：Mock 的生命周期

> [!important] 为什么它们必须一起用？
> `@ExtendWith` 是“开关”，负责启动机器；`@Mock` 是“零件”，负责生成具体的假数据。两者缺一不可。

## 💻 Obsidian 实战笔记：快速模板

```java
// 1. 必须开启 Mockito 引擎
@ExtendWith(MockitoExtension.class)
class MyTest {

    // 2. 声明假对象
    @Mock
    private StudentApiClient mockApi; // 这里 mockApi 自动变成了一个假客户端

    @Test
    void testLogic() {
        // 3. 给假对象“打桩” (Stubbing)
        // 告诉假对象：如果有人调用 fetchStudents()，你就返回这个假列表
        when(mockApi.fetchStudents()).thenReturn(List.of("Austin", "Gemini"));

        // 4. 执行业务代码
        // 此处的逻辑会调用 mockApi，从而获得我们伪造的数据
        List<String> results = mockApi.fetchStudents();

        // 5. 断言验证
        assertEquals(2, results.size());
    }
}
```
### 4. @Spy：半真半假的“间谍”对象

与 `@Mock` 从零“捏造”一个空壳不同，`@Spy` 是对一个**真实对象**进行包装（Partial Mock）。

- **核心差异**：
    
    - **`@Mock`**：完全的“替身”。默认所有方法什么都不做（返回默认值），除非你显式 `when().thenReturn()`。
        
    - **`@Spy`**：真实的“代理”。默认会调用对象的**真实逻辑**。你可以选择性地对其中某一个方法进行“打桩”（覆盖其行为），而保留其他方法的真实实现。
    
#### 💡 使用场景对比

- 当你需要测试一个类中的某一个方法，但该类中其他的依赖逻辑又很复杂或不想执行时，`@Spy` 是绝佳选择。
    

### 5. Spy 实战笔记

```Java
// 使用 @Spy 包装真实对象
@Spy
private ArrayList<String> spyList = new ArrayList<>();

@Test
void testSpy() {
    // 1. 默认行为：调用真实逻辑
    spyList.add("One");
    assertEquals(1, spyList.size()); // 走真实逻辑，size 确实变成 1

    // 2. 打桩行为：部分替换真实逻辑
    // 告诉 Spy：虽然我有真实 size() 方法，但下次调用时请返回 100
    doReturn(100).when(spyList).size();
    //或者 when(spyList.size()).thenReturn(100);

    assertEquals(100, spyList.size()); // 触发了打桩，不走真实逻辑
    assertEquals("One", spyList.get(0)); // 其他未打桩的方法仍走真实逻辑
}
```

> [!WARNING] 注意：Stubbing Spy 的坑
> 
> 使用 `@Spy` 打桩时，建议优先使用 `doReturn(...).when(spy).method()` 的写法，而不是 `when(spy.method()).thenReturn(...)`。
> 
> - **原因**：如果用 `when(...)`，Mockito 会先尝试执行一遍 `spy.method()` 的**真实代码**，这可能导致意外抛出异常或产生副作用。
>     

### 6. Mock 与 Spy 的决策指南

|**特性**|**@Mock**|**@Spy**|
|---|---|---|
|**基础实现**|全新的空壳对象|包装真实对象实例|
|**默认行为**|方法返回 null/0/false|执行真实的方法逻辑|
|**打桩目的**|为了解耦，隔离外部依赖|为了部分重写逻辑，进行边界测试|
|**适用场景**|依赖外部 API、复杂接口时|依赖旧系统代码、或需要测试部分逻辑时|

### 总结

- **`@Mock`** 是“虚构”，通过 `when-then` 构建纯粹的测试隔离环境。
    
- **`@Spy`** 是“伪装”，在保留对象原有功能的基础上，通过 `doReturn-when` 进行精确的局部控制。
    

在复杂的遗留代码（Legacy Code）重构测试时，`@Spy` 往往比 `@Mock` 更能应付那些“一部分依赖网络、一部分又是纯本地逻辑”的棘手对象。

这一部分的 Mockito 机制是否已经覆盖了你目前的需求？如果你还在处理复杂的依赖注入，我们可以探讨一下 `InjectMocks` 与 `Spy` 结合时的调用优先级问题。
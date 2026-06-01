---
tags:
  - java
  - junit
  - test
cover: https://media.licdn.com/dms/image/v2/D4E22AQFsn7DPWeEepg/feedshare-shrink_800/B4EZpjUB0MKMAg-/0/1762602786513?e=2147483647&v=beta&t=z0Xm4aVC7LrmpNCEq25Msmurh2Z35GK9bs8_99lGCbo
---

## ⏳ 执行顺序宏观视角

> [!info] 运行轨迹 (Execution Flow)
> 当你运行整个测试类时，JUnit 会严格按照以下生命周期执行：
> 
> **`@BeforeAll`**: 全局大门打开。整个测试类只执行一次！通常用于连接数据库或启动服务器。
> 
> > **`@BeforeEach`**: 在某个 `@Test` 运行前执行。常用于 new 一个干净的对象，防止测试之间互相干扰。
> > 🎯 **`@Test`** `testMethodA` 正在运行...
> > **`@AfterEach`**: 在某个 `@Test` 运行后执行。常用于清理刚才测试产生的数据。
> > ---
> > **`@BeforeEach`**: 再次运行，提供一个全新的干净环境。
> > 🎯 **`@Test`** `testMethodB` 正在运行...
> > **`@AfterEach`**: 再次清理。
> > ---
> 
> **`@AfterAll`**: 全局大门关闭。整个测试类只执行最后一次！通常用于断开数据库或关闭服务器。

---

## 💻 场景实战：购物车测试 (ShoppingCartTest)

为了彻底弄懂这些注解，我们假设正在测试一个电商网站的 `ShoppingCart`（购物车）类。我们需要连接数据库（耗时操作），并且要测试“添加商品”和“清空购物车”等功能。

### 1. 全局配置：`@BeforeAll`
* **作用**：在所有测试开始前，仅执行一次。
* **硬性要求**：方法必须是 `static`（静态的）。
* **常见应用**：建立昂贵的数据库连接、读取庞大的配置文件、启动本地测试服务器。

```java
private static DatabaseConnection dbConnection;

@BeforeAll
public static void setupAll() {
    System.out.println("⏳ @BeforeAll: 正在建立全局数据库连接...");
    dbConnection = new DatabaseConnection("localhost:3306/testdb");
    dbConnection.connect();
}
````

### 2. 独立环境：`@BeforeEach`

- **作用**：在**每一个** `@Test` 方法执行之前都会被调用一次。
    
- **常见应用**：为每个测试实例化一个全新的、干净的对象，确保测试之间绝对隔离（互不干扰）。

```Java
private ShoppingCart cart;

@BeforeEach
public void setup() {
    System.out.println("  -> @BeforeEach: 为即将运行的测试创建一个全新的空购物车。");
    cart = new ShoppingCart(dbConnection); 
}
```

### 3. 核心测试：`@Test`

- **作用**：标记这不仅是一个普通方法，而是一个需要被框架执行的测试用例。
    
- **常见应用**：调用目标方法，并使用 `assertEquals` 等断言来验证结果。

```Java
@Test
public void testAddItem() {
    System.out.println("    [测试中] 执行 testAddItem...");
    cart.addItem("MacBook Pro M4 Max");
    assertEquals(1, cart.getItemCount(), "购物车里应该有 1 件商品");
}

@Test
public void testRemoveItem() {
    System.out.println("    [测试中] 执行 testRemoveItem...");
    cart.addItem("Keychron Keyboard");
    cart.removeItem("Keychron Keyboard");
    assertEquals(0, cart.getItemCount(), "商品被移除后，购物车应该为空");
}
```

### 4. 局部清理：`@AfterEach`

- **作用**：在**每一个** `@Test` 方法执行完毕后（无论测试是成功还是抛出异常失败）都会被调用一次。
    
- **常见应用**：清理临时文件、重置内存中的缓存、关闭某个打开的输入流。
    
```Java
@AfterEach
public void tearDown() {
    System.out.println("  -> @AfterEach: 清理当前测试产生的数据。");
    cart.clearCache(); // 比如清理当前对象占据的本地缓存
}
```

### 5. 全局收尾：`@AfterAll`

- **作用**：在当前测试类中的所有测试都运行完毕后，仅执行最后一次。
    
- **硬性要求**：方法必须是 `static`（静态的）。
    
- **常见应用**：断开数据库连接、关闭测试服务器、释放占用的大量系统资源。
    

Java

```Java
@AfterAll
public static void tearDownAll() {
    System.out.println("🛑 @AfterAll: 所有测试完毕。断开全局数据库连接。");
    if (dbConnection != null) {
        dbConnection.disconnect();
    }
}
```

```

这份笔记通过结合你的运行轨迹图和具体的实战代码，非常适合作为期末复习的参考材料。如果你们之后的作业里遇到了复杂的 Mock 测试场景，这个生命周期的底座逻辑也是完全通用的。
```
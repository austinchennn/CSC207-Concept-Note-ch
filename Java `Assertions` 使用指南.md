---
tags:
  - junit
  - Mockito
  - java
cover: https://media.licdn.com/dms/image/v2/D4E22AQFsn7DPWeEepg/feedshare-shrink_800/B4EZpjUB0MKMAg-/0/1762602786513?e=2147483647&v=beta&t=z0Xm4aVC7LrmpNCEq25Msmurh2Z35GK9bs8_99lGCbo
---

## 1. 核心机制：`assertEquals` 的比较逻辑

`assertEquals` 的行为取决于比较对象的类型。

- **基本数据类型 (`int`, `double`, `boolean` 等)**：
    
    - 比较的是**字面值**（Value）。
        
    - 示例：`assertEquals(10, 10)` → `true`。
        
- **对象类型 (`String`, `Integer`, 自定义类等)**：
    
    - 内部调用 `obj.equals()` 方法。
        
    - **默认行为**：若未重写 `equals()`，默认使用 `==`（比较**内存地址**）。
        
    - **重写行为**：若重写了 `equals()`（如 `String`、`Integer`），则比较**对象内容**。
        

> [!IMPORTANT] 关键区别
> 
> - `==`：比较**内存地址**（引用是否指向同一个对象）。
>     
> - `assertEquals()`：比较**逻辑值**（通过 `equals()` 方法判断）。
>     

## 2. 常用 Assertions 方法速查

除了 `assertEquals`，JUnit 5 的 `Assertions` 类还提供了以下核心方法：

### 逻辑判断类

- `assertTrue(condition)`：断言条件为 `true`。
    
- `assertFalse(condition)`：断言条件为 `false`。
    
- `assertNull(actual)`：断言对象为 `null`。
    
- `assertNotNull(actual)`：断言对象不为 `null`。
    

### 对象与引用类

- `assertSame(expected, actual)`：断言两个引用指向**同一个内存地址**（即 `expected == actual`）。
    
- `assertNotSame(expected, actual)`：断言两个引用指向不同的内存地址。
    

### 数组与集合类

- `assertArrayEquals(expectedArray, actualArray)`：**必须使用此方法**来比较数组内容，而非 `assertEquals`（`assertEquals` 在数组上仍会比较引用）。
    

### 异常处理类

- `assertThrows(expectedType, executable)`：验证代码块是否抛出了预期的异常。
    
    Java
    
    ```
    assertThrows(IllegalArgumentException.class, () -> {
        myObject.doSomething(-1);
    });
    ```
    

## 3. 注意事项与最佳实践

- **重写规范**：自定义类如果要在 `Assertions` 中使用，**务必重写 `equals()` 和 `hashCode()`**，否则 `assertEquals` 将退化为地址比较。
    
- **数组陷阱**：永远不要对数组使用 `assertEquals`，始终使用 `assertArrayEquals`。
    
- **清晰报错**：大多数 `Assertions` 方法支持重载，可以添加自定义错误信息：
    
    - `assertEquals(expected, actual, "自定义错误消息：由于 XXX 原因导致测试失败")`
        

为了确保你对 Java 对象比较机制有清晰的理解，是否需要我为你详细解释一下 `equals()` 和 `hashCode()` 这两个方法在自定义类中应该如何配合重写？
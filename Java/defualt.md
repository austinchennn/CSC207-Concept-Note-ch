---
aliases:
tags:
  - java
---

这是一份为你整理好的完整笔记，包含了核心概念、代码对比、速查表和类比：

> [!note] Note
> 
> ### 核心概念：`default` 是一种“状态”，而不是关键字！
> 
> 在 Java 的权限控制中，`default` (又称 package-private / package-protected) **不是一个让你写在代码里的单词**。
> 
> 它指的是：**当你什么修饰符都不写时，系统自动赋予的“默认权限”**。
> 
> - **权限范围**：只有**同一个 Package（包/逻辑文件夹）**内的代码可以互相访问。包外的任何类都无法看到它。
>     

---

### 核心对比：写 `public` vs 什么都不写 (default)

#### 1. 写了 `public` 关键字（全网公开）

**表现**：任何人、任何外部包都可以直接访问。

**定义端 (位于 `com.game` 包)：**

Java

```
package com.game;

public class Hero {
    // 明确敲出了 public 关键字
    public int hp = 100; 
}
```

**调用端 (位于 `com.player` 包)：**

Java

```
package com.player;
import com.game.Hero;

public class TestA {
    public void test() {
        Hero h = new Hero();
        System.out.println(h.hp); // ✅ 成功访问！因为 hp 是 public，全局可见。
    }
}
```

---

#### 2. 什么都不写（触发 `default` 权限，包内特供）

**表现**：属性/方法被锁在当前包内。只有 `com.game` 里面的类能用，外人不行。

**定义端 (位于 `com.game` 包)：**

Java

```
package com.game;

public class Hero {
    // ⚠️ 重点：前面空着！没有 public，也没有 private。
    // 这就是 default (包级私有) 状态。
    int hp = 100; 
}
```

**调用端 (位于 `com.player` 包)：**

Java

```
package com.player;
import com.game.Hero;

public class TestB {
    public void test() {
        Hero h = new Hero();
        System.out.println(h.hp); // ❌ 编译直接报错！
        // 报错原因：hp 没写修饰符，默认是"包内可见"。
        // TestB 在 com.player 包，而 Hero 在 com.game 包，跨包拒绝访问。
    }
}
```

---

### 权限对比速查表

|**权限级别**|**在代码里怎么写？**|**同一个类内部**|**同一个包 (Package)**|**不同包的子类**|**全局 (任何包)**|
|---|---|---|---|---|---|
|**Public**|敲出 `public`|✅|✅|✅|✅|
|**Protected**|敲出 `protected`|✅|✅|✅|❌|
|**Default**|**留空！什么都不敲**|✅|✅|❌|❌|
|**Private**|敲出 `private`|✅|❌|❌|❌|

---

### 💡 形象类比（学校教学楼）

可以将 Package 想象成学校里的一栋**教学楼**：

- `public`（公开）：教学楼大门，全校任何人都能进。
    
- **`default`（不写，包内私有）**：某间具体的**教室**。只有这个班级（**同包的代码**）的学生能在里面活动，别的班（外部包）不能随便进来。
    
- `private`（私有）：学生的**书包**，只有学生自己（类本身）能碰。z zzz
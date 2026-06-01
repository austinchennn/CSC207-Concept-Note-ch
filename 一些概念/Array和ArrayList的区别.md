---
cover: https://data-flair.training/blogs/wp-content/uploads/sites/2/2018/05/java-array-vs-arraylist.webp
tags:
  - java
  - array
---

---

### 📝 Java 核心对比：Array vs. ArrayList

#### 1. 灵活性：固定大小 vs. 动态扩容

- **Array**: 一旦创建，**长度固定**。如果你定义了 `int[5]`，你就永远只能存 5 个元素。想加第 6 个？只能重新创建一个更大的数组并把数据拷过去。
    
- **ArrayList**: **动态扩容**。它在底层其实也是个数组，但它会帮你自动打理。当位置不够时，它会自动创建一个更大的数组并完成迁移。你只需要无脑 `.add()`。
    

#### 2. 存储类型：基本类型 vs. 只能存对象

- **Array**: 既可以存基本类型（`int`, `double`），也可以存引用类型（`String`, `Student`）。
    
- **ArrayList**: **只支持引用类型 (Object)**。
    
    - 正如我们之前聊到的，你不能写 `ArrayList<int>`。
        
    - 虽然你可以 `list.add(5)`，但那是 Java 偷偷帮你做了**自动装箱 (Autoboxing)**，把 `int` 变成了 `Integer` 对象。
        

#### 3. 功能与方法 (API)

- **Array**: 功能极其简陋。它只有一个属性 `.length`。你想删除中间一个元素？对不起，请手动挪动后面所有的元素。
    
- **ArrayList**: 拥有强大的“全家桶”服务。提供 `.add()`, `.remove()`, `.contains()`, `.indexOf()` 等各种方便的方法。
    

---

### 📊 核心差异对照表

|**特性**|**Array (数组)**|**ArrayList**|
|---|---|---|
|**大小**|**固定 (Fixed)**|**动态 (Resizable)**|
|**类型支持**|基本类型 + 对象|**仅支持对象** (需要包装类)|
|**语法**|`int[] arr = new int[3];`|`ArrayList<Integer> list = new ArrayList<>();`|
|**访问效率**|极高 (直接内存寻址)|略低 (经过方法调用和对象包装)|
|**获取大小**|`arr.length` (属性)|`list.size()` (方法)|
|**所属范畴**|Java 语言内置的基础语法|`java.util` 包下的集合类|

---

### 💡 什么时候用哪个？

- **用 Array 的场景**：
    
    1. 你非常明确知道数据量是多少（比如一周只有 7 天）。
        
    2. 你对性能要求到了极其苛刻的地步，或者正在处理海量的基本类型数据（省去了对象头的内存开销）。
        
    3. 你需要处理多维矩阵（`int[][]` 这种结构数组写起来更直观）。
        
- **用 ArrayList 的场景**：
    
    1. **绝大多数日常开发情况**。因为你往往不知道未来会有多少数据。
        
    2. 你需要频繁增删数据。
        
    3. 你需要利用 Java 集合框架的其他功能（比如排序、过滤等）。
        

---

### ⚠️ 一个容易混淆的点

还记得我们刚说的 `int[]` 是引用类型吗？

- `int[]` 本身是个对象，所以 `arr1 == arr2` 比的是**地址**。
    
- `ArrayList` 也是个对象，`list1 == list2` 比的也是**地址**。
    
- 但是 `ArrayList` 重写了 `.equals()`，所以 `list1.equals(list2)` 可以比**内容**；而原生的数组**没有**重写 `.equals()`，所以数组比内容必须用 `Arrays.equals(arr1, arr2)`。
    

现在的你，是更倾向于简单高效的 `int[]`，还是灵活全能的 `ArrayList` 呢？
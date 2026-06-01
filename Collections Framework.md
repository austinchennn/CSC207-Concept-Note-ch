Java 的 Collections Framework（集合框架）是 Java 基础中最核心、面试最常考、日常开发最常用的部分。要彻底掌握它，我们需要从**底层存储方式**、**核心性质**、**常用语法**以及高频易错点（踩坑指南）四个维度来拆解。

为了清晰起见，我们将主要分为三大阵营：**List（列表）**、**Set（集合）** 和 **Map（映射，虽然它不继承 Collection 接口，但属于集合框架核心）**。

### 🗺️ 第一部分：全局视野与核心接口

在深入之前，先在脑海中建立继承树：

- **`Iterable`** -> **`Collection`**
    
    - **`List`** (有序、可重复): `ArrayList`, `LinkedList`, `Vector`
        
    - **`Set`** (无序、不可重复): `HashSet`, `LinkedHashSet`, `TreeSet`
        
    - **`Queue`** (队列): `PriorityQueue`, `ArrayDeque`
        
- **`Map`** (键值对，独立接口): `HashMap`, `LinkedHashMap`, `TreeMap`, `ConcurrentHashMap`
    

### 📚 第二部分：核心实现类详解

#### 1. List 家族 (有序、可重复)

|**实现类**|**底层存储方式**|**线程安全**|**扩容机制**|**适用场景**|
|---|---|---|---|---|
|**ArrayList**|动态数组 (Object[])|否|1.5 倍扩容|频繁随机访问 (查询快)，尾部插入|
|**LinkedList**|双向链表 (Node)|否|无需扩容|频繁在头尾或中间插入/删除，不需要随机访问|
|**Vector**|动态数组 (Object[])|是 (全量 synchronized)|2 倍扩容|老旧遗留类，极少使用|

**📌 核心语法与性质：**



```Java
List<String> arrayList = new ArrayList<>(); // 默认初始容量为 10（懒加载，第一次 add 时才分配）
arrayList.add("Java");
arrayList.get(0); // O(1) 时间复杂度

List<String> linkedList = new LinkedList<>();
linkedList.addFirst("Spring"); // 链表特有操作
```

**🚨 ArrayList / LinkedList 易错点：**

1. **频繁扩容的性能损耗：** `ArrayList` 每次扩容需要进行数组拷贝（`Arrays.copyOf`）。如果已知数据量很大，**一定要指定初始容量** `new ArrayList<>(10000)`，避免频繁扩容。
    
2. **`LinkedList` 的遍历陷阱：** 千万**不要用普通的 `for (int i=0; i<size; i++)` 加上 `get(i)`** 来遍历 `LinkedList`，这会导致 $O(n^2)$ 的复杂度。必须使用增强 for 循环（底层是 Iterator）。
    

#### 2. Set 家族 (不可重复)

| **实现类**           | **底层存储方式**        | **有序性** | **允许 Null？** | **适用场景**    |
| ----------------- | ----------------- | ------- | ------------ | ----------- |
| **HashSet**       | **HashMap** 的 Key | 无序      | 允许 (只能1个)    | 去重、快速查找是否存在 |
| **LinkedHashSet** | **LinkedHashMap** | 插入顺序    | 允许           | 需要去重且保持插入顺序 |
| **TreeSet**       | **TreeMap** (红黑树) | 元素大小顺序  | **不允许**      | 需要去重且进行排序   |

**📌 核心语法与性质：**


```Java
Set<String> hashSet = new HashSet<>();
hashSet.add("Apple"); // 底层调用 map.put("Apple", PRESENT_OBJECT)

Set<Integer> treeSet = new TreeSet<>(); // 存储的元素必须实现 Comparable 接口
treeSet.add(5);
```

**🚨 Set 易错点：**

1. **自定义对象去重失效：** 如果把自定义对象塞进 `HashSet`，**必须重写 `hashCode()` 和 `equals()` 方法**。如果不重写，默认比较的是内存地址，会导致内容相同的对象被重复放入。
    
2. **`TreeSet` 强转异常 (ClassCastException)：** 存入 `TreeSet` 的对象必须实现 `Comparable` 接口，或者在创建 `TreeSet` 时传入 `Comparator`，否则在 `add()` 时会直接报错。
    

#### 3. Map 家族 (键值对 Key-Value)

|**实现类**|**底层存储方式 (Java 8+)**|**线程安全**|**允许 Null Key/Value**|**遍历顺序**|
|---|---|---|---|---|
|**HashMap**|数组 + 链表 + **红黑树**|否|允许 1个 Null Key，多个 Null Value|无序|
|**LinkedHashMap**|HashMap + **双向链表**维护顺序|否|允许|插入顺序或访问顺序 (LRU)|
|**TreeMap**|**红黑树**|否|**不允许 Null Key**，允许 Null Value|按 Key 的自然顺序或 Comparator|
|**ConcurrentHashMap**|数组 + 链表 + 红黑树 (CAS + Synchronized)|是|**绝不允许** Null Key/Value|无序|

**📌 核心语法与性质：**
```Java
Map<String, Integer> map = new HashMap<>(16); // 建议初始容量为 2 的幂次方
map.put("Java", 1);
map.putIfAbsent("Java", 2); // 存在则不覆盖
map.getOrDefault("Python", 0); // 防空指针利器
```

**🚨 HashMap / ConcurrentHashMap 易错点（面试重灾区）：**

1. **HashMap 链表转红黑树的条件：** 很多人以为链表长度超过 8 就会转红黑树。错！必须同时满足两个条件：**链表长度 > 8 且 数组总容量 >= 64**。如果容量 < 64，会优先选择扩容数组而不是转树。
    
2. **可变对象作为 Key 的灾难：** 绝对不要用一个状态可变的对象作为 `HashMap` 的 Key。如果把对象放进 Map 后，修改了参与计算 `hashCode` 的属性，该对象的哈希值会改变，导致你**永远无法在 Map 中 get 到这个值**（内存泄漏隐患）。
    
3. **Null 值的坑：** `HashMap` 可以存 null 键值对，但 `ConcurrentHashMap` **绝对不行**。如果在 `ConcurrentHashMap` 存 null 会直接抛出 `NullPointerException`。这是因为多线程环境下存在“二义性”问题（无法区分是 value 本来就是 null，还是没找到这个 key）。
    

### 💣 第三部分：极其高频的集合“血泪史”坑点

除了上述各类的专属坑点外，还有几个所有集合共通的雷区：

#### 1. `Arrays.asList()` 的伪集合陷阱

Java

```
String[] arr = {"a", "b", "c"};
List<String> list = Arrays.asList(arr);
list.add("d"); // ❌ 报错：UnsupportedOperationException
```

- **原因：** `Arrays.asList` 返回的是 `java.util.Arrays` 内部的一个私有静态类，它虽然叫 `ArrayList`，但没有实现 `add` 和 `remove` 方法。它的长度是固定的，并且与原数组**共享内存**（修改 list 会影响原数组）。
    
- **正确做法：** `List<String> list = new ArrayList<>(Arrays.asList(arr));`
    

#### 2. 遍历时删除元素的 `ConcurrentModificationException` (CME)

Java

```
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
for (String s : list) {
    if ("B".equals(s)) {
        list.remove(s); // ❌ 报错：ConcurrentModificationException
    }
}
```

- **原因：** Java 集合内部有一个 `modCount`（修改次数）变量。增强 for 循环底层用的是 Iterator。直接调用 `list.remove()` 会改变 `modCount`，但不会同步给 Iterator 的 `expectedModCount`，导致迭代器检查失败抛出异常（Fast-Fail 机制）。
    
- **正确做法：**
    
    - Java 8+: `list.removeIf(s -> "B".equals(s));` (强烈推荐)
        
    - Java 7-: 使用显式的 Iterator 调用 `iterator.remove()`。
        

#### 3. `subList` 导致的内存泄漏与异常

Java

```
List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
List<Integer> subList = list.subList(0, 2); 
```

- **坑点 1：** `subList` 返回的只是原 List 的一个视图。如果你此时向**原 List** `list` 添加或删除元素，再去操作 `subList`，会抛出 `ConcurrentModificationException`。
    
- **坑点 2（内存泄漏）：** 如果原 List 非常大，你截取了一个很小的 subList 并且一直强引用着它。因为 subList 内部持有原大 List 的引用，会导致整个大 List 无法被 GC 回收。
    

Java 集合的体系非常庞大，这里把最核心的骨架和坑点都梳理出来了。如果你要把这些知识点用于面试或者优化代码，你目前最想深入挖掘哪一部分？比如 HashMap 的扩容底层源码（如何计算哈希、为什么是 2 的幂），还是并发包下的 ConcurrentHashMap 设计原理？
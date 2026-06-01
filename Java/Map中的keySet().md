---
tags:
  - java
---

### 1. 什么是 keySet()？

- **功能**：用于获取一个 Map（或 JSONObject）中所有的**键（Key）**，并把它们打包成一个 Set 集合返回。
    
- **形象比喻**：就像一本书的**目录**。它只告诉你这本书有哪些章节名字，但不包含具体的章节内容（Value）。
    

Java

```
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 10);
map.put("Banana", 20);

// 返回包含 ["Apple", "Banana"] 的集合
Set<String> keys = map.keySet(); 
```

### 2. ⚠️ 核心考点：keySet() 返回的是“视图（View）”

这是 keySet() 最危险、也是面试最常考的特性：**它返回的集合并不是原数据的独立备份，而是原 Map 的一把“钥匙”或“窗户”。**

这意味着，你对这个返回的 Set 所做的（几乎）所有修改，都会**直接联动并篡改原 Map 的数据**！

### 3. 修改联动规则表

|**操作**|**对 keySet 的动作**|**对原 Map 的影响**|**结果/后果**|
|---|---|---|---|
|**删除**|keys.remove("Apple")|同步删除该键值对|🚨 **原 Map 数据丢失！** 连带 Value 一起被删掉。|
|**清空**|keys.clear()|清空整个 Map|💥 **毁灭性打击！** 原 Map 彻底变为空。|
|**添加**|keys.add("Orange")|无法添加，抛出异常|❌ **报错！** UnsupportedOperationException。因为 Map 必须有键和值，只给一个键，Java 不知道 Value 填什么。|

### 4. 正确姿势：如何安全地把 Keys 交给别人？

为了防止别人调用你的方法拿到 keySet() 后，手滑把你的原 Map 数据删光（这就叫 **Aliasing / 别名问题**），你必须进行**防御性拷贝（Defensive Copying）**。

**❌ 危险写法（直接暴露原数据）：**

Java

```
public Set<String> getKeys() {
    return myMap.keySet(); // 别人拿到后可以通过 .clear() 毁掉你的 myMap
}
```

**✅ 满分写法（抄写一份复印件给别人）：**

Java

```
public List<String> getKeys() {
    // 把视图里的名字挨个抄下来，写在了一张全新的纸上 (ArrayList)
    return new ArrayList<>(myMap.keySet()); 
}
```

### 5. keySet() 的经典应用场景

**用于“未知数据”的动态遍历（例如解析 JSON）。**

当你不确定一个结构里到底装了多少东西时，绝不能把 Key 写死。必须先用 keySet() 把所有的键一把抓出来，然后再用 for 循环搭配 .get(key) 去逐个提取对应的值。

Java

```
// 不管 JSON 里有 3 个国家还是 100 个国家，统统一网打尽
for (String key : jsonObject.keySet()) {
    String value = jsonObject.getString(key);
    System.out.println("键: " + key + ", 值: " + value);
}
```
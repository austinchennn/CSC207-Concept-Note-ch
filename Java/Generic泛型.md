---
tags:
  - java
cover: https://i.ytimg.com/vi/CN27X68YO4I/maxresdefault.jpg
---

### I.概述

Generic泛型的本质就是参数化类型

好处：
- 类型安全

- 消除了强制类型转换

### II.泛型类

###### 一、泛型类在创建对象的时候，

- 来指定具体的数据类型

- 若没有指定类型，按照Object类操作

- 指定的类型不能是基本数据类型primitive type（本质因为int并不继承自Object）

###### 二、泛型类的地址

同一泛型类，根据不同的数据类型创建的对象，本质是同一类型
intGeneric.getClass == strGeneric.getClass

### III.泛型类的继承
###### 从泛型类派生子类


- 子类也是泛型类，子类和父类的泛型类型要一致  (可以多，但是必须有一个一样)

		class ChildGeneric <T, E, K, ...> extends Generic<T>  

		ChildGeneric<String> childGenerict = new Generic<>();

- 子类不是泛型类，父类要明确泛型的数据类型

		class ChildGeneric extends Generic<Strings>

		ChildGeneric childGeneric = new Generic();

###### 泛型接口的使用
- 实现类不是泛型类，接口要明确数据类型

- ﻿实现类也是泛型类，实现类的泛型标识包含泛型接口的泛型标识


### IV.泛型方法

- ﻿﻿泛型类，是在实例化类的时候指明泛型的具体类型。

- ﻿﻿泛型方法，是在调用方法的时候指明泛型的具体类型。

> [!语法]
> 修饰符 <T，E，....＞ 返回值类型 方法名（形参列表 `<T>`）｛
>             方法体.…
｝



- ﻿public与返回值中间 \<T> 非常重要，可以理解为声明此方法为泛型方法
<br>
- 只有声明了 \<T> 的方法才是泛型方法，泛型类中的使用了泛型的成员方法并不是泛型方法。
<br>
- ﻿﻿\<T> 表明该方法将使用泛型类型T，此时才可以在方法中使用泛型类型T。
<br>
- ﻿﻿与泛型类的定义一样，此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型。
<br>
> [!example]
>```
>public <E> getProduct (ArrayList<E> list) {
>	return list.get(random.nextInt(list.size));
>} 
>```

- [重点]**独立性**：<u>就算类和方法用的泛型标识 T一样，但是相互独立，类的泛型入参可以与方法的入参不同</u>



**静态**：
- 泛型类的成员方法不支持静态：
- 本质是 静态属性和静态方法是属于**类（Class）**的，而不是属于某个具体的**对象** 但是泛型是在**创建对象实例**时才确定的

**泛型方法可以静态**

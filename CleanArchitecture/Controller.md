---
tags:
  - cleanarchitecture
cover: https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQLGrLI7lDeZK_9ydP8Ths520WDVpe8cQyeIw&s
---

---
在整洁架构（Clean Architecture）以及我们常见的 MVC 架构中，**Controller（控制器）** 扮演的正是我们在上一节讨论的 **Adapter（接口适配器）** 的典型角色。

如果你把整个软件系统想象成一家<font color="#f79646">高级餐厅</font>，那么：

- **顾客** = 外部世界（前端网页、手机 App、其他微服务）。
    
- **后厨主厨** = Use Case（用例层，负责真正的炒菜/核心业务逻辑）。
- 
- **服务员 / 前台接待** = **Controller（控制器）**。
    

Controller 的核心职责就是“对外接客”**和**“翻译传话”。它具体要做以下四件核心工作：

### 1. 接收请求 (Receive Request)

当外部世界想要和你的系统交互时（比如用户点击了“登录”按钮），会发送一个网络请求（通常是 HTTP 请求）。Controller 负责站在大门口把这个请求拦截下来。

- **它会看：** 这是个 GET 请求还是 POST 请求？URL 路径是什么（`/api/login`）？
    

### 2. 翻译与数据清洗 (Parse & Sanitize)

外部传进来的数据通常是“脏乱”且带有外部框架特征的（比如一大坨 JSON 字符串、HTTP Headers）。Use Case 层是非常高傲的，它绝对不看这些底层的网络数据格式。

- **Controller 的工作：** 把 JSON 里的 `username` 和 `password` 提取出来，把字符串转换成 Java 里的基础类型或内部专用的数据传输对象（DTO，Data Transfer Object）。
    
- **基础校验：** 它还会做最表层的数据格式检查。比如“邮箱格式对不对”、“密码是不是空着没填”。（注意：它只做格式校验，不做业务校验）。
    

### 3. 呼叫用例 (Invoke Use Case)

数据洗干净、打包好之后，Controller 就会去敲 Use Case（后厨）的门。

- **Controller 的动作：** 调用用例层的接口（Input Boundary），把干净的 DTO 递进去，说：“主厨，这里有一份登录申请，请处理。”
    
- 接下来，Controller 就干等着，真正的密码比对、防刷机制全交由 Use Case 去算。
    

### 4. 包装并返回响应 (Format Response)

当 Use Case 算完结果（比如登录成功，或者密码错误），会把一个纯粹的业务结果扔回给 Controller（或者通过 Presenter 扔回）。

- **Controller 的工作：** 把这个内部业务结果，重新“翻译”成外部世界能看懂的格式。
    
- 比如，把结果包装成一段标准的 JSON：`{"status": "success", "token": "xyz123"}`，并贴上一个 HTTP 状态码（成功贴 `200 OK`，失败贴 `400 Bad Request`），最后顺着网线把这盘“菜”端回给前端。
    

### 🚨 绝对不能让 Controller 做的事（架构大忌）

为了保持架构的整洁，**Controller 里绝对不能出现任何“核心业务逻辑”（Business Logic）**。

- **不能连数据库：** Controller 里绝对不准出现 SQL 语句或调用数据库操作。
    
- **不能做业务决策：** 比如“VIP 用户登录送 50 积分”，这个规则绝对不能写在 Controller 的 `if-else` 里，必须写在 Entity 或 Use Case 里。
    
### 1. Controller 与 UseCase 的关系

**结论：Controller 单向依赖 UseCase，且由 Controller 主动调用 UseCase。**

- **代码依赖关系（Dependency）：**
    
    - **Controller 依赖 UseCase。** 在 Controller 的代码顶部，你会看到 `import com.xxx.usecase.SomeInputBoundary;` 这样的导包语句。
        
    - **UseCase 绝对不依赖 Controller。** UseCase 的代码里不能出现任何关于 Controller、HTTP、Web 框架的字眼。
        
- **运行时调用关系（Invocation）：**
    
    - **Controller 调用 UseCase。** Controller 就像餐厅的前台服务员，当顾客（外部请求）下好单后，服务员负责把点菜单递给后厨的主厨（UseCase）。
        
    - **数据传递：** Controller 在调用 UseCase 时，不能把带有 Web 框架特征的对象（如 `HttpServletRequest`）传进去，必须将其剥离、打包成普通的纯 Java 数据对象（DTO / Request Model）再传给 UseCase。
        

### 2. Controller 与 Entity 的关系

**结论：在标准且严格的整洁架构中，Controller 应该“假装不认识” Entity，绝对不直接调用 Entity。**

这是一个非常容易踩坑的地方。按照 Uncle Bob 的同心圆图，“依赖由外向内”，Controller 在最外侧，Entity 在最内侧，从物理规则上来说，Controller 确实能“看见”并导入 Entity。**但是，最佳实践强烈反对这样做。**

- **代码依赖关系（Dependency）：**
    
    - **强烈建议：无依赖。** Controller 不应该 `import` 任何 Entity 类。
        
    - **原因：** Entity 包含了最核心的企业业务规则。如果 Controller 直接操作 Entity，很容易导致业务逻辑“泄漏”到 Controller 层。Controller 只配处理最简单的数据外壳（DTO）。
        
- **运行时调用关系（Invocation）：**
    
    - **绝对禁止：Controller 绝不能直接调用 Entity 的方法。**
        
    - 如果你在 Controller 里写了 `userEntity.changePassword()` 或者 `new OrderEntity()`，这在整洁架构中是严重的越权行为。这就好比前台服务员擅自跑进后厨去切菜炒菜了。
        
    - **正确的跨层隔离方式：** Controller 把基础数据（如 String 类型的 password）传给 UseCase，由 UseCase 在内部负责从数据库拉取 Entity，并调用 Entity 的方法。当 UseCase 处理完后，也不能直接把 Entity 返回给 Controller，而是应该把 Entity 拆解成普通的 Response Data，再扔回给 Controller。
        

### 💡 核心总结记忆卡片

|**关系对**|**依赖方向**|**谁调用谁**|**传递的数据格式**|**架构比喻**|
|---|---|---|---|---|
|**Controller -> UseCase**|Controller 依赖 UseCase 接口|Controller 调 UseCase|纯数据结构 (DTO / Request Model)|服务员向主厨递交点菜单|
|**Controller -> Entity**|**最好零依赖** (被完全隔离)|**严禁互相调用**|严禁直接接触|服务员严禁直接碰后厨的食材|
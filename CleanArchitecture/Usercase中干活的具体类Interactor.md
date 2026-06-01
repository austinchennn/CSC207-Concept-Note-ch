---
aliases:
tags:
  - cleanarchitecture
---

### 1. 细微的区别在哪？

- **Use Case（用例）：指的是“概念”和“层级”。** 它是系统要做的一件事情，比如“下棋”、“用户注册”、“加入购物车”。它描述的是应用级的业务流程。
    
- **Interactor（交互器）：指的是“干活的具体类”。** Uncle Bob 在书里把**实现这个用例的具体 Java 类**命名为 Interactor。它就像是一个齿轮，负责把前端（Controller）的输入、业务逻辑（Entity）、和数据库（Gateway）交互结合起来。
  

## 1. 什么是 Interactor？

在整洁架构（Clean Architecture）中，**Interactor（交互器）** 是**用例（Use Case）层的核心具体实现类**。

如果说 Use Case 描述的是系统“要做什么”（抽象概念），那么 Interactor 就是在后台“真正干活的那个人”（具体实体）。它封装了**应用特定的业务规则（Application-Specific Business Rules）**。

- **定位**：同心圆架构的中间层（Use Case 层）。
    
- **特征**：它是**纯粹的核心业务逻辑**，不包含任何具体的框架技术（无 HTTP 请求、无 SQL 语句、无 UI 渲染）。它只负责业务流程的编排。
    

## 2. Interactor 的核心职责（“业务大管家”）

一个标准的 Interactor 在接收到请求后，通常扮演一个“总调度”的角色，按顺序执行以下四步：

1. **接收输入**：从外层（Controller）通过 **Input Boundary（输入边界接口）** 接收清洗好的纯数据对象（Request Model / DTO）。
    
2. **调度实体**：从仓库/网关获取 **Entity（实体）**，并调用实体类内部的方法来执行核心的企业级业务规则。
    
3. **持久化数据**：通过 **Gateway/Repository 接口** 将更新后的实体状态保存回外部存储（如数据库）。
    
4. **输出结果**：将处理完的数据打包成响应模型（Response Model），通过 **Output Boundary（输出边界接口）** 递交给外层的 Presenter 去渲染 UI。
    

## 3. 经典代码实例：下订单（PlaceOrder）

为了清晰展现 Interactor 如何作为纽带连接内外层，我们以“提交订单”业务为例。

### 步骤 A：定义边界接口与数据模型（Use Case 层）

Java

```
// 1. 输入数据模型（纯数据结构）
public class OrderRequestModel {
    public String userId;
    public String itemId;
    public int count;
    // 构造函数省略...
}

// 2. 输入边界（Input Boundary）- 暴露给外层 Controller 调用
public interface PlaceOrderInputBoundary {
    void execute(OrderRequestModel requestModel);
}

// 3. 输出边界（Output Boundary）- 用于将结果传给外层 Presenter
public interface PlaceOrderOutputBoundary {
    void presentSuccess(String orderId, double totalPrice);
    void presentError(String message);
}

// 4. 数据网关接口（Gateway/Repository）- 供 Interactor 调用以间接操作数据库
public interface OrderDataGateway {
    boolean checkInventory(String itemId, int count);
    double getItemPrice(String itemId);
    void saveOrder(String userId, String itemId, int count, double total);
}
```

### 步骤 B：核心业务实体（Domain/Entity 层）

Java

```
// 核心企业级规则：计算折扣
public class OrderEntity {
    public static double calculateTotal(double unitPrice, int count) {
        double rawTotal = unitPrice * count;
        // 企业级铁律：满 100 减 10 元
        if (rawTotal >= 100) {
            return rawTotal - 10;
        }
        return rawTotal;
    }
}
```

### 步骤 C：核心交互器实现（Use Case 层）

这就是 **Interactor**。请注意看它是如何把上面所有的组件串联起来的：

Java

```
package usecase;

import entity.OrderEntity;

public class PlaceOrderInteractor implements PlaceOrderInputBoundary {
    
    // 通过依赖注入（DIP）引入外部接口，Interactor 只认接口，不认具体数据库或UI
    private final OrderDataGateway dataGateway;
    private final PlaceOrderOutputBoundary outputBoundary;

    public PlaceOrderInteractor(OrderDataGateway dataGateway, PlaceOrderOutputBoundary outputBoundary) {
        this.dataGateway = dataGateway;
        this.outputBoundary = outputBoundary;
    }

    @Override
    public void execute(OrderRequestModel requestModel) {
        // 第一步：业务逻辑校验（调用网关接口检查库存）
        if (!dataGateway.checkInventory(requestModel.itemId, requestModel.count)) {
            outputBoundary.presentError("商品库存不足，无法下单！");
            return;
        }

        // 第二步：获取业务数据
        double unitPrice = dataGateway.getItemPrice(requestModel.itemId);

        // 第三步：调用 Entity 执行核心企业规则（计算最终价格）
        double finalPrice = OrderEntity.calculateTotal(unitPrice, requestModel.count);

        // 第四步：持久化数据（保存到数据库）
        String generatedOrderId = "ORD_" + System.currentTimeMillis();
        dataGateway.saveOrder(requestModel.userId, requestModel.itemId, requestModel.count, finalPrice);

        // 第五步：通知输出边界（把结果推给前端展示）
        outputBoundary.presentSuccess(generatedOrderId, finalPrice);
    }
}
```

## 4. 关键辨析与面试必问

### ① Interactor 里面为什么全部都是接口引用？

在 `PlaceOrderInteractor` 中，`OrderDataGateway` 和 `PlaceOrderOutputBoundary` 全是接口。

- **原因**：这就是依赖倒置原则（DIP）的体现。Interactor 是高层核心业务，数据库（MySQL/MongoDB）和表现层（Web/App/Console）是底层细节。**高层绝不能依赖底层细节，底层细节必须依赖高层建立的接口边界。** 这样哪怕后面把 MySQL 换成 PostgreSQL，Interactor 的代码连一个标点符号都不用改。
    

### ② 怎么区分一段代码该写在 Entity 还是 Interactor 里？

- **放进 Entity**：不依赖软件系统的基础核心计算法则（如：满减折扣计算、订单总价计算）。即使公司倒闭了不用电脑办公，手算账本也必须遵守的规则。
    
- **放进 Interactor**：涉及流程编排、应用控制流的规则（如：先查库存 -> 再算钱 -> 再存库 -> 再发通知）。如果没有这套自动化软件系统，这个操作流程就不存在。
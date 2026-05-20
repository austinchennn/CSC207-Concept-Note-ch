这里为你准备了一个非常直观的代码对比。我们将模拟一个银行系统，看看把业务规则写在用例（Interactor）**里和写在**实体（Entity）里，在面对需求变更时会产生怎样截然不同的后果。

### 场景设定

我们有两个基础的业务用例：

1. **取款（WithdrawMoney）**：用户从自己账户取钱。
    
2. **转账（TransferMoney）**：用户把钱从自己账户转给别人。
    
初始的核心业务规则只有一条：**冻结的账户不能扣钱。**

### ❌ 灾难演示：把规则写在用例里（贫血模型）

在这个反面例子中，实体（`Account`）沦为了一个只有数据的空壳（数据结构）。

**1. 毫无防备的实体（仅仅是个数据包）：**

Java

```
public class Account {
    public String id;
    public double balance;
    public boolean isFrozen; // 数据全裸露在外
}
```

**2. 取款用例（自己手写规则）：**

Java

```
public class WithdrawMoneyUseCase {
    public void execute(Account account, double amount) {
        // 🚨 坏味道：用例自己在判断核心业务规则
        if (account.isFrozen) {
            throw new RuntimeException("账户已冻结，无法取款！");
        }
        if (account.balance < amount) {
            throw new RuntimeException("余额不足！");
        }
        account.balance -= amount;
        System.out.println("取款成功");
    }
}
```

**3. 转账用例（复制粘贴规则）：**

Java

```
public class TransferMoneyUseCase {
    public void execute(Account fromAccount, Account toAccount, double amount) {
        // 🚨 坏味道：同样的核心规则，在这里又写了一遍！
        if (fromAccount.isFrozen) {
            throw new RuntimeException("您的账户已冻结，无法转账！");
        }
        if (fromAccount.balance < amount) {
            throw new RuntimeException("余额不足！");
        }
        
        fromAccount.balance -= amount;
        toAccount.balance += amount;
        System.out.println("转账成功");
    }
}
```

#### 💥 灾难降临：需求变更

现在，央行下发了新规：**除了“冻结（isFrozen）”之外，如果账户因为涉诈被“风控锁死（isLockedForFraud）”，也不能扣钱，且必须上报预警。**

**你的工作量：**

1. 修改 `WithdrawMoneyUseCase`，加上 `isLockedForFraud` 的判断和上报逻辑。
    
2. 修改 `TransferMoneyUseCase`，加上一模一样的逻辑。
    
3. 如果你们还有“信用卡还款用例”、“购买理财产品用例”、“自动代扣水电费用例”……**你需要全局搜索所有扣钱的地方，把几十个用例全部改一遍！** 只要漏掉一个，系统就会出现重大 Bug（比如被风控的账户居然还能买理财），这就是所谓的灾难。
    

### ✅ 整洁架构：把规则写在实体里（充血模型）

在这个正确的例子中，实体（`Account`）是一个真正的**对象**，它懂得保护自己的数据，并自带业务规则。

**1. 强壮的实体（封装数据，暴露行为）：**

Java

```
public class Account {
    private String id;
    private double balance;
    private boolean isFrozen;
    private boolean isLockedForFraud; // 新增字段被藏在内部

    // 🌟 核心业务规则全部收敛在这里！
    public void deduct(double amount) {
        if (isFrozen) {
            throw new RuntimeException("账户已冻结，拒绝出账！");
        }
        // 新规来了？直接在这里加逻辑，其他地方根本不用动！
        if (isLockedForFraud) {
            reportToPolice(); // 上报预警
            throw new RuntimeException("账户涉险，拒绝出账！");
        }
        if (balance < amount) {
            throw new RuntimeException("余额不足！");
        }
        this.balance -= amount;
    }
    
    public void add(double amount) {
        this.balance += amount;
    }
    
    private void reportToPolice() {
        System.out.println("🚨 触发风控，正在上报警方...");
    }
}
```

**2. 取款用例（变成无脑的指挥官）：**

Java

```
public class WithdrawMoneyUseCase {
    public void execute(Account account, double amount) {
        // 用例不关心扣钱的规则是什么，它只负责下达指令
        account.deduct(amount);
        System.out.println("取款成功");
    }
}
```

**3. 转账用例（同样轻松）：**

Java

```
public class TransferMoneyUseCase {
    public void execute(Account fromAccount, Account toAccount, double amount) {
        // 先从A扣钱，再给B加钱，逻辑极其清晰
        fromAccount.deduct(amount); 
        toAccount.add(amount);
        System.out.println("转账成功");
    }
}
```

### 总结

在第二种设计中，无论央行下发多少条关于“什么时候不能扣钱”的新规，你**永远只需要修改 `Account` 实体类内部的那一个 `deduct` 方法**。

所有的用例（取款、转账、买理财）连一行代码都不需要改，因为它们只调用了 `account.deduct(amount)`。这就完美实现了架构设计中的**单一职责原则（SRP）**和**高内聚低耦合**。
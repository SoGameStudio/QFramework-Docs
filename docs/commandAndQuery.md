命令和查询
=====

命令和查询是 QFramework 架构的核心组件，它们实现了命令查询责任分离（CQRS）模式。该模式将修改状态的操作（命令）与读取状态的操作（查询）分离，从而在写操作和读操作之间建立了清晰的界限。

本页面解释了命令和查询在 QFramework 中的工作原理，如何实现自定义的命令和查询，以及使用它们的最佳实践。有关整体架构的信息，请参阅核心架构。

------

核心概念
------

在 QFramework 中，命令（Commands）和查询（Queries）用于在不同层级之间进行通信，同时保持恰当的关注点分离：

* 命令：执行改变状态的操作的实体。控制器通过发送命令来间接修改系统和模型。
* 查询：检索数据而不改变状态的实体。它们返回数据但不进行任何修改。

这种实现遵循 CQRS 模式，通过明确分离修改和读取操作来增强可维护性和可测试性。

有效使用命令和查询：

* 通过隔离操作提高可测试性
* 通过明确分离职责，增强了可维护性
* 通过封装复杂操作，使代码更易于发现
* 推广 SOLID 原则，特别是单一职责和开闭原则

------

最佳实践
-----

### 何时使用命令与直接访问

| 操作类型 | 使用命令时                                        | 使用直接访问时                    |
| ---- | -------------------------------------------- | -------------------------- |
| 修改状态 | • 操作影响多个模型/系统<br>• 操作未来可能需要扩展<br>• 操作复杂或需要验证 | 不推荐 - 始终使用 Commands 进行状态修改 |
| 读取数据 | • 数据需要计算或聚合<br>• 数据来自多个来源<br>• 逻辑复杂或可重用      | • 读取简单属性<br>• 数据访问很简单      |



### 命令与查询设计指南

1.   命名规范：
   
   * 命令：使用动词-名词语法（例如， `AddScoreCommand` ， `DeletePlayerCommand` ）
   * 查询：使用获取-名词语法（例如， `GetPlayerNameQuery` ， `GetHighScoresQuery` ）

2. 实现指南：
   
   * 保持命令和查询专注于单一职责
   * 尽可能使命令不可变
   * 在命令构造函数中验证输入
   * 尽可能从查询中返回不可变数据

3.   架构规则：
   
   * 查询中永远不要修改状态
   * 命令不应返回与其修改的状态相关的值（使用单独的查询）
   * 控制器应该使用命令来处理所有状态变化，而不是直接修改

### 错误处理

1.   在命令中：
   
   * 不返回值的命令应在内部处理错误
   * 考虑发送事件来通知成功或失败
   * 对于返回值的命令，使用能够指示成功/失败的结果类型

2.   在查询中：
   
   * 对于可能不返回有效数据的查询，考虑使用可空类型
   * 考虑文档的边缘情况和失败模式
   * 不要对预期条件抛出异常

------

### QFramework 架构中的命令和查询

下图说明了命令和查询如何在 QFramework 的架构中契合：

**QFramework CQRS 流程图**

![loading-ag-84](images/cqrs-flow.jpg)
QFramework 中的命令模式

-----------------

### 命令接口和类

QFramework 为命令提供了两个主要接口：

* `ICommand` : 基本命令接口，包含一个不返回值的 `Execute()` 方法
* `ICommand<TResult>` : 返回 `TResult` 类型结果的命令模式扩展

以及两个实现这些接口的抽象类：

* `AbstractCommand` : 不返回值的命令的基类

* `AbstractCommand<TResult>` : 返回值的命令的基类

### 创建自定义命令

要创建自定义命令：

### 发送命令

任何实现了 `ICanSendCommand` 的类都可以使用提供的扩展方法发送命令：

```csharp

// Send a command without a return value
this.SendCommand(new AddScoreCommand(100));

// Or create and send in one step (if command has a parameterless constructor)
this.SendCommand<AddScoreCommand>();

// Send a command with a return value
int totalScore = this.SendCommand(new CalculateTotalScoreCommand());
```

### 命令流程

以下时序图说明了命令如何在 QFramework 架构中流转：

  **命令执行流程**

![loading-ag-389](images/command-flow.jpg)
QFramework 中的查询模式

-----------------

### 查询接口和类

  QFramework 提供：

* `IQuery<TResult>` : 用于返回类型为 `TResult` 的查询的接口
* `AbstractQuery<T>` : 返回类型为 `T` 的查询的抽象基类

### 创建自定义查询

要创建自定义查询：

  **查询创建过程**

```csharp
public class GetPlayerNameQuery : AbstractQuery<string>
{
    protected override string OnDo()
    {
        var playerModel = this.GetModel<IPlayerModel>();
        return playerModel.Name;
    }
}
```

### 发送查询

任何实现了 `ICanSendQuery` 的类都可以使用提供的扩展方法发送查询：

```csharp
string playerName = this.SendQuery(new GetPlayerNameQuery());
```

### 查询流程

下面的时序图说明了查询如何在 QFramework 架构中流转：

  **查询执行流程**

![loading-ag-1200](images/query-flow.jpg)
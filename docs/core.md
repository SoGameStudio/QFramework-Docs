# QFramework.cs 实现

本文深入探讨了核心 QFramework.cs 文件实现，涵盖了构成 QFramework 架构系统基础的 `Architecture<T>` 类、 `IOCContainer` 和 `TypeEventSystem` 组件。

有关高层架构概念和分层设计模式的信息，请参阅核心架构。有关特定工具包的实现，请参阅工具包概述。

## 核心架构实现

QFramework.cs 文件通过大约 1000 行代码实现了一个完整的架构框架，其核心围绕三个主要组件：

![qfcs-arch](images\qfcs-arch.jpg)



## 架构类实现

`Architecture<T>` 类作为中央协调器，实现了带有依赖注入和生命周期管理的通用单例模式。

### 单例模式与初始化

该框架使用线程安全的单例模式，并支持懒加载初始化：


**关键实现细节：**

| 组件              | 类型               | 目的             |
| ----------------- | ------------------ | ---------------- |
| `mArchitecture`   | `static T`         | 单例实例存储     |
| `mInited`         | `bool`             | 初始化状态跟踪   |
| `OnRegisterPatch` | `static Action<T>` | 注册钩子的扩展点 |
| `mContainer`      | `IOCContainer`     | 依赖注入容器     |

### 注册与检索系统

该架构提供类型安全的框架组件注册和检索功能：

注册方法遵循一致的模式，组件既会在 IOC 容器中注册，如果架构已经初始化，也会被初始化。

### 命令和查询执行

命令和查询通过委托模式执行，确保正确的架构注入：

## IOCContainer 实现

`IOCContainer` 提供了一种基于类型的轻量级依赖注入解决方案

todo

### 注册和检索逻辑

容器使用 `typeof(T)` 作为键，将实例存储为对象，并在检索时进行类型转换：

| 方法                    | 实现                               | 目的                 |
| ----------------------- | ---------------------------------- | -------------------- |
| `Register<T>`           | `mInstances[typeof(T)] = instance` | 存储类型实例         |
| `Get<T>`                | `(T)mInstances[typeof(T)]`         | 使用强制类型转换检索 |
| `GetInstancesByType<T>` | `OfType<T>()` 在值上               | 获取所有接口实例     |

### 扩展方法实现模式

每个规则接口都有一个对应的静态扩展类，它提供了实际的功能：

| 规则接口            | 扩展类                      | 关键方法             |
| ------------------- | --------------------------- | -------------------- |
| `ICanGetModel`      | `CanGetModelExtension`      | `GetModel<T>()`      |
| `ICanGetSystem`     | `CanGetSystemExtension`     | `GetSystem<T>()`     |
| `ICanSendCommand`   | `CanSendCommandExtension`   | `SendCommand<T>()`   |
| `ICanRegisterEvent` | `CanRegisterEventExtension` | `RegisterEvent<T>()` |

这种模式确保组件只能访问与其架构层相应的框架功能，同时保持一个干净、强类型的 API。

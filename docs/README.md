## QFramework 简介

QFramework 是一套渐进式、快速开发框架，适用于任何类型的游戏及应用项目。

QFramework 包含一套 开发架构 和 大量的工具集。

QFramework 特性速览：

- 开发架构（QFramework.cs v1.0）
  - 简单、易上手、强大
  - MVC
  - IOC、分层支持
  - CQRS 支持
  - 符合 SOLID原则
  - 可以使用 DDD 的方式设计项目
  - 不到 1000 行代码
- 工具集（QFramework.Toolkits v0.16）
  - UIKit 界面&View快速开发&管理解决方案
    - UI、GameObject 的代码生成&自动赋值
    - 界面管理
    - 层级管理
    - 界面堆栈
    - 默认使用 ResKit 方式管理界面资源
    - 可自定义界面的加载、卸载方式
    - Manager Of Manager 架构集成（不推荐使用）
  - ResKit 资源快速开发&管理解决方案
    - AssetBundle 提供模拟模式，开发阶段无需打包即可加载资源
    - 资源名称代码生成支持
    - 同一个 API 可加载 AssetBundle、Resources、网络 和 自定义来源的资源
    - 提供一套引用计数的资源管理模型
  - AudioKit 音频管理解决方案
    - 提供背景音乐、人声、音效 三种音频播放 API
    - 音量控制
    - 默认使用 ResKit 方式管理音频资源
    - 可自定义音频的加载、卸载方式
  - CoreKit 提供大量的代码工具
    - ActionKit：动作序列执行系统
    - CodeGenKit：代码生成 & 自动序列化赋值工具
    - EventKit：提供基于类、字符串、枚举以及信号类型的事件工具集
    - FluentAPI：对大量的 Unity 和 C# 常用的 API 提供了静态扩展的封装（链式 API）
    - IOCKit：提供依赖注入容器
    - LocaleKit：本地化&多语言工具集
    - LogKit：日志工具集
    - PackageKit：包管理工具，由此可更新框架和对应的插件模块。
    - PoolKit：对象池工具集，提供对象池的基础上，也提供 ListPool 和 Dictionary Pool 等工具。
    - SingletonKit：单例工具集
    - TableKit：提供表格类数据结构的工具集

QFramework 的设计哲学是从每个细节上提升开发效率。
---
title: 什么是Akka.NET  
date: 2016-08-17 22:53:13
categories: 翻译
tags: Akka.NET
---
## 可扩展，分布式 实时业务处理

我们认为写正确的，并发的，容错的和可扩展的应用程序太难了。

很多时候，是因为我们用了错误的工具和错误的抽象层次。Akka.NET就是为了改变这种情况。

通过使用 Actor 模型，我们提升了抽象层次并且提供一个更好的平台去实现可扩展，富有弹性和可响应的应用系统-点击 [响应式宣言](http://www.reactivemanifesto.org/zh-CN) 了解更多。

对于容错性，我们采用“let it crash”（让它崩溃）的模式，电信领域采用这种模式取得了巨大成功，因为实现了可以自我修复异常并且永不停止的应用系统。Actors 也提供分布式透明[^1]的抽象和真正可扩展，容错应用程序的基础。

[^1]: 作者注：所谓分布式透明或位置透明我认为就是程序是分布式部署但给人的感觉就是在同一个环境中

Akka.NET是开源程序，在 [Apache-2.0](http://www.apache.org/licenses/LICENSE-2.0) 许可证下发行--[点我了解这种许可证](https://www.tldrlegal.com/l/apache2)。

源码地址：<https://github.com/akkadotnet/akka.net>.

## 一种独特的混合

### Actors   
Actors 给你:

* 简单，高层次抽象的并发和并行。
* 异步，无阻塞和具有高性能的事件驱动模型。
* 非常轻量级的事件驱动过程（每GB的对内存有数百万的actors）。见C# 或 F# 章节。

### 容错性
* 监管者层次具有“让他崩溃”语义。
* 监管者层级可以跨越多个虚拟机，以提供真正的容错系统。
* 卓越的高可写容错系统：自我修复，永不停止。见[容错](http://getakka.net/docs/Fault%20tolerance)。

### 位置透明[^1]
Akka中的一切都是为分布式环境中工作设计的：所有actors的交互都是通过消息传递的，一切都是异步的。

支持集群。[点我了解更多](https://github.com/akkadotnet/akka.net/pull/400)

### 持久化
Akka.Persistence 当前是beta版。[点我查看开发进度](https://github.com/akkadotnet/akka.net/tree/dev/src/core/Akka.Persistence)
### 商业支持
Akka.NET 由 [Petabridge](http://petabridge.com/) 公司提供支持。

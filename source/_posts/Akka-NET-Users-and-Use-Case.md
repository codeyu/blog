---
title: 【Akka.NET 文档】Akka.NET 用户和使用案例
date: 2016-08-28 15:09:58
categories: 翻译
tags: Akka.NET
copyright: false
---
<!--more-->
Akka.NET 被很多行业的大型组织使用。如投资和商业银行，零售和社交媒体， 仿真，游戏和博彩，汽车和交通系统， 医疗保健， 数据分析等等。 任何有高吞吐量和低延迟需求的系统都可以选择使用 Akka.NET。

Akka.NET 的使用案例有大量论述。 下面是一些实际生产用户的经验之谈：

## Akka.NET 用户

#### 交易处理 (在线游戏, 金融／银行, 贸易, 统计, 博彩, 社交媒体, 通讯)
* [CellularSales](https://youtu.be/G3ZafPNI-hk?t=31m6s)

#### 后台服务 (任何行业，任何应用)

[IVC 业务系统](http://ivcbusinesssystems.com/):

> Sam Covington, IVC 业务系统: 我们用Akka.NET实现了一套内部的“Actor”系统，这使我们创新和提高生产力，不用重复发明轮子。这种微服务的后端是我们所有产品和服务的基础。我们用它在企业社交产品，和Livescan办公产品如客户指纹识别。

#### 并发和并行 (任何程序)
* [SNL Financial (McGraw Hill 子公司): Akka.NET 在华尔街的使用](https://petabridge.com/blog/akkadotnet-goes-to-wall-street/)

#### 模拟计算
Master/Worker, Compute Grid, MapReduce 等等。

#### 批处理 (任何领域)
Camel integration 通过批量数据源 Actors 分治批量工作任务

#### 通信 Hub (电信, Web 媒介, 移动媒介)
* [EventDay: 用Akka.NET 实现可扩展的会议和事件管理](https://youtu.be/G3ZafPNI-hk?t=6m16s)

#### 游戏和博彩
垂直扩展，水平扩展，容错性 / 高可用

#### 商业智能/数据挖掘/通用计算
* [用Octopus发布系统和 Akka.NET 实现 Domain.au 的实时流处理 ](https://twitter.com/philiplaureano/status/735976018993778688)


#### 物联网（IoT）
* [Synchromatics: 用 Akka.NET 实现实时公共交通跟踪与分析](https://youtu.be/YuY1ziEqifU?t=3m38s)

#### 复杂事件处理（CEP）

* [MarkedUp 分析: 用分布式 Actor 系统和 Akka.NET 实现实时营销自动化](http://www.aaronstannard.com/markedup-akkadotnet/)
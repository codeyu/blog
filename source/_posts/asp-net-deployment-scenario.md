---
title: 【Akka.NET 文档】asp.net 部署场景
date: 2016-08-21 23:16:10
categories: 翻译
tags: Akka.NET
copyright: false
description:
---
<!--more-->
### 创建ASP.NET 资源

寄宿在ASP.NET应用中很容易。 首先，在 Global.asax 里设置下。

```csharp
public class MvcApplication : System.Web.HttpApplication
{
    protected static ActorSystem ActorSystem;
    //这里声明你的顶层 actor-refs
    protected static IActorRef MyActor;

    protected void Application_Start()
    {
        //你的 mvc 配置。 在这之前或之后初始化你的Actor 系统都无所谓
        ActorSystem = ActorSystem.Create("app");
        //这里注册你的顶层 actors
        MyActor = ActorSystem.ActorOf<MyActor>();
    }
}
```

正如你所见，这里的要点是确保对`ActorSystem` 的静态引用。 这确保它不会意外的被垃圾回收，并且在Web 应用的开始和停止事件中得到创建和处理。

>**警告**<br>尽管寄宿在ASP.NET 应用里很容易。 **忠告之语**：当IIS 抽风时，你的App 所在的应用程序池（applicationpool）随之停止或启动。这意味着  `ActorSystem` 可能随时被中断。

通常情况下，你在ASP.NET应用程序里面使用一个非常轻量级的`ActorSystem` ，并通过Akka.Remote/ Akka.Cluster把重型工作分流到一个单独的Windows 服务里。

### Controllers 和 Akka.NET 的交互
下面的例子使用 Web API Controller:
```csharp
public class SomeController  : ApiController
{
      //使用异步Action
      public async Task<SomeResult> Post(SomeRequest someRequest)
      {
           //根据传入的参数，使用之前创建的Actor 发送一个消息
           //并且使用`Ask` 异步方法等待发送消息的结果
           var result = await MvcApplication.MyActor.Ask<SomeResult>(new SomeMessage(someRequest.SomeArg1,someRequest.SomeArg2));
           return result;
      }
}
```
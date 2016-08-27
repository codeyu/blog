---
title: 【Akka.NET 文档】Azure 云服务部署场景
date: 2016-08-27 10:21:42
categories: 翻译
tags: Akka.NET
copyright: false
---
<!--more-->
下面的示例假设您已经创建了一个新的包含一个空`Worker Role` 的Azure PaaS云服务。通过安装[Azure .Net SDK](http://azure.microsoft.com/en-gb/downloads/)将云服务模版添加到Visual Studio。

在部署到云之前，可以在本地使用“Azure服务模拟器”对`Worker Role`的实现进行测试。 参考MSDN Azure 的文章 ["使用模拟器在本地调试和运行一个云服务"](https://msdn.microsoft.com/en-us/library/azure/dn339018.aspx) 了解更多细节。

Azure PaaS `Worker Role`的实现和[Windows Service 部署场景](http://codeyu.com/2016/08/26/windows-service-deployment-scenario/)的例子很相似。   
开始使用 Akka.Net 的最快方式是创建一个简单的`Worker Role`，在它的 RunAsync() 方法里调用 top-level
user-actor，如下所示:

#### WorkerRole.cs
```csharp
using Akka.Actor;
```
```csharp
namespace MyActorWorkerRole
{
    public class WorkerRole : RoleEntryPoint
    {
        private readonly CancellationTokenSource cancellationTokenSource = new CancellationTokenSource();
        private readonly ManualResetEvent runCompleteEvent = new ManualResetEvent(false);

        private ActorSystem _actorSystem;

        public override bool OnStart()
        {
            // Setup the Actor System
            _actorSystem = ActorSystem.Create("MySystem");

            return (base.OnStart());
        }

        public override void OnStop()
        {
            this.cancellationTokenSource.Cancel();
            this.runCompleteEvent.WaitOne();

            // Shutdown the Actor System
            _actorSystem.Shutdown();

            base.OnStop();
        }

        public override void Run()
        {
            try
            {
                this.RunAsync(this.cancellationTokenSource.Token).Wait();
            }
            finally
            {
                this.runCompleteEvent.Set();
            }
        }

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // Create an instance to the top-level user Actor
            var workerRoleActor = _actorSystem.ActorOf<WorkerRoleActor>("WorkerRole");

            // Send a message to the Actor
            workerRoleActor.Tell(new WorkerRoleMessage("Hello World!"));

            while (!cancellationToken.IsCancellationRequested)
            {
                await Task.Delay(1000);
            }
        }
    }
}
```
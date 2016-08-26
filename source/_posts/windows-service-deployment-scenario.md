---
title: windows service deployment scenario
date: 2016-08-26 23:25:36
categories: 翻译
tags: Akka.NET
copyright: false
---
<!--more-->
对于Windows Service 的部署， 推荐使用
[TopShelf](http://topshelf.readthedocs.org/en/latest/index.html) 创建 Windows Service。它从根本上简化了Windows Service 的托管。

最快的方式入门TopShelf 是创建一个控制台程序，它长得像下面的样子:

#### Program.cs
```csharp
using Akka.Actor;
using Topshelf;
```
```csharp
class Program
{
    static void Main(string[] args)
    {
        HostFactory.Run(x =>
        {
            x.Service<MyActorService>(s =>
            {
                s.ConstructUsing(n => new MyActorService());
                s.WhenStarted(service => service.Start());
                s.WhenStopped(service => service.Stop());
                //继续和重新启动指令也是可用的
            });

            x.RunAsLocalSystem();
            x.UseAssemblyInfoForServiceInfo();
        });
    }
}

/// <summary>
/// 这个类扮演你的应用程序和TopShelf之间的接头人
/// </summary>
public class MyActorService
{
    private ActorSystem mySystem;

    public void Start()
    {
        //this is where you setup your actor system and other things
        mySystem = ActorSystem.Create("MySystem");
    }

    public async void Stop()
    {
        //this is where you stop your actor system
        await mySystem.Terminate();
    }
}
```

如你所见，上面这个例子是最简单的使用TopShelf 的方式。 不过也有其他与TopShelf集成的方式，给你更多的控制。

使用 Topshelf 安装非常简单，在命令行输入`myConsoleApp.exe install` 就行了。

更多选项和设置请参阅[Topshelf 文档](http://topshelf.readthedocs.org/en/latest/index.html).
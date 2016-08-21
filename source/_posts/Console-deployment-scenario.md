---
title: 【Akka.NET 文档】控制台程序部署场景
date: 2016-08-21 23:10:50
categories: 翻译
tags: Akka.NET
copyright: false
description: 
---
<!--more-->
## 控制台程序

```PM
PM> install-package Akka
PM> install-package Akka.Remote
```

```cs
using Akka;
using Akka.Actor;
using Akka.Configuration;

namespace Foo.Bar
{
    class Program
    {
        static void Main(string[] args)
        {
            //配置远程路径为 localhost:8081
            var fluentConfig = FluentConfig.Begin()
                .StartRemotingOn("localhost", 8081)
                .Build();

            using (var system = ActorSystem.Create("my-actor-server", fluentConfig))
            {
                //实例化两个服务
                var service1= system.ActorOf<Service1>("service1");
                var service2 = system.ActorOf<Service2>("service2");
                Console.ReadKey();
            }
        }
    }
}
```
---
title: 【Akka.NET 文档】入门
date: 2016-08-20 08:02:42
categories: 翻译
tags: Akka.NET
copyright: false
---
<!--more-->
## 开始使用Akka.NET
本教程旨在介绍如何使用Akka.NET 创建一个简单的问候Actor ，使用C# 语言。

## 项目设置

启动visual studio 并新建一个 C# 控制台程序.
建好后，打开包管理器控制台（Package Manager Console），键入:

```PM
PM> Install-Package Akka
```

然后我们需要添加相关语句:

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

//增加两行
using Akka;
using Akka.Actor;

namespace ConsoleApplication11
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## 创建第一个actor

首先，我们要创建消息类型（Greet 类）以使Actor 去响应：

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

using Akka;
using Akka.Actor;

namespace ConsoleApplication11
{
    // 创建一个（不可变的）消息类型，您的Actor 将响应
    public class Greet
    {
        public Greet(string who)
        {
            Who = who;
        }
        public string Who { get;private set; }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

建好后，开始建actor：
```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

using Akka;
using Akka.Actor;

namespace ConsoleApplication11
{
    public class Greet
    {
        public Greet(string who)
        {
            Who = who;
        }
        public string Who { get;private set; }
    }

    // 创建 actor 类
    public class GreetingActor : ReceiveActor
    {
        public GreetingActor()
        {
            // 告诉Actor 回应问候消息
            Receive<Greet>(greet =>
               Console.WriteLine("Hello {0}", greet.Who));
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

现在，是时候消费Actor了， 我们通过创建 `ActorSystem` 实例和使用 `ActorOf` 方法来实现：
```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

using Akka;
using Akka.Actor;

namespace ConsoleApplication11
{
    public class Greet
    {
        public Greet(string who)
        {
            Who = who;
        }
        public string Who { get;private set; }
    }

    public class GreetingActor : ReceiveActor
    {
        public GreetingActor()
        {
            Receive<Greet>(greet =>
               Console.WriteLine("Hello {0}", greet.Who));
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // 实例化一个 actor 系统 (actors 的容器)
            var system = ActorSystem.Create("MySystem");

            // 创建你的actor 并得到对它的引用。
            // 这将返回 "ActorRef" 类型, 不是指
            // 实际的Actor 实例而是Actor 的客户端或代理。
            var greeter = system.ActorOf<GreetingActor>("greeter");

            // 发送消息到 actor
            greeter.Tell(new Greet("World"));

            // 防止程序在异步工作完成前退出
            Console.ReadLine();
        }
    }
}
```

就是这样，你的Actor 现在已经准备好接收从任意调用线程发送的消息了。

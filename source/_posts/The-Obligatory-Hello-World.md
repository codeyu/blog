---
title: 【Akka.NET 文档】约定成俗的Hello World
date: 2016-08-20 10:44:56
categories: 翻译
tags: Akka.NET
copyright: false
---
下面的例子演示怎么在C# 和F# 语言中定义和使用actors

## C# 语言的Hello World
#### 定义一个消息：
```csharp
// Create an (immutable) message type that your actor will respond to
public class Greet
{
    public Greet(string who)
    {
        Who = who;
    }
    public string Who { get;private set; }
}
```
#### 通过使用 `ReceiveActor` API 定义你的actor
```csharp
// Create the actor class
public class GreetingActor : ReceiveActor
{
    public GreetingActor()
    {
        Receive<Greet>(greet => Console.WriteLine("Hello {0}", greet.Who));
    }
}
```

#### ..或者使用 `TypedActor` API
```csharp
public class GreetingActor : TypedActor , IHandle<Greet>
{
    public void Handle(Greet greet)
    {
        Console.WriteLine("Hello {0}!", greet.Who);
    }
}
```


#### 用法:
```csharp
// Create a new actor system (a container for your actors)
var system = ActorSystem.Create("MySystem");

// Create your actor and get a reference to it.
// This will be an "IActorRef", which is not a reference to the actual actor
// instance but rather a client or proxy to it.
var greeter = system.ActorOf<GreetingActor>("greeter");

// Send a message to the actor.
greeter.Tell(new Greet("World"));

// This prevents the app from exiting
// before the async work is done.
Console.ReadLine();
```
参考:
- [Untyped actors](http://api.getakka.net/docs/stable/html/6300028C.htm)
- [Typed actors](http://api.getakka.net/docs/stable/html/DCCA8182.htm)

## F# 语言的Hello World

```fsharp
// Create an (immutable) message type that your actor will respond to
type Greet = Greet of string

let system = ActorSystem.Create "MySystem"

// Use F# computation expression with tail-recursive loop
// to create an actor message handler and return a reference
let greeter = spawn system "greeter" <| fun mailbox ->
    let rec loop() = actor {
        let! msg = mailbox.Receive()
        match msg with
        | Greet who -> printf "Hello, %s!\n" who
        return! loop() }
    loop()

greeter <! Greet "World"
```

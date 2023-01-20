---
layout: post
title: 我在 .net 中使用一个有点扭曲的依赖注入配置方式
date: 2022-01-26
categories: [使用方法, Web 端开发]
description: 不要 new，要依赖注入
tags: [backend, .net, Dependency-Injection]
---

## 碎碎念

其实我花了好多时间才搞明白，依赖注入到底是个啥。最开始对它的印象就是，哦，它可以不用 new。额，然后呢？为啥不要 new？

某天睡不着觉之后去 Autofac 的文档，突然才看明白，原来依赖注入的灵魂其实是抽象依赖，这样就可以在不修改代码的情况下通过更换依赖的实现而修改逻辑了。

这个做法有一点像换显卡。PCI-E 就是那个抽象的依赖，系统和主板不管你插到上面的是个 3090 还是 610，只要你符合接口的协议，插上去的显卡就能正常工作（当然还包括驱动和供电之类的）。

但是我其实没有好好用过任何一个依赖注入框架。我自己工作的 Qt/C++很少有人提到这个东西。虽然我有在做类似的事情，但是都是手动注入，不是用什么框架。

头一次用依赖注入框架是大学做毕业设计的时候，写一个管理系统后端时用了注入框架，但是当时其实是照猫画虎，复制粘贴修改代码来的。我自己并不理解为什么代码要这么写。

断断续续思考这一概念两年多了，参照了很多框架的示例代码，也看了很多开源项目的做法，终于糊（抄）出了一个自己相对满意的依赖注入使用方式，于是打算稍微记录一下，告诉多年以后的自己，当年想的东西就是这些，是不是很傻（逃

## 概括

这个使用方式我不知道和注入框架有没有关系，但是我看到和我使用相同方式的项目从 Autofac 切换到了 DryIoc，我自己用微软自己的 DI 框架也做了。思想应该是通用的，因为我觉得更多地是在使用 C# 语言和 .net 本身的功能，而不是注入框架的功能。

这个方式想要达到的效果就是，不需要每增加一个依赖，就去 AddScoped 一次。以及不需要为每一个 Assembly 都写一遍注入。写逻辑的时候，只需要新建依赖，标注，然后写实现，需要依赖的地方只需要写出自己需要的依赖就可以用了。

## 基础

这个使用方式需要生成一个 IHost，依赖都跑在里面。理论上来说有不需要 Host 的写法，但是我今天还是先记下需要 Host 的做法。

首先是使用 C# 的 [Attribute](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/attributes/) 类来创建一个标记依赖的 Attribute。这个 Attribute 用来描述在添加依赖的时候，你想要如何添加这个依赖，代码如下：

```c#
    [AttributeUsage(AttributeTargets.Class, AllowMultiple = true, Inherited = false)]
    public sealed class ExportAttribute : Attribute
    {
        public Type? ContractType { get; }
        public bool SingleInstance { get; set; } = true;
        public bool LazyCreate { get; set; } = true;
    }
```

`SingleInstance` 和 `LazyCreate` 就是字面意思，不作解释。`ContractType` 则可以用来说明此依赖的接口是哪一个，在注入的时候就可以这么写：

```c#
[Export(ContractType:typeof(IService), SingleInstance = false, LazyCreate = true)]
public class ServiceImplA:IService
{
    //Iservice impl
}

type = typeof(ServiceImplA);
exported = type.GetCustomAttributes();
if (exported is ExportAttribute)
{
    service.AddScoped(exported.ContractType ?? type, type);
    // service.AddScoped(IService, ServiceImplA)
}
```

抽象依赖的注入，上面提到过，并且不止这一种，我也还在学习中。最近发现的另一个好处就是，获取依赖的时候，可以获取同一个接口的所有不同实现的实例，在写消息处理的时候非常有用。

## 避免重复工作

上面是我们对单个依赖的处理。但是我不希望每加一个依赖，我就要写一遍 AddScoped，而且如果是手写，其实也不需要 Attribute。如何实现增加的依赖都能自动注入呢？

首先，你需要读取配置文件，文件中写明了哪些 Assembly 是你需要的依赖。当然你也可以不用配置文件，直接硬编码。

然后，写一个三层循环，外层循环遍历所有 Assembly，中层循环遍历每个 Assembly 里的每个类型，内层循环遍历每个类型中的所有自定义 Attribute，大概这个样子：

```c#
foreach (var assembly in Assemblies)
    foreach (var type in assembly.DefinedTypes)
    {
        if (type.IsAbstract)
            continue;

        foreach (var attribute in type.GetCustomAttributes())
        {
            switch (attribute)
            {
                case ExportAttribute exported: // 其他的 CustomAttribute 就不需要在这里处理了，过滤一下
                    if (exported.SingleInstance)
                    {
                        services.AddSingleton(exported.ContractType ?? type, type);
                    }
                    else
                    {
                        services.AddScoped(exported.ContractType ?? type, type);
                    }
                    break;
                default:
                    break;
            }
        }
    }
```

这个 switch 里还可以做更多的限定，因为注入的方式不止这两种，当然，道理是一样的。这么写了之后，所有带有 ExportAttribute 的类型，都可以被注入了。增加 Assembly 或者 Type 都不需要重复已经做过的工作，只需要标注好你想要如何被注入即可。

把上面的代码整理一下，丢到一个 Bootstrap namespace 下，在启动的时候先跑这里的代码，整个注入的工作就完成了。

## 依赖的使用（快来注入我！）

大多数的注入框架都有直接请求需要的依赖的方法。但是其实用得最多的还是通过构造方法来注入。这一点没什么好说的，写构造就行。对 MsDI 来说，所有可以 GetRequiredService 的依赖，应该是都可以在构造方法中进行注入的。

比如说前面提到的，获取某个接口的所有实现，可以通过 IServerProvider 的 GetServices 方法返回一个 IEnumerable，也可以通过在构造方法中定义一个 IEnumerable 的参数，让注入框架注入所有这个接口的实现，像这样：

```c#
public User(IEnumerable<IService> services)
{
    foreach (var service in services)
    {
        service.DoSomething();
    }
}
```

## 总结

目前我对于依赖注入的理解和使用就这些，因为太蠢了所以其实花了好长的时间才搞懂怎么使用。其实这样是比较扭曲的，我在 Bootstrap 里还是写了几个 new，来读取配置文件之类的东西。对于第三方依赖，其实也应该有类似的方式来注入，比方说用一个空的接口继承第三方的接口之类的。但是我现在还是直接在 Bootstrap 里写第三方依赖的接口注入。

我参考的项目最多的是 [ING](https://github.com/amatukaze/ing)，一个砍口垒的浏览器。在学习 .net 方面这个项目可以算是我的启蒙项目了，非常感谢项目的作者羽毛和蜜瓜～（所以 2.0 什么时候出）

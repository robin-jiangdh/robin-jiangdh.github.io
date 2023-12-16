---
title: ASP.NET CORE 管道模型和中间件
Published: 12/22/2019
tags: ['AspNetCore','netcore','middleware','pipeline']
---

## ASP.NET 管道模型

请求进入ASP.NET 工作进程后，由进程创建HttpWorkRequest 对象，封装此次请求有关的所有信息，然后进入HttpRuntime
类进行进一步的处理。HttpRuntime 通过请求信息创建HttpContext
上下文对象，此对象将贯穿整个管道，直到响应结束。同时创建或从应用程序池里初始化一个HttpApplication对象，由此对象开始处理之前注册的多个HttpModule。之后调用HandlerFactory
创建Handler处理程序，最终处理此次请求内容，生存响应返回。
![ASP.NET 管道模型.png](https://blog.robinjiang.com/posts/asset/2019-12-22-aspnetcore-middleware-and-pipeline/aspnet-pipeline.jpg)

管道架构异常复杂，同时不具备热插拔的功能。由于这套流程相对固定，对于特定的业务场景优化难度加大。

## aspnetcore下的管道模型

![request-pipeline](https://blog.robinjiang.com/posts/asset/2019-12-22-aspnetcore-middleware-and-pipeline/aspnetcore-request-pipeline.png)

回归web的本质，就是请求与响应而已。
中间件本质上就是一个类型为 Func<RequestDelegate, RequestDelegate> 的委托对象；
简单来说，中间件就是一个处理http请求和响应的组件，多个中间件构成了请求处理管道，每个中间件都可以选择处理结束，还是继续传递给管道中的下一个中间件，以此串联形成请求管道。

实际编码实现上，提供了一个Imiddleware的接口定义

```
public interface IMiddleware
{
    Task InvokeAsync(HttpContext context, RequestDelegate next);
}
```

### UseMiddleware

使用UseMiddleware扩展方法使用IMiddleware时，需要注意你的IMiddleware中间件需要注册到DI中

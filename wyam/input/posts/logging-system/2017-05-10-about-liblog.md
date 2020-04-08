---
title: 抽象日志组件
Published: 05/10/2017
tags: ['LibLog','LoggingSystem','Logging'] 
---

> 本篇是基于[Liblog](https://github.com/damianh/LibLog) 的结构分析，同时我提倡在非netcore项目中使用此类库，正常情况下只需要在基础组件库中生成单个文件即可，同时一次安装，终身有效。

日志核心功能以接口的方式进行定义，简单有效：

```
 interface ILog{
    bool Log(LogLevel logLevel, Func<string> messageFunc, Exception exception = null,
            params object[] formatParameters);
 }
```

Liblog的核心是使用提供者模式(Provider Pattern)来实现对各种具体日志实现支持的。调用方面通过 LogProvider.GetCurrentClassLogger() 或者 LogProvider.For() 来获取Ilog的实例。其中GetCurrentClassLogger() 适用与所有静态类或者扩展方法，For()主要用于可实例化的内部。

```
private static readonly ILog logger = LogProvider.GetCurrentClassLogger()
 
private static readonly ILog Logger = LogProvider.For()
```



> ### 两个小问题
>
> - 日志缓存策略，即生成的日志在本地存在buffer中，后续一次性提交，或者出现了整个日志内容过大，导致无法一次性提交的情况；那么Logproverider.For() 获取的日志实例会出现无法释放的问题，最后只能通过强制GC来释放。解决方案，尽可能控制你的日志文本大小。
>
> - LogProvider.GetCurrentClassLogger()并非是一个线程安全方法，如果需要在Parallel和Task中使用的话，一定要在外部获取实例后传递进去；
>
> 关于多线程下的日志实践，这个问题比较头大，会单独开一章来描述我的方案
>
> 


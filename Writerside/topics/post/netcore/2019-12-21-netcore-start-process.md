---
title: AspNetCore启动流程概述
Published: 12/21/2019
tags: ['AspNetCore','netcore','webhost','Ihost']
---

aspnetcore 应用程序，是一个拥有内置的Self-hosted的WebServer,用于处理外部请求。大部分情况下以控制台应用的方式实现子托管（Self-Hosted）。
相对于传统的aspnet 提供的大而全的设计，aspnetcore以最小化抽象设计为原则，以扩展(中间件)方式提供的易用性扩展来实现更高程度的按需使用。
在ASP.NET Core应用中通过配置并启动一个Host来完成应用程序的启动和其生命周期的管理。而Host的主要的职责就是Web
Server的配置和Pilpeline（请求处理管道）的构建。
在应用中主要通过 构造一个IHostBuilder 的抽象对象来负责(通过build()方法)创建一个Ihost实例。并最终通过Ihost下的run()
方法来启动这个实例。

```
IHostBuilder > IHost>Run()
```

期间设计两个核心的概念IHostbuilder和Ihost

# IHostBuilder

可以理解为为了启动host而准备的一些配置文件或行为，提供默认注册行为CreateDefaultBuilder和相关扩展ConfigureWebHostDefaults，以及其他一些配置入口。

### CreateDefaultBuilder相关

大部分都是配置系统相关的api

- UseContentRoot
- ConfigureHostConfiguration
- ConfigureAppConfiguration
- ConfigureLogging
- UseDefaultServiceProvider

### ConfigureWebHostDefaults

配置webserver的默认行为

- UseStaticWebAssets
- UseKestrel/UseIIS
- ConfigureServices 服务中间件的注册，包括路由中间件
- UseStartup 程序Startup 启动，该启动类中可以注册中间件、扩展第三方中间件，以及相关应用程序配置的处理等

# Build()

核心要素

- BuildHostConfiguration();->围绕ConfigureHostConfiguration的一系列配置回调->执行ConfigureHostConfiguration
- CreateHostingEnvironment();-> HostingEnvironment环境相关配置
- CreateHostBuilderContext();-> HostBuilder 上下文构建，这个略复杂
- BuildAppConfiguration();->围绕ConfigureAppConfiguration的一系列回调->执行ConfigureAppConfiguration；
- CreateServiceProvider();>服务注册添加，并执行ConfigureServices添加的一系列回调->执行ConfigureServices；
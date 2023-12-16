---
title: 生成环境的若干日志实践
Published: 12/20/2018
tags: ['LibLog','LoggingSystem','Logging'] 
---

- 只记录你需要的日志数据；
- 关于日志数据数据中的敏感信息记录问题，账户名可以保留，但银行卡/密码等敏感信息，必须要在源头过滤掉，严禁在日志生成后使用文本处理的方式处理这些字段。**推荐的方式是使用日志模板+关键字过滤的方式来在开发阶段就将这些不规范的日志暴露出来**。
- 生产环境的日志应该需要包含你的业务细节，而不是流水线日志。如果你的开发框架支持AOP拦截日志的话，先确定你知道如何在生产环境中关闭这些日志，大部分情况下，AOP日志有且仅用于开发环境的debug，生产环境如果用这种方式记录日志的话，那么大部分业务日志都会被这类日志冲垮；
- 除了常见的日志级别外，日志系统需要支持简单模式和完整模式的日志；
- 日志要尽可能做到不干扰业务正常允许，但允许例外，特定情况下，日志可能是你最后的灾备恢复手段。
- 记录日志是一个高IO的操作，条件允许的情况下，不要干扰主业务的io操作。
- 善用日志模板，提高日志可读性以及降低日志大小
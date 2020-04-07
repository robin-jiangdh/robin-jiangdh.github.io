---
title: GELF日志格式漫谈
Published: 06/12/2017
tags: ['GELF ','GrayLog','LoggingSystem','Logging'] 
---




## 什么是GELF

Graylog扩展日志格式（GELF）是一种日志格式。

> ## Structured events from anywhere. Compressed and chunked.
> The Graylog Extended Log Format (GELF) is a log format that avoids the shortcomings of classic plain syslog:
> 
> - Limited to length of 1024 bytes – Not much space for payloads like backtraces
> - No data types in structured syslog. You don’t know what is a number and what is a string.
> - The RFCs are strict enough but there are so many syslog dialects out there that you cannot possibly parse all of them.
> - No compression

[GELF 规范](http://docs.graylog.org/en/2.4/pages/gelf.html#gelf-payload-specification)

> A GELF message is a JSON string with the following fields: 
> -  **version** `string (UTF-8)`
>
>     GELF spec version – “1.1”; **MUST** be set by client library.
>
> - **host** `string (UTF-8)`
>
>     the name of the host, source or application that sent this message; **MUST** be set by client library.
>
> -  **short_message** `string (UTF-8)`
>
>     a short descriptive message; **MUST** be set by client library.
>
> -  **full_message** `string (UTF-8)`
>
>     a long message that can i.e. contain a backtrace; optional.
>
> -  **timestamp** `number`
>
>     Seconds since UNIX epoch with optional decimal places for milliseconds; *SHOULD* be set by client library. Will be set to the current timestamp (now) by the server if absent.
>
> -  **level** `number`
>
>     the level equal to the standard syslog levels; optional, default is 1 (ALERT).
>
> - **facility** `string (UTF-8)`
>
>     optional, deprecated. Send as additional field instead.
>
> -  **line** `number`
>
>     the line in a file that caused the error (decimal); optional, deprecated. Send as additional field instead.
>
> -  **file** `string (UTF-8)`
>
>     the file (with path if you want) that caused the error (string); optional, deprecated. Send as additional field instead.
>
> -  **_[additional field]** `string (UTF-8)` or `number`
>
>     every field you send and prefix with an underscore (`_`) will be treated as an additional field. Allowed characters in field names are any word character (letter, number, underscore), dashes and dots. The verifying regular expression is: `^[\w\.\-]*$`. Libraries SHOULD not allow to send id as additional field (`_id`). Graylog server nodes omit this field automatically。

## GELF 能解决什么问题
- 这是个格式规范，能够让绝大部分日志格式相对统一合理，提高日志可读性。
- 这是个json规范，实现比较简单。
- 由于无视源文本类型，对于绝大部分日志工具类能很好支持
## 实践中，GELF 存在哪些问题
- 这是一个非强制性约束，也就意味着，你需要根据现有业务增加更多的约束条件，比如异常堆栈等标准信息；

- 连续日志的追踪需要增加更高级别的支持，类似CorrelationID；

- 关于敏感信息过滤问题，假设要实现用户密码字段的过滤，只能通过正则处理生成后的日志文本来过滤，无法前置过滤；

- 日志模板问题，大部分日志是依据格式化的模板生成的，类似“{timestamp}-{controllername}:{toekn}-{postdata}”；日志模板能够极大的降低传输的数据量，很遗憾，这套规范中很难去扩展实现这种模板支持；

- 对于Serilog这类提供更多日志信息的库，容易丢失支持的内容



## GELF+Serilog+template实践

 repo近期会迁入github


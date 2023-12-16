---
title: MySQL 执行计划
Published: 01/13/2016
tags: ['mysql','db','执行计划']
---

> 一直在使用Explain，今天被扫盲，专业名词叫执行计划~，野生码农误国啊~
>
> [官方文档地址](https://dev.mysql.com/doc/refman/5.7/en/execution-plan-information.html)

MySQL提供[**explain/desc**](https://dev.mysql.com/doc/refman/5.7/en/execution-plan-information.html)
命令输出执行计划，我们通过执行计划优化SQL语句

> 根据表，列，索引的详细信息以及`WHERE`
> 子句中的条件，MySQL优化器考虑了许多技术来有效执行SQL查询中涉及的查找。无需读取所有行即可执行对巨大表的查询；可以执行涉及多个表的联接，而无需比较行的每个组合。优化器选择执行最有效查询的一组操作称为“
> 查询执行计划 ”，也称为 [`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)
> 计划。您的目标是认识到 [`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)
> 表示查询优化的计划，如果发现一些低效的操作，则可以学习SQL语法和索引技术来改进计划。

# 使用Explain优化查询

该[`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)语句提供有关MySQL如何执行语句的信息：

- [`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)
  作品有 [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html)， [`DELETE`](https://dev.mysql.com/doc/refman/5.7/en/delete.html)， [`INSERT`](https://dev.mysql.com/doc/refman/5.7/en/insert.html)， [`REPLACE`](https://dev.mysql.com/doc/refman/5.7/en/replace.html)
  ，和 [`UPDATE`](https://dev.mysql.com/doc/refman/5.7/en/update.html)语句。
- 当[`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)
  与可解释的语句一起使用时，MySQL将显示来自优化器的有关语句执行计划的信息。也就是说，MySQL解释了它将如何处理该语句，包括有关如何连接表以及以何种顺序连接表的信息。
- 当[`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)与 `FOR CONNECTION *`connection_id`*`
  而不是可解释的语句一起使用时，它将显示在名为这个“connetction_id”的连接中正在执行的语句的执行计划。
- 对于[`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html)语句，
  在[`SHOW WARNINGS`](https://dev.mysql.com/doc/refman/5.7/en/show-warnings.html)
  模式下，[`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)可以输出更多的执行计划信息。
- [`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)对于检查涉及分区表的查询很有用。
- `FORMAT`选项可用于设置输出格式。`TRADITIONAL`以表格格式显示输出。默认情况下不需要提供`FORMAT`设置 。 format设置为`JSON`
  时，以JSON格式显示信息。

# Explain输出格式 {id="explain_1"}

[`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)
语句提供有关MySQL如何执行语句的信息。 [`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)
支持 [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html)， [`DELETE`](https://dev.mysql.com/doc/refman/5.7/en/delete.html)， [`INSERT`](https://dev.mysql.com/doc/refman/5.7/en/insert.html)， [`REPLACE`](https://dev.mysql.com/doc/refman/5.7/en/replace.html)
和 [`UPDATE`](https://dev.mysql.com/doc/refman/5.7/en/update.html)语句。

[`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)
为[`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html)语句中使用的每个表返回一行信息
。它按照MySQL在处理语句时读取它们的顺序列出了输出中的表。MySQL使用嵌套循环连接方法解析所有连接。这意味着MySQL从第一个表中读取一行，然后在第二个表，第三个表中找到匹配的行，依此类推。处理完所有表后，MySQL将通过表列表输出选定的列和回溯，直到找到一个表，其中存在更多匹配的行。从该表中读取下一行，然后继续下一个表。

[`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)
输出包括分区信息。此外，对于[`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html)
语句，[`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)
产生可与被显示扩展信息 [`SHOW WARNINGS`](https://dev.mysql.com/doc/refman/5.7/en/show-warnings.html)
之后的 [`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)

### 扩展的EXPLAIN输出格式

对于[`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html)
语句，该 [`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)语句会产生额外的（“ 扩展
”）信息，这些信息不属于 [`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)
输出，但可以通过在[`SHOW WARNINGS`](https://dev.mysql.com/doc/refman/5.7/en/show-warnings.html)
以下语句发出来查看[`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)。输出中的 `Message`
值[`SHOW WARNINGS`](https://dev.mysql.com/doc/refman/5.7/en/show-warnings.html)
显示优化器如何限定[`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html)语句
中的表名和列名， [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html)应用重写和优化规则后的外观以及有关优化过程的其他注释。

可在[`SHOW WARNINGS`](https://dev.mysql.com/doc/refman/5.7/en/show-warnings.html)语句后面
显示的扩展信息 [`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)
仅针对 [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html)
语句生成。 [`SHOW WARNINGS`](https://dev.mysql.com/doc/refman/5.7/en/show-warnings.html)
显示其他可解释语句空结果（[`DELETE`](https://dev.mysql.com/doc/refman/5.7/en/delete.html)， [`INSERT`](https://dev.mysql.com/doc/refman/5.7/en/insert.html)， [`REPLACE`](https://dev.mysql.com/doc/refman/5.7/en/replace.html)
，和 [`UPDATE`](https://dev.mysql.com/doc/refman/5.7/en/update.html)）。

# 获取命名连接的执行计划信息

```
EXPLAIN [options] FOR CONNECTION connection_id;
```

[`EXPLAIN FOR CONNECTION`](https://dev.mysql.com/doc/refman/5.7/en/explain-for-connection.html)
返回[`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)
当前在给定连接中用于执行查询的信息。由于数据（和支持统计数据）的更改，它可能会产生与[`EXPLAIN`](https://dev.mysql.com/doc/refman/5.7/en/explain.html)
在等效查询文本上运行不同的结果
。行为上的这种差异对于诊断更多瞬时性能问题很有用。例如，如果您在一个需要很长时间才能完成的会话中运行语句，则[`EXPLAIN FOR CONNECTION`](https://dev.mysql.com/doc/refman/5.7/en/explain-for-connection.html)
在另一会话中使用该语句可能会产生有关延迟原因的有用信息。

*`connection_id`*
是从`INFORMATION_SCHEMA` [`PROCESSLIST`](https://dev.mysql.com/doc/refman/5.7/en/processlist-table.html)
表或 [`SHOW PROCESSLIST`](https://dev.mysql.com/doc/refman/5.7/en/show-processlist.html)语句获得的连接标识符
。如果有[`PROCESS`](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_process)
特权，则可以为任何连接指定标识符。否则，您只能为自己的连接指定标识符。

如果命名连接未执行语句，则结果为空。否则，`EXPLAIN FOR CONNECTION`
仅当在命名连接中执行的语句是可解释的时才适用。这包括 [`SELECT`](https://dev.mysql.com/doc/refman/5.7/en/select.html)， [`DELETE`](https://dev.mysql.com/doc/refman/5.7/en/delete.html)， [`INSERT`](https://dev.mysql.com/doc/refman/5.7/en/insert.html)， [`REPLACE`](https://dev.mysql.com/doc/refman/5.7/en/replace.html)
，和 [`UPDATE`](https://dev.mysql.com/doc/refman/5.7/en/update.html)。（但是， `EXPLAIN FOR CONNECTION`
不适用于预备语句，甚至不适用于这些类型的预备语句。）

# 估计查询性能

在大多数情况下，您可以通过计算磁盘搜索次数来估计查询性能。对于小型表，通常可以在一个磁盘搜索中找到一行（因为索引可能已缓存）。对于更大的表，您可以估计，使用B树索引，您需要进行许多查找才能找到行： 。 `log(*`
row_count`*) / log(*`index_block_length`* / 3 * 2 / (*`index_length`* + *`data_pointer_length`*)) + 1`

在MySQL中，索引块通常为1,024字节，数据指针通常为四个字节。对于一个键值长度为三个字节（大小为[`MEDIUMINT`](https://dev.mysql.com/doc/refman/5.7/en/integer-types.html)
）的500,000行表 ，该公式表示 `log(500,000)/log(1024/3*2/(3+4)) + 1`=搜索 `4`。

该索引将需要大约500,000 * 7 * 3/2 = 5.2MB的存储空间（假设典型的索引缓冲区填充率为2/3），因此您可能在内存中拥有很多索引，因此只需要一个或两个调用即可读取数据以查找行。

但是，对于写操作，您需要四个搜索请求来查找在何处放置新索引值，通常需要两个搜索来更新索引并写入行。

前面的讨论并不意味着您的应用程序性能会因log缓慢下降 *`N`*
。只要所有内容都由OS或MySQL服务器缓存，随着表的增加，事情只会变得稍微慢一些。在数据变得太大而无法缓存之后，事情开始变得缓慢得多，直到您的应用程序仅受磁盘搜索约束（随日志增长
*`N`*）。为避免这种情况，请随着数据的增长而增加密钥缓存的大小。对于`MyISAM`
表，键缓存大小由[`key_buffer_size`](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_key_buffer_size)
系统变量控制 
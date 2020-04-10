---
title: MySQL索引原理
Published: 01/13/2016
tags: ['mysql','db','mysql索引'] 
---

- 基本概念
  	- 索引的功能就是加速查找
  	- mysql中的primary key，unique，联合唯一也都是索引，这些索引除了加速查找以外，还有约束的功能
- 常用索引
  - 普通索引INDEX：加速查找
  - 唯一索引：
    - 主键索引PRIMARY KEY：加速查找+约束（不为空、不能重复）
    - 唯一索引UNIQUE:加速查找+约束（不能重复）
  - 联合索引：
    - PRIMARY KEY(id,name):联合主键索引
    - UNIQUE(id,name):联合唯一索引
    - INDEX(id,name):联合普通索引
- 索引的两大类型hash与btree
     - 我们可以在创建上述索引的时候，为其指定索引类型，分两类： 
        -  hash类型的索引：查询单条快，范围查询慢
        -  btree类型的索引：b+树，层数越多，数据量指数级增长（我们就用它，因为innodb默认支持它） 
     
     - 不同的存储引擎支持的索引类型也不一样： 
        -  InnoDB 支持事务，支持行级别锁定，支持 B-tree、Full-text 等索引，不支持 Hash 索引；
        -  MyISAM 不支持事务，支持表级别锁定，支持 B-tree、Full-text 等索引，不支持 Hash 索引；
        -  Memory 不支持事务，支持表级别锁定，支持 B-tree、Hash 等索引，不支持 Full-text 索引；
        -  NDB 支持事务，支持行级别锁定，支持 Hash 索引，不支持 B-tree、Full-text 等索引；
        -  Archive 不支持事务，支持表级别锁定，不支持 B-tree、Hash、Full-text 等索引；
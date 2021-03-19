---
title: "Mysql使用explain/desc"
date: 2021-03-16T15:22:06+08:00
draft: false
---
## 1. 用法
select语句前加上explain或desc

## 2.explain/desc 各字段含义

- select_type: SELECT 查询的类型.

- table：查询的数据表

- type：这是重要的列，显示连接使用了何种类型。从最好到最差的连接类型为const、eq_reg、ref、range、index和all。

- possible_keys：显示可能应用在这张表中的索引。如果为空，没有可能的索引。可以为相关的域从where语句中选择一个合适的语句

- key： 实际使用的索引。如果为null，则没有使用索引。很少的情况下，mysql会选择优化不足的索引。这种情况下，可以在select语句中使用use index（indexname）来强制使用一个索引或者用ignore index（indexname）来强制mysql忽略索引

- key_len：使用的索引的长度。在不损失精确性的情况下，长度越短越好

- ref：显示索引的哪一列被使用了，如果可能的话，是一个常数

- rows：mysql认为必须检查的用来返回请求数据的行数，mysql完成此请求扫描的行数。

- extra：关于mysql如何解析查询的额外信息。将在表4.3中讨论，但这里可以看到的坏的例子是using temporary和using filesort，意思mysql根本不能使用索引，结果是检索会很慢。

## 3. select_type 表示了查询的类型, 它的常用取值有:

- SIMPLE, 表示此查询不包含 UNION 查询或子查询

- PRIMARY, 表示此查询是最外层的查询

- UNION, 表示此查询是 UNION 的第二或随后的查询

- DEPENDENT UNION, UNION 中的第二个或后面的查询语句, 取决于外面的查询

- UNION RESULT, UNION 的结果

- SUBQUERY, 子查询中的第一个 SELECT

- DEPENDENT SUBQUERY: 子查询中的第一个 SELECT, 取决于外面的查询. 即子查询依赖于外层查询的结果.

## 4. type中各个值解释
> ALL < index < range ~ index_merge < ref < eq_ref < const < system

- ALL：全表扫描，MySQL将遍历全表以找到匹配的行

- INDEX：与ALL区别为INDEX类型只遍历索引树

- RANGE：索引范围扫描，对索引的扫描开始于某一点，返回匹配值域的行。显而易见的索引范围扫描是带有between或者where子句里带有<, >查询。当mysql使用索引去查找一系列值时，例如IN()和OR列表，也会显示range（范围扫描）,当然性能上面是有差异的。

- REF: 使用非唯一索引扫描或者唯一索引的前缀扫描，返回匹配某个单独值的记录行。

- eq_ref，类似ref，区别就在使用的索引是唯一索引，对于每个索引键值，表中只有一条记录匹配，简单来说，就是多表连接中使用primary key或者 unique key作为关联条件。

- const，system: 当MySQL对查询某部分进行优化，并转换为一个常量时，使用这些类型访问。如将主键置于where列表中，MySQL就能将该查询转换为一个常量。system是const类型的特例，当查询的表只有一行的情况下，使用system。
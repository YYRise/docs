---
title: "MySq索引"
date: 2021-01-04T09:49:38+08:00
draft: false
---

## 1. Index Key
- 用来确定扫描的数据范围; 分为上边界和下边界
- 含有 `=` 包括（`<=、 >=`），能使用该索引键，继续匹配后面的索引键
- `<、 >` 时不能匹配索引键值

### 1.1 上边界(last key)
- 通过 `=, <=, <` 来确定

```mysql
索引：idx_c1_c2_c3(c1,c2,c3)
where条件为： c1<=1 and c2=2 and c3<3

--> last key (c1,c2,c3)
--> c1为 '<='，加入上边界界定，继续匹配下一个
--> c2为 '='，加入上边界界定，继续匹配下一个
--> c3为 '<'，加入上边界界定，停止匹配
```
### 1.2 下边界(first key)
- 通过 `=, >=, >` 来确定

- **过程：** 
> 利用最左原则，首先判断第一个索引键值在where条件中是否存在，如果存在，则判断比较符号，如果为(=,>=)中的一种，加入下边界的界定，然后继续判断下一个索引键，如果存在且是(>)，则将该键值加入到下边界的界定，停止匹配下一个索引键；如果不存在，直接停止下边界匹配。

```mysql
索引：idx_c1_c2_c3(c1,c2,c3)
where条件为： c1>=1 and c2>2 and c3=1

--> first key (c1,c2)
--> c1为 '>=' ，加入下边界界定，继续匹配下一个
--> c2为 '>'，加入下边界界定，停止匹配
--> c3无用
```

## 2. Index Filter
- 索引过滤
- 字段在索引键值中，但是无法确定Index Key的部分

```mysql
索引：idx_c1_c2_c3(c1,c2,c3)
where条件为： c1>=1 and c2<=2 and c3 =1

--> index key (c1)
--> index filter(c2, c3)
```
- index key 只是c1的原因：
> c1 只有下边界, 无上边界
> c2 确定上边界而无法确定下边界


## 3. Table Filter
- 表过滤
- 引擎层回表取行数据，然后在server层过滤数据。

[^1]: ICP: Index Condition Pushdown, 是一种在存储引擎层使用索引过滤数据的一种优化方式
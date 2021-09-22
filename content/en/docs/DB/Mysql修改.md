---
title: "Mysql修改"
date: 2021-01-26T11:48:55+08:00
draft: false
---

## 1. 修改字段
### 修改字段类型
` ALTER  TABLE 表名 MODIFY [COLUMN] 字段名 新数据类型 新类型长度  新默认值  新注释; `

### 修改字段名
` ALTER  TABLE 表名 CHANGE [column] 旧字段名 新字段名 新数据类型; `

### 在指定位置插入新字段
` ALTER TABLE 表名 ADD [COLUMN] 字段名 字段类型 是否可为空 COMMENT '注释' AFTER 指定某字段 ; `

### 删除字段
` ALTER TABLE 表名 DROP [COLUMN] 字段名 ; `

## 2. 修改表
### 修改表名
` ALTER TABLE 旧表名 RENAME TO 新表名 ; `
### 修改表的注释
` ALTER TABLE 表名 COMMENT '新注释'; `

## 3. 索引
### 查看索引
``` sql
show index from table_name;
show keys from table_name;
```

### 创建索引 
- 可以用alter也可以用create

``` sql
alter table table_name add index index_name (列名1, 列名2, ……);
alter table table_name add unique (列名1, 列名2, ……);
alter table table_name add primary key (列名1, 列名2, ……);

create index index_name on table_name (列名1, 列名2, ……);
create unique index index_name on table_name (列名1, 列名2, ……);
```
> 1. 索引名index_name可选，缺省时，MySQL将根据第一个索引列赋一个名称。
> 2. ALTER TABLE允许在单个语句中更改多个表，因此可以同时创建多个索引。

### 删除索引
``` sql
drop index index_name on table_name;
alter table table_name drop index index_name;
alter table table_name drop primary key;
```

### 修改索引
- 先删除
` ALTER TABLE table_name DROP INDEX index_name; `

- 再以修改后的内容创建同名索引
` CREATE INDEX index_name ON table_name; `

## 4. 实例 
### 4.1 删除重复数据只保留一条记录
` delete from table_name where id not in (select id from (select min(id) as id from table_name group by `col`) as b); `

### 4.2 查询最近一条记录
- 查询后排序取第一条
`select * from table_name where condition order by create_time desc limit 1`
> * 需要遍历整表
> * limit 查询出全部结果后，取第一条

- `max()` 和 `group by` 结合使用
`select *, max(create_time) from table_name where condition group by id`
> * 一次遍历即可取出结果

## 5. 给用户授权数据库
`grant all privileges on 授权的数据库.* to 'user'@'%';`
`flush privileges;`

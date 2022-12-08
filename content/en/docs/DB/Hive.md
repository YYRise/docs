---
title: "Hive"
date: 2020-12-28T14:07:30+08:00
draft: true
---
## 1 分区
### 1.1 查看分区
```sh
SHOW partitions table_name;
```

### 1.2 添加分区
```sh
ALTER TABLE table_name ADD PARTITION (partCol = 'value1') location 'loc1'; //示例
ALTER TABLE table_name ADD IF NOT EXISTS PARTITION (dt='20130101') LOCATION '/user/hadoop/warehouse/table_name/dt=20130101'; //一次添加一个分区

ALTER TABLE page_view ADD PARTITION (dt='2008-08-08', country='us') location '/path/to/us/part080808' PARTITION (dt='2008-08-09', country='us') location '/path/to/us/part080809';  //一次添加多个分区
```

### 1.3 删除分区
```sh
ALTER TABLE table_name DROP IF EXISTS PARTITION (dt='2008-08-08');
ALTER TABLE page_view DROP IF EXISTS PARTITION (dt='2008-08-08', country='us');
``` 

### 1.4 修改分区
```sh
ALTER TABLE table_name PARTITION (dt='2008-08-08') SET LOCATION "new location";
ALTER TABLE table_name PARTITION (dt='2008-08-08') RENAME TO PARTITION (dt='20080808');
``` 

## 2 数据库

### 2.1 查看当前数据库
```sh
SELECT current_database();
``` 

## 3 表


### 3.1 添加列
```sh
ALTER TABLE table_name ADD COLUMNS (col_name STRING);  //在所有存在的列后面，但是在分区列之前添加一列
``` 

### 3.2 修改列

```sh
CREATE TABLE test_change (a int, b int, c int);

// will change column a's name to a1
ALTER TABLE test_change CHANGE a a1 INT; 

// will change column a's name to a1, a's data type to string, and put it after column b. The new table's structure is: b int, a1 string, c int
ALTER TABLE test_change CHANGE a a1 STRING AFTER b; 

// will change column b's name to b1, and put it as the first column. The new table's structure is: b1 int, a string, c int
ALTER TABLE test_change CHANGE b b1 INT FIRST; 
```
 

### 3.3 修改表属性
```sh
ALTER table table_name set TBLPROPERTIES ('EXTERNAL'='TRUE');  //内部表转外部表 
ALTER table table_name set TBLPROPERTIES ('EXTERNAL'='FALSE');  //外部表转内部表
``` 

### 3.4 表的重命名
```sh
ALTER TABLE table_name RENAME TO new_table_name;
```

### 3.5 建表

#### 3.5.1 外部表(被 `external` 修饰)
1. 外部分区表
```sh
CREATE EXTERNAL TABLE IF NOT EXISTS db_name.table_name(
  id string,
  name string)
PARTITIONED BY (`p_day_id` string)
ROW FORMAT DELIMITED 
  FIELDS TERMINATED BY '\t' 
  LINES TERMINATED BY '\n' 
STORED AS INPUTFORMAT 
  'org.apache.hadoop.mapred.TextInputFormat' 
LOCATION
  'hdfs://namenode/path/to/data/p_day_id=xxxxxxxx';
```
* 加载分区数据
```sh
ALTER TABLE db_name.table_name ADD PARTITION(p_day_id=xxxxxxxx) LOCATION 'hdfs://namenode/path/to/data/p_day_id=xxxxxxxx';
```
#### 3.5.2 内部表(未被 `external` 修饰)
```sh
CREATE EXTERNAL TABLE IF NOT EXISTS db_name.table_name(
  id string,
  name string)
ROW FORMAT DELIMITED 
  FIELDS TERMINATED BY '\t' 
  LINES TERMINATED BY '\n' 
STORED AS INPUTFORMAT 
  'org.apache.hadoop.mapred.TextInputFormat' ;
```
* 加载数据
```sh
# hdfs 数据
load data inpath '{$file_path}' [overwrite] into table {$table_name};
# 本地数据
load data local inpath '{$file_path}' [overwrite] into table {$table_name};
```


#### 3.5.3 内部表和外部表区别：
> 1. 内部表数据由Hive自身管理，外部表数据由HDFS管理；
> 2. 删除内部表会直接删除元数据（metadata）及存储数据；删除外部表仅仅会删除元数据，HDFS上的文件并不会被删除；
> 3. 对内部表的修改会将修改直接同步给元数据，而对外部表的表结构和分区进行修改，则需要修复（`MSCK REPAIR TABLE table_name;`）


## 4 查询语句中显示列名

### 4.1 带表名的列名(table.field)
```sh
set hive.cli.print.header=true;
```
### 4.2 无表名的列名
```sh
set hive.cli.print.header=true;
set hive.resultset.use.unique.column.names=false;
```

## 5 错误

### 5.1 [Fatal Error] total number of created files now is 1000xx, which exceeds 100000.
```sh
set hive.exec.max.created.files=1000000;
```

### 5.2  FAILED: SemanticException Column dt Found in more than One Tables/Subqueries
有一字段在多个表中出现，但是没有在字段前加表名前缀，导致出现错误，在字段前加上表名即可。

## 6 加载数据

### 6.1 加载hdfs数据
#### 6.1.1 hive的LOCATION和hdfs路径一致时，添加分区方式：
```sh
ALTER TABLE
  table_name
ADD IF NOT EXISTS PARTITION
  (dt='2019-11-01', hour='13', dstreamid='123456')
LOCATION
  'hdfs://namenode/path/to/data/2019-11-01/13/123456';
```

#### 6.1.2 不一致时
```sh
load data inpath 'hdfs://namenode/path/to/data/2019-11-01/13/123456' overwrite into table table_name partition(dt='2019-11-01', hour='13', dstreamid='123456');
``` 
> overwrite into: 覆盖hive中的数据; 若追加使用 `into`；

### 6.2 导入本地数据
```sh
load data local inpath '{$file_path}' [overwrite] into table {$table_name};
``` 
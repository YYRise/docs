---
title: "MySql不存在则插入，存在则更新或忽略"
date: 2020-12-29T11:30:13+08:00
draft: true
---

## 1. 不存在则插入，存在则更新
### 1.1 `on duplicate key update`
* 如果插入的数据会导致 `UNIQUE 索引` 或 `PRIMARY KEY` 发生冲突/重复，则执行 `UPDATE` 语句，例：
``` sql
INSERT INTO `student`(`name`, `age`) VALUES('Jack', 19) ON DUPLICATE KEY UPDATE `age`=19;

若插入则：
-- 1 row(s) affected
若更新则：
-- 2 row(s) affected
```

### 1.2 `replace into`
* 如果插入的数据会导致 `UNIQUE 索引` 或 `PRIMARY KEY` 发生冲突/重复，则**先删除旧数据再插入最新的数据**，例：
``` sql
REPLACE INTO `student`(`name`, `age`) VALUES('Jack', 18);

若插入则：
-- 1 row(s) affected
若更新则：
-- 2 row(s) affected
```

## 2. 避免重复插入
* `insert ignore into`
* 如果插入的数据会导致 `UNIQUE 索引` 或 `PRIMARY KEY` 发生冲突/重复，则**忽略此次操作/不插入数据**，例：
``` sql
INSERT IGNORE INTO `student`(`name`, `age`) VALUES('Jack', 18);

若插入则：
-- 1 row(s) affected
若已存在则：
-- 0 row(s) affected
```
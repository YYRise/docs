---
title: "Shell日期"
date: 2020-12-28T13:38:33+08:00
draft: false
---

## 日期循环
### 数字格式
```sh
#! /bin/bash

start=20200101
end=20200103

while [ ${start} -le ${end} ]
do
  echo ${start}
  start=`date -d "1 day ${start}" +%Y%m%d`	# 日期自增
done

```

> * shell 数字比较：
> ```sh
> -ne	# !=
> -gt	# >
> -lt	# <
> -ge	# >=
> -le	# <=
> ```

### 字符串格式

```sh
#! /bin/bash

start='2020-01-01'
end='2020-01-03'

# while [ "${start}" != "${end}" ]
# while [ $start != $end ]
# while [ $start /< $end ]  #需要转义<，否则认为是一个重定向符号
while [[ $start < $end ]]
do
  echo ${start}
  start=`date -d "1 day ${start}" +%Y-%m-%d`	# 日期自增
done

```
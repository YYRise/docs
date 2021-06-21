---
title: "Nginx使用logrotate日志分割"
date: 2021-06-21T16:08:35+08:00
draft: false
---

## 1. 配置文件参数

|参数名称|说明|
| :--- | :--- |
| daily | 指定转储周期为每天 |
| weekly | 指定转储周期为每周 |
| monthly | 指定转储周期为每月 |
| dateext | 使用当期日期作为命名格式，如：access.log-20201121 |
| dateformat .%s |配合dateext使用，紧跟在下一行出现，定义文件切割后的文件名，必须配合dateext使用，只支持 %Y %m %d %s 这四个参数 |
| compress | 通过gzip压缩转储以后的日志 |
| nocompress | 不压缩 |
| copytruncate | 用于还在打开中的日志文件，把当前日志备份并截断 |
| nocopytruncate | 备份日志文件但是不截断 |
| create mode owner group | 转储文件，使用指定的文件模式创建新的日志文件 |
| nocreate | 不建立新的日志文件 |
| delaycompress 和 compress | 一起使用时，转储的日志文件到下一次转储时才压缩 |
| nodelaycompress | 覆盖 delaycompress 选项，转储同时压缩。|
| errors address | 专储时的错误信息发送到指定的Email 地址 |
| ifempty | 即使是空文件也转储，这个是 logrotate 的缺省选项。 |
| missingok | 如果日志丢失，不报错继续滚动下一个日志 |
| notifempty | 如果是空文件的话，不转储 |
| mail address | 把转储的日志文件发送到指定的E-mail 地址 |
| nomail | 转储时不发送日志文件 |
| olddir directory | 转储后的日志文件放入指定的目录，必须和当前日志文件在同一个文件系统 |
| noolddir | 转储后的日志文件和当前日志文件放在同一个目录下 |
| sharedscripts | 运行postrotate脚本，作用是在所有日志都轮转后统一执行一次脚本。如果没有配置这个，那么每个日志轮转后都会执行一次脚本 |
| prerotate/endscript | 在转储以前需要执行的命令可以放入这个对，这两个关键字必须单独成行 |
| rotate count | 指定日志文件删除之前转储的次数，0 指没有备份，5 指保留5 个备份 |
| size log-size | 当日志文件到达指定的大小时才转储，log-size能指定bytes(缺省)及KB (sizek)或MB(sizem)，当日志文件 >= log-size 的时候就转储 |

## 2. logrotate 命令参数

` logrotate [OPTION...] <configfile> `
> -d, --debug ：debug模式，测试配置文件是否有错误。
> -f, --force ：强制转储文件。
> -m, --mail=command ：压缩日志后，发送日志到指定邮箱。
> -s, --state=statefile ：使用指定的状态文件。
> -v, --verbose ：显示转储过程。
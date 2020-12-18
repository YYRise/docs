---
title: "Crontab 定时任务"
date: 2020-12-18T11:05:49+08:00
draft: false
---

<!-- ![img](/images/header/background1.jpg)
{{< img src="/images/header/background1.jpg" title="Sample Image" caption="Image with title, caption, alt, ..." alt="image alt" width="700px" position="center" >}}
-->

## 命令

* 编辑当前用户的crontab `crontab -e`
* 编辑其他用户的crontab `crontab -u username -e`
* 查看当前用户的crontab `crontab -l`
* 查看其他用户的crontab `crontab -u username -l`

## 日志
* 执行的日志文件 
> `/var/log/cron`
* 自定义输出日志
> ```sh
> 0 0 * * * php /home/crontab/cli.php >> /home/crontab/cli.log 2>&1
> ```
> 把错误输出和标准输出都输出到 `cli.log` 中。


## 规则

```sh
*    *    *    *    *
-    -    -    -    -
|    |    |    |    |
|    |    |    |    +----- day of week (0 - 7) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
|    |    |    +---------- month (1 - 12) OR jan,feb,mar,apr ...
|    |    +--------------- day of month (1 - 31)
|    +-------------------- hour (0 - 23)
+------------------------- minute (0 - 59)
```


## 使用string代替

| 字符串 | <center>含义</center> |  |
| :--- | :--- | :--- |
| @reboot |  Run once, at startup. | 系统启动时 |
| @yearly       | Run once a year, | 0 0 1 1 * |
| @annually     |  (same as @yearly)|  
| @monthly      |  Run once a month, | 0  0  1  *  * |
| @weekly       |  Run once a week, | 0 0 * * 0 |
| @daily        |    Run once a day, | 0 0 * * * |
| @midnight     |  (same as @daily) |  |
| @hourly       |   Run once an hour, | 0 * * * * |

## 实例
| <center>例子</center> | <center>执行时间</center> | <center>备注</center> |
| :--- | --- | --- |
| * * * * * | 每分钟执行 | |
| *\/5 * * * * | 每5分钟执行 | |
| 0 5,17 * * * | 每天 5:00和17:00执行 | |
| 0 5-17 * * * | 每天 5:00到17:00每小时执行 | |
| * * * * * sleep 30; sh /scripts/script.sh | 每30秒执行 | |
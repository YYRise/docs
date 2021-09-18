---
title: "Pprof"
date: 2021-09-18T10:44:49+08:00
draft: false
---

## 1. pprof 模块名

- allocs：查看过去所有内存分配的样本。
- block：查看导致阻塞同步的堆栈跟踪。
- cmdline：当前程序的命令行的完整调用路径。
- goroutine：查看当前所有运行的 goroutines 堆栈跟踪。
- heap：查看活动对象的内存分配情况。
- mutex：查看导致互斥锁的竞争持有者的堆栈跟踪。
- profile：默认进行 30s 的 CPU Profiling，得到一个分析用的 profile 文件。
- threadcreate：查看创建新OS线程的堆栈跟踪。

## 2. cpu top 列名

- flat：函数自身的运行耗时。
- flat%：函数自身在 CPU 运行耗时总比例。
- sum%：函数自身累积使用 CPU 总比例。
- cum：函数自身及其调用函数的运行总耗时。
- cum%：函数自身及其调用函数的运行耗时总比例。
- Name：函数名。

## 3. heap 参数

- inuse_space：应用程序的常驻内存占用字节。
- inuse_objects：应用程序的常驻内存分配的对象数量。
- alloc_space：应用程序的临时内存占用字节。
- alloc_objects：应用程序的内存临时分配的对象数量。
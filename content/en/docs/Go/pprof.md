---
title: "Pprof"
date: 2021-09-18T10:44:49+08:00
draft: false
---

## 1. pprof 模块名

- allocs：查看过去所有内存分配的样本。
- block：查看导致阻塞同步的堆栈跟踪。记录Goroutine阻塞等待同步（包括定时器通道）的位置，默认不开启，需要调用 `runtime.SetBlockProfileRate` 进行设置。
- cmdline：当前程序的命令行的完整调用路径。
- goroutine：查看当前所有运行的 goroutines 堆栈跟踪。
- heap：查看活动对象的内存分配情况。
- mutex：互斥锁分析，报告互斥锁的竞争情况，默认不开启，需要调用 `runtime.SetMutexProfileFraction` 进行设置。
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
`$ go tool pprof -inuse_space http://host:port/debug/pprof/heap`

- inuse_space：应用程序的常驻内存占用字节。
- inuse_objects：应用程序的常驻内存分配的对象数量。
- alloc_space：应用程序的临时内存占用字节。
- alloc_objects：应用程序的内存临时分配的对象数量。

## 4. Goroutine

`traces`：打印对应的所有调用栈及指标，查看整个调用链路，分别在哪里使用了多少个 goroutine

## 5. pprof文件

- 1. 下载 `$ wget http://host:port/debug/pprof/profile`
- 2. 命令行查看 `$ go tool pprof profile`
- 3. web查看 `$ go tool pprof -http=:port profile `

在浏览器中显示如图：
![img](/images/Go/web-pprof-graph.png)

### 5.1 在浏览器中查看各View

- 1. Top
![img](/images/Go/web-pprof-top.png)

- 2. Graph
![img](/images/Go/web-pprof-graph.png)

- 3. Peek
![img](/images/Go/web-pprof-peek.png)

- 4. Source
![img](/images/Go/web-pprof-source.png)

- 5. Flame Graph
![img](/images/Go/web-pprof-flame-graph.png)
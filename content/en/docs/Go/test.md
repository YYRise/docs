---
title: "Test"
date: 2021-09-14T11:39:58+08:00
draft: false
---

## 1. go test 参数
`go test [-c] [-i] [build/test flags] [packages] [build/test flags & test binary flags]`

> - `-c` : 编译go test成为可执行的二进制文件，但是不运行测试。
> - `-i` : 安装测试包依赖的package，但是不运行测试。

- 关于build flags，调用 `go help build`，这些是编译运行过程中需要使用到的参数，一般设置为空
- 关于packages，调用 `go help packages`，这些是关于包的管理，一般设置为空
- 关于flags for test binary，调用go help testflag，这些是go test过程中经常使用到的参数

> - `-test.v` : 是否输出全部的单元测试用例（不管成功或者失败），默认没有加上，所以只输出失败的单元测试用例。
> - `-test.run pattern`: 只跑哪些单元测试用例
> - `-test.bench patten`: 只跑那些性能测试用例
> - `-test.benchmem` : 是否在性能测试的时候输出内存情况
> - `-test.benchtime t` : 性能测试运行的时间，默认是1s
> - `-test.cpuprofile cpu.out` : 是否输出cpu性能分析文件
> - `-test.memprofile mem.out` : 是否输出内存性能分析文件
> - `-test.blockprofile block.out` : 是否输出内部goroutine阻塞的性能分析文件
> - `-test.memprofilerate n` : 内存性能分析的时候有一个分配了多少的时候才打点记录的问题。这个参数就是设置打点的内存分配间隔，也就是profile中一个sample代表的内存大小。默认是设置为512 * 1024的。如果你将它设置为1，则每分配一个内存块就会在profile中有个打点，那么生成的profile的sample就会非常多。如果你设置为0，那就是不做打点了。可以通过设置 `memprofilerate=1` 和 `GOGC=off` 来关闭内存回收，并且对每个内存块的分配进行观察。
> - `-test.blockprofilerate n`: 基本同上，控制的是goroutine阻塞时候打点的纳秒数。默认不设置就相当于 `-test.blockprofilerate=1` ，每一纳秒都打点记录一下
> - `-test.parallel n` : 性能测试的程序并行cpu数，默认等于GOMAXPROCS。
> - `-test.timeout t` : 如果测试用例运行时间超过t，则抛出panic
> - `-test.cpu 1,2,4` : 程序运行在哪些CPU上面，使用二进制的1所在位代表，和nginx的nginx_worker_cpu_affinity是一个道理
> - `-test.short` : 将那些运行时间较长的测试用例运行时间缩短



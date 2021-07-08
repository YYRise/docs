---
title: "Redis"
date: 2021-07-07T18:01:50+08:00
draft: false
---

## 1. 占用内存
- 命令： `$ info`

```
# Memory

used_memory:13490096 //数据占用了多少内存（字节）

used_memory_human:12.87M //数据占用了多少内存（带单位的，可读性好）

used_memory_rss:13490096  //redis占用了多少内存

used_memory_peak:15301192 //占用内存的峰值（字节）

used_memory_peak_human:14.59M //占用内存的峰值（带单位的，可读性好）

used_memory_lua:31744  //lua引擎所占用的内存大小（字节）

mem_fragmentation_ratio:1.00  //内存碎片率

mem_allocator:libc //redis内存分配器版本，在编译时指定的。有libc、jemalloc、tcmalloc这3种。
```

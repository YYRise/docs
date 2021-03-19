---
title: "Map"
date: 2021-03-16T09:53:48+08:00
draft: false
---

## 1. 不能比较的类型
- func、map和slice不支持判等操作，所以它们不能用作 map 的 key。
> 原因：golang 的 key 是被转换为 hash 值，和 val 成对存储在 hash 桶中，也就是说 golang 需要先定位到一个 hash 桶，然后使用 key 转换的 hash 值与该 hash 桶中存储的 hash 值逐一比对，如果没有相等的，直接返回结果，如果有相等的，就再用 key 本身去比对一次，原因是为了避免 hash 碰撞，只有 hash 值和 key 比对都相等，证明查找到了 key-val 键值对。

- 定义`map[interface{}]string`时，使用func、map和slice做key，编译时不报错，运行时panic
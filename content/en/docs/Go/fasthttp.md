---
title: "Fasthttp"
date: 2021-09-16T15:03:07+08:00
draft: false
---

## 1. fasthttp VS net/http

- net/http 的实现是一个连接新建一个 goroutine；fasthttp 是利用一个 worker 复用 goroutine，减轻 runtime 调度 goroutine 的压力
- net/http 解析的请求数据很多放在 `map[string]string(http.Header)` 或 `map[string][]string(http.Request.Form)`，有不必要的 []byte 到 string 的转换，是可以规避的
- net/http 解析 HTTP 请求每次生成新的 `*http.Request` 和 `http.ResponseWriter`; fasthttp 解析 HTTP 数据到 `*fasthttp.RequestCtx`，然后使用 `sync.Pool` 复用结构实例，减少对象的数量
- fasthttp 会延迟解析 HTTP 请求中的数据，尤其是 Body 部分。这样节省了很多不直接操作 Body 的情况的消耗
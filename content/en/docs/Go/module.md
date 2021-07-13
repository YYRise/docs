---
title: "Module"
date: 2021-07-09T10:21:00+08:00
draft: false
---

![img](/images/Go/go.mod.png)

## 1. 版本格式

### 1.1 使用git tag

- 一般格式为： `vmajor.minor.patch`

`google.golang.org/protobuf v1.26.0`

### 1.2 无git tag

- 使用伪版本号： `vmajor.minor.patch-yyyyMMddhhmmss-comitid`

`github.com/valyala/tcplisten v0.0.0-20161114210144-ceec8f93295a`

## 2. go.mod只定义`vmajor`或者`vmajor.minor`

```go
github.com/panicthis/B v1.2
github.com/panicthis/C v1
github.com/panicthis/G/v2 v2
```
运行 `go mod tidy`
```go
github.com/panicthis/B v1.2.1
github.com/panicthis/C v1.4.0
github.com/panicthis/G/v2 v2.0.0
```
> go get 命令也一样

## 3. latest, upgrade 和 patch

- `latest`: 选择最高的release版本，如果没有release版本，则选择最高的pre-release版本，如果根本就没有打过tag,则选择最高的伪版本号的版本(默认分支的最后的提交版本)
- `upgrade`: 类似latest,但是如果有比release更高的版本(比如pre-release),会选择更高的版本
- `patch`: major和minor和当前的版本相同，只把patch升级到最高。当然如果没有当前的版本，则无从比较，则patch退化成latest语义

## 4. 指定 commit id 或 head

```go
github.com/panicthis/H 4f7657a
github.com/panicthis/H HEAD
```

## 5. 指定分支

```go
github.com/panicthis/J master
github.com/panicthis/K feat-1
```

## 6. 使用 `>、>=、<、<=` 比较符

```go
github.com/stretchr/testify >v1.7.0
github.com/panicthis/F >=v1.1.0
github.com/panicthis/F <v1.1.0
github.com/panicthis/F <=v1.1.0
```

## 7. `none`

`go get github.com/stretchr/testify@none`

从go module中移出这个依赖。
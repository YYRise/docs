---
title: "Module"
date: 2021-07-09T10:21:00+08:00
draft: false
---

## 1. `go.mod` 文件
![img](/images/Go/go.mod.png)

### 1.1 版本号格式

#### 1.1.1 使用git tag

- 一般格式为： `vmajor.minor.patch`

`google.golang.org/protobuf v1.26.0`

#### 1.1.2 无git tag

- 使用伪版本号： `vmajor.minor.patch-yyyyMMddhhmmss-comitid`

`github.com/valyala/tcplisten v0.0.0-20161114210144-ceec8f93295a`

### 1.2 indirect注释

- 当前项目依赖A,但是A的go.mod遗漏了B, 那么就会在当前项目的go.mod中补充B, 加indirect注释

- 当前项目依赖A,但是A没有go.mod,同样就会在当前项目的go.mod中补充B, 加indirect注释

- 当前项目依赖A,A又依赖B,当对A降级的时候，降级的A不再依赖B,这个时候B就标记indirect注释

### 1.3 incompatible

这些库的版major版本已经大于等于2了，但是他们的module path中依然没有添加v2、v3这样的后缀。

虽然可以引用，但是实际它们是不符合规范的。

### 1.4 exclude

在项目中跳过某个依赖库的某个版本

### 1.5 retract

go 1.16中新增

如果你误发布了某个版本，或者事后发现某个版本不成熟，那么你可以推一个新的版本，在新的版本中，声明前面的某个版本被撤回，提示大家都不要用了。

撤回的版本tag依然还存在，go proxy也存在这个版本，所以你如果强制使用，还是可以使用的，否则这些版本就会被跳过。

和exclude的区别是retract是这个库的owner定义的， 而exclude是库的使用者在自己的go.mod中定义的。

## 2. 用法
### 2.1 go.mod只定义`vmajor`或者`vmajor.minor`

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

### 2.2 latest, upgrade 和 patch

- `latest`: 选择最高的release版本，如果没有release版本，则选择最高的pre-release版本，如果根本就没有打过tag,则选择最高的伪版本号的版本(默认分支的最后的提交版本)
- `upgrade`: 类似latest,但是如果有比release更高的版本(比如pre-release),会选择更高的版本
- `patch`: major和minor和当前的版本相同，只把patch升级到最高。当然如果没有当前的版本，则无从比较，则patch退化成latest语义

### 2.3 指定 commit id 或 head

```go
github.com/panicthis/H 4f7657a
github.com/panicthis/H HEAD
```

### 2.4. 指定分支

```go
github.com/panicthis/J master
github.com/panicthis/K feat-1
```

### 2.5. 使用 `>、>=、<、<=` 比较符

```go
github.com/stretchr/testify >v1.7.0
github.com/panicthis/F >=v1.1.0
github.com/panicthis/F <v1.1.0
github.com/panicthis/F <=v1.1.0
```

### 2.6. `none`

`go get github.com/stretchr/testify@none`

从go module中移出这个依赖。
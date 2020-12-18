---
title: "配置多个远程Git仓库"
date: 2020-12-17T11:14:00+08:00
draft: false
---

[TOC]
## 情景一：不同的库分别 pull/push

### 1. 使用git命令配置
```sh
# 添加
git remote add 名字 仓库地址

# 查看远程仓库
git remote -v 
# origin    仓库地址 (fetch)
# origin    仓库地址 (push)
# 名字    仓库地址 (fetch)
# 名字    仓库地址 (push)

# 删除
git remote remove 名字
```

### 2. 修改.git/config 文件
```sh
[remote "origin"]
        url = 仓库地址
        fetch = +refs/heads/*:refs/remotes/origin/*
[remote "名字"]
        url = 仓库地址
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
        remote = origin
        remote = 名字
        merge = refs/heads/master
        rebase = true
```

### 3. 操作
```sh
git pull/push origin [分支]
git pull/push 名字 [分支]
```
> 默认是 origin 仓库

## 情景二：不同的库一次push

### 1. 使用git命令配置

```sh
# 添加
git remote set-url --add origin 仓库地址

# 查看远程仓库
git remote -v 
# origin    仓库地址1 (fetch)
# origin    仓库地址1 (push)
# origin    仓库地址2 (push)

# 删除
git remote set-url --delete origin 仓库地址
```

### 2. 修改.git/config 文件

```sh
[remote "origin"]
        url = 仓库地址1
        fetch = +refs/heads/*:refs/remotes/origin/*
        url = 仓库地址2
[branch "master"]
        remote = origin
        merge = refs/heads/master
        rebase = true
```

### 3. 操作
```sh
git push [分支]
```
> pull 时默认**仓库地址1**
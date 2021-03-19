---
title: "rz上传和sz下载"
date: 2021-03-05T14:47:29+08:00
draft: false
---

- 不足：无法传输大于 4G 的文件。

## 1. sz 下载远程机器文件
> s: send
```sh
#下载一个文件
sz file

#下载多个文件
sz file1 file2 ……

#下载dir目录下的所有文件，不包含dir下的文件夹
sz dir/*
```

## 2. rz 上传文件到远程机器
> r: received

- 文件已存在时，默认不会上传，使用`-y`覆盖

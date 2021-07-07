---
title: "Curl使用"
date: 2021-07-07T16:40:55+08:00
draft: false
---

## 1. 参数

| 简名 | 全名 | 含义 |
| :--- | :--- | :--- | 
| -A | --user-agent \<string> | 设置用户代理发送给服务器 | 
| -b | --cookie \<name=string/file> | cookie字符串或文件读取位置 | 
| -c | --cookie-jar \<file> | 操作结束后把cookie写入到这个文件中 | 
| -C | --continue-at \<offset> | 断点续转 | 
| -D | --dump-header \<file> | 把header信息写入到该文件中 | 
| -e | --referer | 来源网址 | 
| -f | --fail | 连接失败时不显示http错误 | 
| -o | --output | 把输出写到该文件中 | 
| -O | --remote-name | 把输出写到该文件中，保留远程文件的文件名 | 
| -r | --range \<range> | 检索来自HTTP/1.1或FTP服务器字节范围 | 
| -s | --silent | 静音模式。不输出任何东西 | 
| -T | --upload-file \<file> |  上传文件 | 
| -u | --user \<user[:password]> | 设置服务器的用户和密码 | 
| -w | --write-out [format] | 什么输出完成后 | 
| -x | --proxy \<host[:port]> | 在给定的端口上使用HTTP代理 | 
| -# | --progress-bar | 进度条显示当前的传送状态 | 

## 2. 用法

### 2.1 下载

- 1. 循环下载

`curl -O http://www.linux.com/dodo[1-5].JPG `

- 2. 下载重命名

` curl -O http://www.linux.com/{hello,bb}/dodo[1-5].JPG ` 

> 由于下载的hello与bb中的文件名都是dodo1，dodo2，…… 因此第二次下载的会把第一次下载的覆盖，这样就需要对文件进行重命名。

` curl -o #1_#2.JPG http://www.linux.com/{hello,bb}/dodo[1-5].JPG `

> 这样在hello/dodo1.JPG的文件下载下来就会变成 `hello_dodo1.JPG` 
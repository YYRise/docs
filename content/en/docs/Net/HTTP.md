---
title: "HTTP"
date: 2021-06-25T10:18:56+08:00
draft: false
---

## 1. HTTP/1.0 和 HTTP/1.1 支持的方法

| 方法 | 说明 | 支持的HTTP 协议版本 | 
| --- | --- | --- | 
| GET | 获取资源 | 1.0、 1.1 |
| POST | 传输实体主体 | 1.0、 1.1 |
| PUT | 传输文件 | 1.0、 1.1 |
| HEAD | 获得报文首部 | 1.0、 1.1 |
| DELETE | 删除文件 | 1.0、 1.1 |
| OPTIONS | 询问支持的方法 | 1.1 |
| TRACE | 追踪路径 | 1.1 |
| CONNECT | 要求用隧道协议连接代理 | 1.1 |
| LINK | 建立和资源之间的联系 | 1.0 |
| UNLINK | 断开连接关系 | 1.0 |

## 2. 状态码的类别

| | 类别 | 原因短语 |
| --- | --- | --- |
| 1xx | Informational(信息性状态码) | 接收的请求正在处理 | 
| 2xx | Success (成功状态码) | 请求正常处理完毕 |
| 3xx | Redirection (重定向状态码) | 需要进行附加操作以完成请求 |
| 4xx | Client Error (客户端错误状态码) | 服务器无法处理请求 |
| 5xx | Server Error (服务器错误状态码) | 服务器处理请求错误 |


## 3. HTTP/1.1 首部字段

### 3.1 通用首部字段

| 首部字段名 | 说明 |
| :--- | :--- |
| Cache-Control | 控制缓存的行为 |
| Connection | 逐跳首部、连接的管理 |
| Date | 创建报文的日期时间 |
| Pragma | 报文指令 |
| Trailer | 报文末端的首部一览 |
| Transfer-Encoding | 指定报文主体的传输编码方式 |
| Upgrade | 升级为其他协议 |
| Via | 代理服务器的相关信息 |
| Warning | 错误通知 |

### 3.2 请求首部字段

| 首部字段名 | 说明 |
| :--- | :--- |
| Accept | 用户代理可处理的媒体类型 |
| Accept-Charset | 优先的字符集 |
| Accept-Encoding | 优先的内容编码 |
| Accept-Language | 优先的语言（自然语言） |
| Authorization | Web认证信息 |
| Expect | 期待服务器的特定行为 |
| From | 用户的电子邮箱地址 |
| Host | 请求资源所在服务器 |
| If-Match | 比较实体标记（ETag） |
| If-Modified-Since | 比较资源的更新时间 |
| If-None-Match | 比较实体标记（与 If-Match 相反） |
| If-Range | 资源未更新时发送实体 Byte 的范围请求 |
| If-Unmodified-Since | 比较资源的更新时间（与If-Modified-Since相反） |
| Max-Forwards | 最大传输逐跳数 |
| Proxy-Authorization | 代理服务器要求客户端的认证信息 |
| Range | 实体的字节范围请求 |
| Referer | 对请求中 URI 的原始获取方 |
| TE | 传输编码的优先级 |
| User-Agent | HTTP 客户端程序的信息 |

### 3.3 响应首部字段

| 首部字段名 | 说明 |
| :--- | :--- |
| Accept-Ranges | 是否接受字节范围请求 |
| Age | 推算资源创建经过时间 |
| ETag | 资源的匹配信息 |
| Location | 令客户端重定向至指定URI |
| Proxy-Authenticate | 代理服务器对客户端的认证信息 |
| Retry-After | 对再次发起请求的时机要求 |
| Server | HTTP服务器的安装信息 |
| Vary | 代理服务器缓存的管理信息 |
| WWW-Authenticate | 服务器对客户端的认证信息 |

### 3.4 实体首部字段

| 首部字段名 | 说明 |
| :--- | :--- |
| Allow | 资源可支持的HTTP方法 |
| Content-Encoding | 实体主体适用的编码方式 |
| Content-Language | 实体主体的自然语言 |
| Content-Length | 实体主体的大小（单位：字节） |
| Content-Location | 替代对应资源的URI |
| Content-MD5 | 实体主体的报文摘要 |
| Content-Range | 实体主体的位置范围 |
| Content-Type | 实体主体的媒体类型 |
| Expires | 实体主体过期的日期时间 |
| Last-Modified | 资源的最后修改日期时间 |

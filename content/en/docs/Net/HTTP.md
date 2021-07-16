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

| 类别 | 原因短语 |
| --- | --- | --- |
| 1xx | Informational(信息性状态码) | 接收的请求正在处理 | 
| 2xx | Success (成功状态码) | 请求正常处理完毕 |
| 3xx | Redirection (重定向状态码) | 需要进行附加操作以完成请求 |
| 4xx | Client Error (客户端错误状态码) | 服务器无法处理请求 |
| 5xx | Server Error (服务器错误状态码) | 服务器处理请求错误 |

### 2.1 2XX 成功

| 状态码 | 含义 | 说明 |
| :--- | :--- | :--- |
| 204 | No Content | 请求成功处理，但不返回实体，浏览器页面不更新 | 
| 206 | Partial Content | 成功处理客户端Content-Range指定的范围请求 |

### 2.2 3XX 重定向

| 状态码 | 含义 | 说明 |
| :--- | :--- | :--- |
| 301 | Moved Permanently | 永久性重定向 | 
| 302 | Found | 临时性重定向，客户端或代理可能会将POST变换成GET | 
| 303 | See Other | 应使用GET方法从另一个URI请求资源，与 302 状态码有区别 |
| 304 | Not Modified | 未满足客户端附带条件（请求报文中含If-Match，If-Modified-Since，If-None-Match，If-Range，If-Unmodified-Since）的GET请求 |
| 307 | Temporary Redirect | 临时重定向，与302相同，但请求方法不会变 |
| 308 | Permanent Redirect | 和 301 是一致的，区别在于，308 状态码不允许浏览器将原本为 POST 的请求重定向到 GET 请求上 |

### 2.3 4XX 客户端错误

| 状态码 | 含义 | 说明 |
| :--- | :--- | :--- |
| 400 | Bad Request | 请求报文中存在语法错误 |
| 401 | Unauthorized | 请求需要有通过 HTTP 认证（BASIC 认证、DIGEST 认证）的认证信息。若之前已进行过 1 次请求，则表示用 户认证失败 |
| 403 | Forbidden | 访问被拒绝，没有必要给出拒绝的详细理由 |
| 404 | Not Found | 服务器上无法找到请求的资源，也可以在服务器端拒绝请求且不想说明理由时使用 |

### 2.4 5XX 服务器错误

| 状态码 | 含义 | 说明 |
| :--- | :--- | :--- |
| 500 | Internal Server Error | 服务器端在执行请求时发生了错误 |
| 503 | Service Unavailable | 服务器暂时处于超负载或正在进行停机维护，现在无法
处理请求 | 

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

- Host 在 HTTP/1.1 规范内是唯一一个必须被包含在请求内的首部字段。
> 首部字段 Host 和以单台服务器分配多个域名的虚拟主机的工作机制有很密切的关联，这是首部字段 Host 必须存在的意义。

> 请求被发送至服务器时，请求中的主机名会用 IP 地址直接替换解决。但如果这时，相同的 IP 地址下部署运行着多个域名，那么服务器就会无法理解究竟是哪个域名对应的请求。因此，就需要使用首部字段 Host 来明确指出请求的主机名。若服务器未设定主机名，那直接发送一个空值即可

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

## 4. Cookie

### 4.1 Set-Cookie 字段属性

| 属性 | 说明 |
| :--- | :--- |
| NAME=VALUE | 赋予 Cookie 的名称和其值（必需项）|
| expires=DATE | Cookie 的有效期（默认为浏览器关闭前为止）|
| path=PATH | 将服务器上的文件目录作为Cookie的适用对象（若不指定则默认为文档所在的文件目录）|
| domain=域名 |作为 Cookie 适用对象的域名 （若不指定则默认为创建 Cookie的服务器的域名）|
| Secure | 仅在 HTTPS 安全通信时才会发送 Cookie |
| HttpOnly | 加以限制，使 Cookie 不能被 JavaScript 脚本访问 |

> 一旦 Cookie 从服务器端发送至客户端，服务器端就不存在可以显式删除 Cookie 的方法。但可通过覆盖已过期的 Cookie，实现对客户端 Cookie 的实质性删除操作。
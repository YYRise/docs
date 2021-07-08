---
title: "HTTPS"
date: 2021-07-08T16:11:26+08:00
draft: false
---

## 1. HTTPS 加密、解密、验证、及数据传输过程

![img](/images/Net/HTTPS/https-1.png)

## 2. SSL/TLS 握手过程

![img](/images/Net/HTTPS/ssl_tls.png)

## 3. HTTPS 通信步骤

![img](/images/Net/HTTPS/https-2.png)

1. 客户端通过发送 Client Hello 报文开始 SSL 通信。报文中包含:
> Version: 客户端支持的 SSL 的指定版本（版本向下兼容）
> Random: 一个随机数
> Cipher Suite: 加密组件列表（所使用的加密算法及密钥长度等）

2. 服务器可进行 SSL 通信时，会以 Server Hello 报文作为应答。和客户端一样，在报文中包含 SSL 版本、随机数以及加密组件。服务器的加密组件内容是从接收到的客户端加密组件内筛选出来的。

3. 之后服务器发送 Certificate 报文。报文中包含CA证书。 该证书通常有两个目的：
> 1. 身份验证
> 2. 证书中包含服务器的公钥，该公钥结合密码套件的密钥协商算法协商出预备主密钥。

4. 最后服务器发送 Server Hello Done 报文通知客户端，最初阶段的 SSL 握手协商部分结束。

5. SSL 第一次握手结束之后，客户端以 Client Key Exchange 报文作为回应。报文中包含通信加密中使用的一种被称为 Pre-master secret 的随机密码串。该报文已用步骤 3 中的公钥进行加密。

6. 接着客户端继续发送 Change Cipher Spec 报文。该报文会提示服务器，在此报文之后的通信会采用 Pre-master secret 密钥加密。

7. 客户端发送 Finished 报文。该报文包含连接至今全部报文的整体校验值。这次握手协商是否能够成功，要以服务器是否能够正确解密该报文作为判定标准。

8. 服务器同样发送 Change Cipher Spec 报文。

9. 服务器同样发送 Finished 报文。

10. 服务器和客户端的 Finished 报文交换完毕之后，SSL 连接就算建立完成。当然，通信会受到 SSL 的保护。从此处开始进行应用层协议的通信，即发送 HTTP 请求。

11. 应用层协议通信，即发送 HTTP 响应。

12. 最后由客户端断开连接。断开连接时，发送 close_notify 报文。上图做了一些省略，这步之后再发送 TCP FIN 报文来关闭与 TCP的通信。
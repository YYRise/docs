---
title: "ssh公钥远程登录"
date: 2021-03-03T10:34:23+08:00
draft: false
---

## 1. ssh-keygen 生成秘钥
- 默认路径
> `~/.ssh/id_rsa`: 私钥
> `~/.ssh/id_rsa.pub`: 公钥

## 2. 公钥部署到远程服务器

将公钥的内容追加到 `~/.ssh/authorized_keys`
- 在客户端机上执行命令： `$ ssh-copy-id user@host`
- 或执行以下名令：
``` sh
#在本机上执行此命令，上传公钥
scp -P your_port host.pub user@hostname:/tmp
#在服务器上执行此命令，追加到authorized_keys
cd /tmp && cat host.pub >> ~/.ssh/authorized_keys
#更改权限
chmod 600 ~/.ssh/authorized_keys
```
- 或用下面的命令
```sh
$ ssh user@host 'mkdir -p .ssh && cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub
```
> （1）"$ ssh user@host"，表示登录远程主机；
> （2）单引号中的mkdir .ssh && cat >> .ssh/authorized_keys，表示登录后在远程shell上执行的命令;
> （3）"$ mkdir -p .ssh"的作用是，如果用户主目录中的.ssh目录不存在，就创建一个；
> （4）'cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub的作用是，将本地的公钥文件~/.ssh/id_rsa.pub，重定向追加到远程文件authorized_keys的末尾。


## 3. 更改服务器配置 
配置文件为：`/etc/ssh/sshd_config`，检查下面几行前面"#"注释是否去掉
```conf
RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
```  

## 4. 重启服务器ssh服务
```sh
// ubuntu系统
service ssh restart

// debian系统
/etc/init.d/ssh restart
```

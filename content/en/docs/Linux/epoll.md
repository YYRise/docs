---
title: "Epoll"
date: 2021-10-20T15:54:43+08:00
draft: false
---

## 1. epoll 接口

### 1.1 unix.EpollCreate1(flag int) (fd int, err error)
> 创建epoll句柄 

参数：
- flag: 
	> flag = 0:  表示和epoll_create函数完全一样，不需要size的提示了
	> flag = EPOLL_CLOEXEC: 创建的epfd会设置FD_CLOEXEC，一般使用该值
	> flag = EPOLL_NONBLOCK: 创建的epfd会设置为非阻塞

返回值：
- fd: 即epfd

### 1.2 unix.EpollCtl(epfd int, op int, fd int, event *EpollEvent) (err error)
> 将被监听的fd添加到epoll句柄或从epool句柄中删除或者对监听事件进行修改。 

参数：
- epfd: epoll_create返回的的epoll fd
- op: 操作值
	> EPOLL_CTL_ADD： 注册新的fd到epfd中
	> EPOLL_CTL_MOD： 修改已经注册的fd的监听事件；
	> EPOLL_CTL_DEL： 从epfd中删除一个fd
- fd：需要操作/监听的文件句柄 
- event：是告诉内核需要监听什么事件
	> EPOLLIN：触发该事件，表示对应的文件描述符上有可读数据。(包括对端SOCKET正常关闭)； 
	> EPOLLOUT：触发该事件，表示对应的文件描述符上可以写数据； 
	> EPOLLPRI：表示对应的文件描述符有紧急的数据可读（这里应该表示有带外数据到来）； 
	> EPOLLERR：表示对应的文件描述符发生错误； 
	> EPOLLHUP： 表示对应的文件描述符被挂断； 
	> EPOLLET：将EPOLL设为边缘触发(EdgeTriggered)模式，这是相对于水平触发(Level Triggered)来说的。 
	> EPOLLONESHOT： 只监听一次事件，当监听完这次事件之后，如果还需要继续监听这个socket的话，需要再次把这个socket加入到EPOLL队列里。  

### 1.3 unix.EpollWait(epfd int, events []EpollEvent, msec int) (n int, err error) 
> 等侍注册在epfd上的socket fd的事件的发生，如果发生则将发生的fd和事件类型放入到events数组中

参数： 
- epfd：由epoll_create 生成的epoll文件描述符 
- events：用于回传代处理事件的数组 
- timeout：等待I/O事件发生的超时毫秒数，-1相当于阻塞，0相当于非阻塞。一般用-1即可
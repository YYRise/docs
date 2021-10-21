---
title: "Epoll"
date: 2021-10-20T15:54:43+08:00
draft: false
---

## 1. epoll 接口

### 1.1 unix.EpollCreate1(flag int) (fd int, err error)
> 创建epoll句柄 
> 调用epoll_create时，内核除了帮我们在epoll文件系统里建了个file结点，在内核cache里还建了个红黑树 用于存储以后epoll_ctl传来的socket外，还会再建立一个list链表，用于存储准备就绪的事件.

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
- timeout：等待I/O事件发生的超时毫秒；一般用-1即可
	> timeout = -1: 一直阻塞，直到有文件描述符进入ready状态或者捕获到信号才返回；
	> timeout = 0: 非阻塞检测是否有描述符处于ready状态，不管结果怎么样，调用都立即返回；
	> timeout > 0: 表示调用将最多持续timeout时间，如果期间有检测对象变为ready状态或者捕获到信号则返回，否则直到超时。

## 2. epoll 两种触发方式

> select和poll只支持 LT 工作模式，epoll的默认的工作模式是LT模式。

### 2.1 水平触发 (level trigger，LT)

- 读：只要缓冲内容不为空，LT模式返回读就绪。
- 写：只要缓冲区还不满，LT模式会返回写就绪。

### 2.2 边缘触发 (edge trigger，ET)

- 读：
> 1. 当缓冲区由不可读变为可读的时候，即缓冲区由空变为不空的时候。
> 2. 当有新数据到达时，即缓冲区中的待读数据变多的时候。
> 3. 当缓冲区有数据可读，且应用进程对相应的 fd 进行EPOLL_CTL_MOD 修改EPOLLIN事件时。

- 写：
> 1. 当缓冲区由不可写变为可写时。
> 2. 当有旧数据被发送走，即缓冲区中的内容变少的时候。
> 3. 当缓冲区有空间可写，且应用进程对相应的 fd 进行EPOLL_CTL_MOD 修改EPOLLOUT事件时。。


## 3. epoll 高效

- 1. 使用红黑树管理fd，做到了增删改之后性能的优化和平衡
- 2. epoll 池添加 fd 的时候，调用 file_operations->poll ，把这个 fd 就绪之后的回调路径安排好。通过事件通知的形式，做到最高效的运行；
- 3. select和poll的动作基本一致，只是poll采用链表来进行文件描述符的存储，而select采用fd标注位来存放，所以select会受到最大连接数的限制，而poll不会。
- 4. select、poll、epoll虽然都会返回就绪的文件描述符数量。但是select和poll并不会明确指出是哪些文件描述符就绪，而epoll会。造成的区别就是，系统调用返回后，调用select和poll的程序需要遍历监听的整个文件描述符找到是谁处于就绪，而epoll则直接处理即可。
- 5. select、poll都需要将有关文件描述符的数据结构拷贝进内核，最后再拷贝出来。而epoll创建的有关文件描述符的数据结构本身就存于内核态中，系统调用返回时利用mmap()文件映射内存加速与内核空间的消息传递：即epoll使用mmap减少复制开销。
- 6. select、poll采用轮询的方式来检查文件描述符是否处于就绪态，而epoll采用回调机制。造成的结果就是，随着fd的增加，select和poll的效率会线性降低，而epoll不会受到太大影响，除非活跃的socket很多。
- 7. epoll的边缘触发模式效率高，系统不会充斥大量不关心的就绪文件描述符

![img](/images/Linux/epoll池.png)
> epoll相比于select并不是在所有情况下都要高效，例如在如果有少于1024个文件描述符监听，且大多数socket都是出于活跃繁忙的状态，这种情况下，select要比epoll更为高效，因为epoll会有更多次的系统调用，用户态和内核态会有更加频繁的切换。

### 3.1 epoll高效的本质：

- 1. 减少了用户态和内核态的文件句柄拷贝
- 2. 减少了对可读可写文件句柄的遍历
- 3. mmap 加速了内核与用户空间的信息传递，epoll是通过内核与用户mmap同一块内存，避免了无谓的内存拷贝
- 4. IO性能不会随着监听的文件描述的数量增长而下降
- 5. 使用红黑树存储fd，以及对应的回调函数，其插入，查找，删除的性能不错，相比于hash，不必预先分配很多的空间

## 4. epoll、 select、 poll 对比

### 4.1 用户态将文件描述符传入内核的方式

- `select`：创建3个文件描述符集并拷贝到内核中，分别监听读、写、异常动作。这里受到单个进程可以打开的fd数量限制，默认是1024。
- `poll`：将传入的struct pollfd结构体数组拷贝到内核中进行监听。
- `epoll`：执行epoll_create会在内核的高速cache区中建立一颗红黑树以及就绪链表(该链表存储已经就绪的文件描述符)。接着用户执行的epoll_ctl函数添加文件描述符会在红黑树上增加相应的结点。

### 4.2 内核态检测文件描述符读写状态的方式

- `select`：采用轮询方式，遍历所有fd，最后返回一个描述符读写操作是否就绪的mask掩码，根据这个掩码给fd_set赋值。
- `poll`：同样采用轮询方式，查询每个fd的状态，如果就绪则在等待队列中加入一项并继续遍历。
- `epoll`：采用回调机制。在执行epoll_ctl的add操作时，不仅将文件描述符放到红黑树上，而且也注册了回调函数，内核在检测到某文件描述符可读/可写时会调用回调函数，该回调函数将文件描述符放在就绪链表中。

### 4.3 找到就绪的文件描述符并传递给用户态的方式

- `select`：将之前传入的fd_set拷贝传出到用户态并返回就绪的文件描述符总数。用户态并不知道是哪些文件描述符处于就绪态，需要遍历来判断。
- `poll`：将之前传入的fd数组拷贝传出用户态并返回就绪的文件描述符总数。用户态并不知道是哪些文件描述符处于就绪态，需要遍历来判断。
- `epoll`：epoll_wait只用观察就绪链表中有无数据即可，最后将链表的数据返回给数组并返回就绪的数量。内核将就绪的文件描述符放在传入的数组中，所以只用遍历依次处理即可。这里返回的文件描述符是通过mmap让内核和用户空间共享同一块内存实现传递的，减少了不必要的拷贝。

### 4.4 重复监听的处理方式

- `select`：将新的监听文件描述符集合拷贝传入内核中，继续以上步骤。
- `poll`：将新的struct pollfd结构体数组拷贝传入内核中，继续以上步骤。
- `epoll`：无需重新构建红黑树，直接沿用已存在的即可。

# I/O模型

## 基础概念

### 文件描述符fd 

文件描述符(file description),用于表述指向文件引用的抽象话题概念。

文件描述符在形式上是一个非负整数,实际上它是一个索引值,指向内核为每一个进程所维护的该进程打开文件的记录表,当程序打开一个现有文件或者创建一个新文件时,内核就向该进程返回一个文件描述符。

unix系统把任何对象看做是文件,文件就是一串二进制流,我门对数据(流)的读写操作就是对文件的操作,所以当我们的进程在做读写操作时会返回一个记录访问位置的索引值,当我们继续操作该文件时可直接通过这个索引值到达上一次的位置。

文件描述符限制

在编写文件操作的或者网络通信的软件时，初学者一般可能会遇到“Too many open files”的问题。这主要是因为文件描述符是系统的一个重要资源，虽然说系统内存有多少就可以打开多少的文件描述符，但是在实际实现过程中内核是会做相应的处理的，一般最大打开文件数会是系统内存的10%（以KB来计算）（称之为系统级限制），查看系统级别的最大打开文件数可以使用sysctl -a | grep fs.file-max命令查看。与此同时，内核为了不让某一个进程消耗掉所有的文件资源，其也会对单个进程最大打开文件数做默认值处理（称之为用户级限制），默认值一般是1024，使用ulimit -n命令可以查看。在Web服务器中，通过更改系统默认值文件描述符的最大值来优化服务器是最常见的方式之一。

### 用户空间和内核空间 

*TODO*

## 通俗解释

引用自：https://www.jianshu.com/p/6a6845464770

> 1.**阻塞I/O模型**
>  老李去火车站买票，排队三天买到一张退票。
>  耗费：在车站吃喝拉撒睡 3天，其他事一件没干。

> 2.**非阻塞I/O模型**
>  老李去火车站买票，隔12小时去火车站问有没有退票，三天后买到一张票。耗费：往返车站6次，路上6小时，其他时间做了好多事。

> 3.**I/O复用模型**
>  [1.select/poll](https://link.zhihu.com/?target=http%3A//1.select/poll)
>  老李去火车站买票，委托黄牛，然后每隔6小时电话黄牛询问，黄牛三天内买到票，然后老李去火车站交钱领票。
>  耗费：往返车站2次，路上2小时，黄牛手续费100元，打电话17次
>  2.epoll
>  老李去火车站买票，委托黄牛，黄牛买到后即通知老李去领，然后老李去火车站交钱领票。
>  耗费：往返车站2次，路上2小时，黄牛手续费100元，无需打电话

> 4.**信号驱动I/O模型**
>  老李去火车站买票，给售票员留下电话，有票后，售票员电话通知老李，然后老李去火车站交钱领票。
>  耗费：往返车站2次，路上2小时，免黄牛费100元，无需打电话

> 5.**异步I/O模型**
>  老李去火车站买票，给售票员留下电话，有票后，售票员电话通知老李并快递送票上门。
>  耗费：往返车站1次，路上1小时，免黄牛费100元，无需打电话

## I/O模型

### 阻塞I/O（blocking I/O ）

I/O最普遍的模型是阻塞I/O模型（我们在前面部分中使用的所有的例子都是阻塞I/O）。默认情况下，所有Socket都是阻塞的。场景如下图所示：

![img](D:\技术文档\面试资料整理\git\文章\gif\06fig01.gif) 

我们使用UDP代替TCP，因为使用UDP，"ready"的数据读取的概念很简单：要么接收了整个数据报，要么没有。随着TCP变得更复杂，因为诸如socket的低水位标志等其他变量开始发挥作用。

我们还将`recvfrom`作为一个系统调用来区分我们的应用程序和内核，而不管`recvfrom`如何实现。通常有一个从应用程序运行到内核中运行的切换，稍后返回应用程序。

在上图中，进程调用`recvfrom`并且系统调用不会返回直到数据到达并复制到应用程序缓冲区，或发生错误。最常见的错误是系统调用被信号中断，正如我们在5.9节介绍。我们说在它调用`recvfrom`返回之前正整个时间段内进程被阻塞。当`recvfrom`成功返回后，我们的应用程序处理数据报。

### 非阻塞I/O(nonblocking I/O)

当`socket`被设置为非阻塞时，我们告诉内核“当我请求的I/O操作不能完成而不使进程休眠时，不要使进程休眠，而是返回一个错误”。下图如下：

![img](D:\技术文档\面试资料整理\git\文章\gif\figure_6.2.png) 

- 在最初的三次 `recvfrom`调用，没有数据返回，内核立即返回一个错误`ewouldblock`。
- 对于我们第四次调用`recvfrom`，数据准备好了，它被复制到我们的应用程序缓冲区，并且recvfrom返回成功。然后我们处理数据。

当一个应用程序对一个非阻塞的描述符循环调用`recvfrom`时，它被称为`polling`。应用程序不断轮询内核，看看是否有一些操作已经准备就绪。这通常是浪费CPU时间，但是这个模型偶尔会遇到，通常在专用于一个函数的系统上。

### I/O多路复用（I/O multiplexing ）

With **I/O multiplexing**, we call `select` or `poll` and block in one of these two system calls, instead of blocking in the actual I/O system call. The figure is a summary of the I/O multiplexing model: 

![img](D:\技术文档\面试资料整理\git\文章\gif\figure_6.3.png) 

We block in a call to `select`, waiting for the datagram socket to be readable. When `select` returns that the socket is readable, we then call `recvfrom` to copy the datagram into our application buffer.

#### Comparing to the blocking I/O model 

Comparing [Figure 6.3](https://notes.shichao.io/unp/figure_6.3.png) to [Figure 6.1](https://notes.shichao.io/unp/figure_6.1.png):

- Disadvantage: using `select` requires two system calls (`select` and `recvfrom`) instead of one
- Advantage: we can wait for more than one descriptor to be ready (see [the `select` function](https://notes.shichao.io/unp/ch6/#select-function) later in this chapter)

#### Multithreading with blocking I/O

Another closely related I/O model is to use multithreading with blocking I/O. That model very closely resembles the model described above, except that instead of using `select` to block on multiple file descriptors, the program uses multiple threads (one per file descriptor), and each thread is then free to call blocking system calls like `recvfrom`.

#### select 、poll 、epoll



### signal driven I/O (SIGIO)



### asynchronous I/O (the POSIX aio_functions)
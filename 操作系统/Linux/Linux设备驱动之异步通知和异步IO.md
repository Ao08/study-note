# Linux设备驱动之异步通知和异步I/O

在设备驱动中使用异步通知可以使得对设备的访问可进行时，由驱动主动通知应用程序进行访问。因此，使用无阻塞I/O的应用程序无需轮询设备是否可访问，而阻塞访问也可以被类似“中断”的异步通知所取代。**异步通知类似于硬件上的“中断”概念，比较准确的称谓是“信号驱动的异步I/O”**。

## 1、linux异步通知编程

### linux信号

作用：linux系统中，**异步通知可以使用信号来实现**（除了使用信号来实现，**还可以使用回调函数**）。

函数原型为：

```c
void (*signal(int signum,void (*handler))(int)))(int)
```

第一个参数是指定信号的值，第二个参数是指定针对前面信号的处理函数。

## 2、linux2.6异步I/O

同步I/O：linux系统中最常用的输入输出（I/O）模型是同步I/O，在这个模型中，当请求发出后，应用程序就会阻塞，直到请求满足。

异步I/O：I/O请求可能需要与其它进程产生交叠。

Linux 系统中最常用的输入/输出（I/O）模型是同步 I/O。在这个模型中，当请求发出之后，应用程序就会阻塞，直到请求满足为止。**这是很好的一种解决方案，因为调用应用程序在等待 I/O 请求完成时不需要使用任何中央处理单元（CPU）。**但是在某些情况下，I/O 请求可能需要与其他进程产生交叠。可移植操作系统接口（POSIX）异步 I/O（AIO）应用程序接口（API）就提供了这种功能。

## 3、AIO系列API：

### aio_read–异步读

```c
int aio_read( struct aiocb *aiocbp );
```

aio_read()函数在请求进行排队之后会立即返回。如果执行成功，返回值就为 0；如果出现错误，返回值就为−1，并设置 errno 的值。

### aio_write–异步写

```c
int aio_write( struct aiocb *aiocbp );
```

aio_write()函数会立即返回，说明请求已经进行排队（成功时返回值为 0，失败时返回值为−1，并相应地设置 errno。

### aio_error–确定请求的状态

```c
int aio_error( struct aiocb *aiocbp );
```

这个函数可以返回以下内容。

EINPROGRESS：说明请求尚未完成。

ECANCELLED：说明请求被应用程序取消了。

-1：说明发生了错误，具体错误原因由 errno 记录。

## AIO的通知

### 使用信号作为AIO的通知

信号作为异步通知的机制在AIO中依然使用，为了使用信号，使用AIO的应用程序同样需要定义信号处理程序，在指定的信号被触发时，调用这个处理程序，作为信号上下文的一部分，特定的 aiocb 请求被提供给信号处理函数用来区分 AIO 请求。 

### 使用回调函数作为AIO的通知

回调函数就是一个通过函数指针调用的函数。

![](http://oklbfi1yj.bkt.clouddn.com/Linux%E8%AE%BE%E5%A4%87%E9%A9%B1%E5%8A%A8%E4%B9%8B%E5%BC%82%E6%AD%A5%E9%80%9A%E7%9F%A5%E5%92%8C%E5%BC%82%E6%AD%A5I/O/1.png)








































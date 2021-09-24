# Epoll使用和注意事项

**epoll_create：**

　　–`int epoll_create(int size);`

　　–epoll_create创建一个epoll的句柄。

　　–参数size指定epoll所支持的最大句柄数。

　　–函数会返回一个新的epoll句柄，之后的所有操作将通过这个句柄来进行操作。

　　–在用完句柄之后，需要用close()来关闭这个创建出来的epoll句柄。

------

#### <font color="red">注意事项1：有操作的`events[i].data.fd`一旦被读取或写入`send(events[i].data.fd)`或`recv(events[i].data.fd)`之后`events[i].data.fd`值会变成0（fd=0），导致不能再读取</font>

**epoll_ctl：**

　　–`int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);`

　　–参数epfd是epoll_create()的返回值。

　　–参数op表示动作，用三个宏来表示：

- EPOLL_CTL_ADD：注册新的fd到epfd中；
- EPOLL_CTL_MOD：修改已经注册的fd的监听事件；
- EPOLL_CTL_DEL：从epfd中删除一个fd；

　　–参数fd是需要监听的socket描述符。

　　–参数event通知内核需要监听什么事件。

------

```C++
struct epoll_event结构如下：
typedef union epoll_data {
    void *ptr;
    int fd;
    __uint32_t u32;
    __uint64_t u64;
} epoll_data_t;

struct epoll_event {
    __uint32_t events; /* Epoll events */
    epoll_data_t data; /* User data variable */
};
```



**events定义：**

　　–EPOLLIN ：表示对应的文件描述符可以读（包括对端SOCKET正常关闭）；

　　–EPOLLOUT：表示对应的文件描述符可以写；

　　–EPOLLPRI：表示对应的文件描述符有紧急的数据可读（这里应该表示有带外数据到来）；

　　–EPOLLERR：表示对应的文件描述符发生错误；

　　–EPOLLHUP：表示对应的文件描述符被挂断；

　　–EPOLLET： 将EPOLL设为边缘触发(Edge Triggered)模式，这是相对于水平触发(Level Triggered)来说的；

　　–EPOLLONESHOT：只监听一次事件，当监听完这次事件之后，如果还需要继续监听这个socket的话，需要再次把这个socket加入到EPOLL队列里

------

**epoll_wait：**

　　–`int epoll_wait(int epfd, struct epoll_event *events, int maxevents, int timeout);`

　　–参数epfd是epoll_create()的返回值。

　　–参数events是一个epoll_event*的指针，当epoll_wait这个函数操作成功之后，epoll_events里面将储存所有的读写事件。

　　–参数maxevents是当前需要监听的所有socket句柄数。

　　–参数timeout是 epoll_wait的超时，为0的时候表示马上返回，为-1的时候表示一直等下去，直到有事件范围，正整数表示等这么长的时间。

　　–一般如果网络主循环是单独的线程的话，可以用-1来等，这样可以保证一些效率，如果是和主逻辑在同一个线程的话，则可以用0来保证主循环的效率。

epoll_wait范围之后应该是一个循环，遍历所有的事件。

------

**关于ET、LT两种工作模式：**

**LT（level triggered）是缺省的工作方式，并且同时支持block和no-block socket.**

　　–在LT模式中，内核通知一个文件描述符是否就绪了，然后可以对这个就绪的fd进行IO操作。

　　–如果你不作任何操作，内核还是会继续通知你的，所以，这种模式编程出错误可能性要小一点。

**ET （edge-triggered）是高速工作方式，只支持no-block socket。**

　　–在ET模式下，当描述符从未就绪变为就绪时，内核通过epoll告诉你。

　　–ET模式会假设你知道文件描述符已经就绪，并且不会再为那个文件描述符发送更多的就绪通知，直到你做了某些操作导致那个文件描述符不再为就绪状态了。

　　–如果一直不对这个fd作IO操作（从而导致它再次变成未就绪），内核不会发送更多的通知。

ET和LT的区别：

　　–LT事件不会丢弃，而是只要读buffer里面有数据可以让用户读，则不断的通知你。

　　–ET则只在事件发生之时通知。可以简单理解为LT是水平触发，而ET则为边缘触发。

　　–LT模式只要有事件未处理就会触发，而ET则只在高低电平变换时（即状态从1到0或者0到1）触发。
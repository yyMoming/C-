# select和poll

####  select 函数

```C++
#include <sys/select.h>
#include <sys/time.h>
int select(int maxfdp1,fd_set *readset,fd_set *writeset,fd_set *exceptset,const struct timeval *timeout)
返回值：就绪描述符的数目，超时返回0，出错返回-1
```

 - 第一个参数maxfdp1指定待测试的描述字个数，它的值是待测试的最大描述字加1（因此把该参数命名为maxfdp1），描述字0、1、2...maxfdp1-1均将被测试。

 - 中间的三个参数readset、writeset和exceptset指定我们要让内核测试读、写和异常条件的描述字。如果对某一个的条件不感兴趣，就可以把它设为空指针。struct fd_set可以理解为一个集合，这个集合中存放的是文件描述符，可通过以下四个宏进行设置：

   	- void FD_ZERO(fd_set *fdset);      //清空集合
   	- void FD_SET(int fd, fd_set *fdset);  //将一个给定的文件描述符加入集合之中
   	- void FD_CLR(int fd, fd_set *fdset);  //将一个给定的文件描述符从集合中删除
   	- int FD_ISSET(int fd, fd_set *fdset);  // 检查集合中指定的文件描述符是否可以读写 

   > 注意：每次select循环之前要复制一个fdset，不然每次select会改变fdset内容

![img](/Users/admin/Documents/ywm_info/C++/C-/Sightsing项目/image/select.png)

------


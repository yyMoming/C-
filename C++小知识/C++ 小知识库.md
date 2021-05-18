## C++ 小知识库

------

## 1. C 或 C++ 如何通过程序执行 shell 命令并获取命令执行结果？

参考链接：[C++ 通过程序执行shell命令并获取结果](https://mp.weixin.qq.com/s/feyfQtV1m7Pys4e5aBOchg)

或者 https://blog.csdn.net/youngstar70/article/details/70305687

##  

## 2. C++ 内存管理

参考链接：[C++内存管理全景指南](https://mp.weixin.qq.com/s/mLE73t6jVpzGLqoxK_iJcg)

或者 https://github.com/gperftools/gperftools/blob/master/docs/heapprofile.html



## 3. C++ 处理C代码

```c
#ifdef __cplusplus 
extern "C" { 
#endif

//一段代码

#ifdef __cplusplus 
} 
#endif 
```

这样的代码到底是什么意思呢？首先，__cplusplus是cpp中的自定义宏，那么定义了这个宏的话表示这是一段cpp的代码，也就是说，上面的代码的含义是:如果这是一段cpp的代码，那么加入extern "C"{和}处理其中的代码。

要明白为何使用extern "C"，还得从cpp中对函数的重载处理开始说起。在c++中，为了支持重载机制，在编译生成的汇编码中，要对函数的名字进行一些处理，加入比如函数的返回类型等等.而在C中，只是简单的函数名字而已，不会加入其他的信息.也就是说:C++和C对产生的函数名字的处理是不一样的.

https://www.cnblogs.com/yuemw/p/7908413.html
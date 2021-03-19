# C++ 内存模型

### 1. 原子操作：

​	所谓的**原子操作**，取的就是“原子是最小的、不可分割的最小个体”的意义，能够操作最接近机器的指令，这个和硬件相关了，虽然和硬件相关，但我们的Ｃ++11还是整合了这一切，让原子操作有了共同的调用接口。它表示在多个线程访问同一个全局资源的时候，能够确保所有其他的线程都不在同一时间内访问相同的资源。也就是他确保了在同一时刻只有唯一的线程对这个资源进行访问。这有点类似互斥对象对共享资源的访问的保护，但是原子操作更加接近底层，因而效率更高。

​	在以往的 C++ 标准中并没有对原子操作进行规定，我们往往是使用汇编语言，或者是借助第三方的线程库，例如intel的pthread来实现。在新标准 C++11 ，引入了原子操作的概念，并通过这个新的头文件提供了多种原子操作数据类型，例如，**atomic_bool**, **atomic_int**等等，如果我们在多个线程中对这些类型的共享资源进行操作，编译器将保证这些操作都是原子性的，也就是说，确保任意时刻只有一个线程对这个资源进行访问，编译器将保证，多个线程访问这个共享资源的正确性。从而避免了锁的使用，提高了效率。

#### 例：假若我们要设计一个广告点击统计程序，在服务器程序中，使用多个线程模拟多个用户对广告的点击

```c++
#include <boost/thread/thread.hpp>
#include <atomic> 
#include <iostream>
#include <time.h>

using namespace std;
// 全局的结果数据 
long total = 0; 

// 点击函数
void click()
{
    for(int i=0; i<1000000;++i)
    {
        // 对全局数据进行无锁访问 
        total ++;     
    }
}
 
 
int main(int argc, char* argv[])
{
    // 计时开始
    clock_t start = clock();
    // 创建100个线程模拟点击统计
    boost::thread_group threads;
    for(int i=0; i<100;++i) 
    {
        threads.create_thread(click);
    }

    threads.join_all();
    // 计时结束
    clock_t finish = clock();
    // 输出结果
    cout<<"result:"<<total<<endl;
    cout<<"duration:"<<finish -start<<"ms"<<endl;
    return 0;
}


```

这个例子的输出结果是：
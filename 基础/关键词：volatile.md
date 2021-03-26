## 关键词：volatile 

------

#### 1. 首先了解 C++ 内存模型（memory model）

<img src="/Users/admin/Library/Application Support/typora-user-images/image-20210308165157301.png" alt="image-20210308165157301" style="zoom:50%;" />

**线程**（Thread）中的访问都依靠主内存进行交互，即主内存中的共享变量用于线程之间的通信。但是可能会发生这样的情况：若线程中的共享变量已经改变，但未能及时更新主内存中的共享变量，因此当另一个线程要访问该线程的共享变量使取得的值不正确。这就是**“可见性问题”**。

#### 2. 可见性问题的解决方案

- 加锁（synchronizer)
- 使用关键词 **volatile**

（1）加锁

```c++
int main(){
	MyThread mythread = new MyThread();
	myThread.start();
	for(;;){
		synchronized(myThread){
			if(myThread.isFlag()){
				cout << "主进程访问到flag变量" << endl;
			}
		}
	}
}
```

加锁的作用：

​	1）主动清空本地内存

​	2）从主内存中拷贝共享变量

​	3）更新主内存中共享变量

因此，每次 thread 被访问时，都会进行上述操作，访问的线程再从主内存中获取该共享变量。

（2）使用关键词 **volatile**

在子线程中使用 **volatile** 修饰共享变量，每个线程要操作变量时会从主内存中拷贝到本地内存作为副本，当线程操作变量副本并协会主内存后，会通过 <u>CPU总线嗅探机制</u> 告知其他线程。


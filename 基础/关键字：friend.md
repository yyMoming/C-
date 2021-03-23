## 关键字：friend

------

### 1.  介绍

> C++ 中类的访问机制有三种：<font color='green'>`public`</font>、<font color='green'>`private`</font>、<font color='green'>`protected`</font>。<font color='green'>`private`</font>、<font color='green'>`protected`</font>不允许外部类或外部函数访问，特别是<font color='green'>`private`</font>，是不允许非类内成员访问的。

​	**友元** <font color='green'>`friend`</font>提供了一种普通函数、类访问另一个类中的私有<font color='green'>`private`</font>或保护<font color='green'>`protected`</font>机制的成员。虽然友元在一定程度上破坏了类的封装行和数据的透明性，但是却也提高了程序的运行效率。



### 2. 普通非成员友元函数

<font color='green'>`friend <类型><友元函数名>(<参数表>);`</font>

```c++
// xxx.cpp
class A{
    template<typename T>
	friend int get_private(const T& t);
public:
	A(int tmp):a(tmp){}
private:
	int a;
};
// yyy.h
template<class T>
int get_private(const T& t){
    return t.a;
};

int main(){
	A a(10);
	cout << get_private(a) << endl;
}
>>> 10
```

`类 A` 的类外普通函数 `get_private` 通过被 A在类内声明为<font color='green'>`friend`</font>，即可对类中任意数据成员进行访问。



### 3.  友元类 （友元关系是单向的）

<font color='green'>`friend class <类型名>;`</font>

> 类作为友元需要注意友元类和原始类之间的**相互依赖关系**，如果在友元类中定义的函数使用到了原始类的私有变量，那么就需要在 <u>友元类定义的文件中包含原始类定义的头文件</u> 。但是在原始类的定义中（包含友元类声明的那个类），就<u>不需要包含友元类的头文件</u>，也不需要在类定义前去声明友元类，因为友元类的声明自身就是一种声明（它指明可以在类外找到友元类）。

```c++
class A{
   friend class B；
public:
	A(int tmp):a(tmp){}
private:
	int a;
};

class B{
public:
	B(A tmp):a(tmp.a){}
    int get_B_a(){
        return a;
    }
private:
	int a;
};

int main() {
	A a(10);
	B b(a);
	cout << b.get_B_a() << endl;
}
>>> 10
```



### 4. 类成员函数作为友元函数

> 要类成员函数作为友元，在声明友元的时候要用**类限定符** `::`，所以必须先定义包含友元函数的类，但是在定义友元的函数时候，又必须事先定义原始类。通常的做法先声明包含友元函数的类，再定义原始类，这个顺序不能乱。

```c++
//B.h
#pragma once
class A; //先定义
class B
{
public:
	B(void);
	~B(void);
	int B::func(A xx)
    {
        return xx.mx * xx.my;
    }
};

//A.h
#pragma once
#include "B.h"
class A
{
friend int B::func(A xx);
public:
	A(void):mx(20),my(30){}
	~A(void){}
private:
	int mx;
	int my;
};

//main.cpp
#include "A.h"
#include "B.h"
#include <iostream>
using namespace std;
void main()
{
	A a;
	B b;
	cout<<b.func(a)<<endl;
	system("pause");
}
```



<font color='green'>`Tips`</font> 

- 友元关系没有继承性 假如类B是类A的友元，类C继承于类A，那么友元类B是没办法直接访问类C的私有或保护成员。

- 友元关系没有传递性 假如类B是类A的友元，类C是类B的友元，那么友元类C是没办法直接访问类A的私有或保护成员，也就是不存在“友元的友元”这种关系。

- 如果想要指定两个类都有成员函数作为对方的友元，那么必须第二个类是第一个类的友元。

  ```c++
  //A.h
  #pragma once
   
  // class B is a friend class of A
  class A
  {
  	friend class B;
  public:
  	A(void):ma(10),mb(20){}
  	int funa(B& b);
  private:
  	int	ma;
  	int	mb;
  };
  
  //B.h
  #pragma once
  #include "A.h"
   
   
  // A's function funa is a friend function of B
  class B
  {
  	friend int A::funa(B& b);
  public:
  	B(void);
  	int funb(A& a);
  	int func(A& a);
  private:
  	int mx;
  	int my;
  };
  
  //A.cpp
  #include "A.h"
  #include "B.h"
  
  int A::funa(B& b)
  {
  	return  b.mx * b.my;
  }
  
  //B.cpp
  #include "B.h"
   
  B::B(void):mx(12),my(15)
  {
  }
   
   
  B::~B(void)
  {
  }
   
   
  int B::funb(A& a)
  {
  	return a.ma + a.mb;
  }
   
  int B::func(A& a)
  {
  	return a.ma * a.mb;
  }
  ```

  


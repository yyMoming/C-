# 关键字：namespace

------

## 1.  介绍

>在c++中，符号常量、变量、函数、结构、枚举、类和对象等等都有自己的`名字`。工程越大，名称互相冲突性的可能性越大，为了避免在大规模程序的设计中，以及在程序员使用各种各样的C++库时，这些标识符的命名发生冲突，标准C++引入关键字namespace（命名空间/名字空间/名称空间），可以更好地控制标识符的作用域。



## 2. 使用

##### 	1）定义：在全局定义域范围内定义

```c++
namespace A{
	int A_a = 1;
}
namespace B{
	int B_b = 2;
}
int main(){
	cout << "A-a：" << A::A_a << endl;
	cout << "B-b：" << B::B_b << endl;
}
```

> 在函数内部或类内部不能定义命名空间，`局部定义命名空间是错误的`

```c++
void test(){
	namespace A{ //错误， 函数内不能定义命名空间
        int A_a = 1;
    }
    namespace B{
        int B_b = 2;
    }
    cout << "A-a：" << A::A_a << endl;
    cout << "B-b：" << B::B_b << endl;
}
```



##### 	2）命名空间可以嵌套

```python
namespace A{
	int A_a = 1;
	namespace B{
        int B_b = 2;
    }
}

int main(){
	cout << "A-a：" << A::A_a << endl;
	cout << "A-B-b：" << A::B::B_b << endl;
}
```



##### 	3) 命名空间的基本使用

> 随时可以把新成员添加到已有的命名空间中

```c++
// file1.h
namespace A {
    int a = 100;
    int b = 200;
}

// file2.h
namespace A {
    int c = 300;
}

int main(){
	cout << "A-a：" << A::A_a << endl;
    cout << "A-b：" << A::A_b << endl;
    cout << "A-c：" << A::A_c << endl;
}
```

> 命名空间可以存放`变量`和`函数`，命名空间中的函数可以在 “命名空间” 外定义。

```c++
namespace A {
    int a=100; //变量
    void func(); //函数
}
void A::func(); //函数
{
	cout << "A::func()" << endl;
}
```

> 匿名命名空间，意味着命名空间中的标识符只能在文件中访问

```c++
namespace{
    int a = 10;
    void func(){
        cout<<"hello namespace"<<endl;
    }
}
int main(){
	cout << a << endl;
	func();
}
```


# 关键字：struct

### 1. struct介绍

- C 中的 struct

  > - 在C中struct只单纯的用作数据的复合类型，也就是说，在结构体声明中只能将数据成员放在里面，而不能将函数放在里面。
  >
  > - 在C结构体声明中不能使用C++访问修饰符，如：`public`、`protected`、`private` 而在C++中可以使用。
  > - 在C中定义结构体变量，如果使用了下面定义必须加struct。struct base xx;
  > - C的结构体不能继承（没有这一概念）。
  > - 若结构体的名字与函数名相同，可以正常运行且正常的调用！例如：可以定义与 struct Base 不冲突的 void Base() {}。

- C++ 中的 struct

  > - C++结构体中不仅可以定义数据，还可以定义函数。
  > - C++结构体中可以使用访问修饰符。
  > - C++结构体使用可以直接使用不带struct。
  > - C++可继承

> 情形1：不使用 `typedef` 定义结构体别名

​	1）未添加同名函数时：

```c++
struct Student {

};
Struct Student s; //ok
Student s;  //ok
```

​	2）包含同名函数时

```c++
struct Student {

};
Student(){}
Struct Student s; //ok
Student s;  //error
```

>情形2：使用 `typedef` 定义结构体别名

```c++
typedef struct Base1 {         
    int v1;
//    private:   //error!
        int v3;
    public:     //显示声明public
        int v2;
    void print(){       
        printf("%s\n","hello world");
    };    
}B;
void B() {}  //error! 符号 "B" 已经被定义为一个 "struct Base1" 的别名
```



### 2.C++ 中的 struct

> C++ 中的 struct 对 C 中的 struct 进行了扩充，它已经不再只是一个包含不同数据类型的数据结构了，它已经获取了太多的功能。
>
> - struct 能包含成员函数了
> - struct 能继承了
> - struct 能实现多态了

> class 和 struct 的本质区别是：**默认的继承访问权限**，**struct是public的，class是private的。**
>
> - “**class”这个关键字还用于定义模板参数，就像“typename**”。
> - **没有构造函数或虚函数的 struct 可以在定义的时候用{}赋初值。**
>   （**因为加入这样的函数，使得类的内部结构发生了变化。而加入一个普通的成员函数呢？你会发现{}依旧可用。其实你可以将普通的函数理解成对数据结构的一种算法，这并不打破它数据结构的特性。**）

```c++
struct A{}；class B : A{}; //private继承
struct C : B{}； //public继承
```

```c++
struct A //定义一个struct
{
   char c1;
   int n2;
   double db3;
};
A a={'p', 7, 3.1415926}; //定义时直接赋值
```


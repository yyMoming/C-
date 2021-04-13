# 关键字：mutable

### 1. Mutable 

​	1）`mutable` 的中文意思是 “可变的，易变的”，跟 `constant`是反义词。C++中为了突破 <font color='green'>`const`</font> 的限制涉及了<font color='green'>`mutable`</font>。被 <font color='green'>`mutable`</font> 修饰的变量，将永远处于可变的状态，即使是在 <font color='green'>`const`</font> 函数中。

​	2）C++ 中，不可变的变量，称为常量，使用 <font color='green'>`const  `</font> 来修饰。但通常将 <font color='green'>`const`</font> 和 <font color='green'>`mutable`</font> 联用，会让人疑惑，到底可变还是不可变。

> 事实上，<font color='green'>`mutable`</font> 是用来修饰一个 <font color='green'>`const`</font> 实例的部分可变成员的。
>
> - 二进制层面的 <font color='green'>`const`</font> ，就是绝对常量，在任何情况下不可修改（除了使用<font color='green'>`const_cast`</font>。
> - 引入了 <font color='green'>`mutable`</font> 之后，C++ 可以有逻辑层面的 <font color='green'>`const`</font>，即对于一个常量实例来说，外部观察它是常量不可修改，但内部仍然可以存在非常量数据成员。

<font color='orange'>`Example 1`</font>

```c++
struct Test
{
    int a;
    mutable int b;
};

const struct Test test = {1,2};
test.a = 10;  # 编译错误
test.b = 20;  # 允许访问
```

> Note：<font color='green'>`mutable`</font> 在类中只能修饰非静态数据成员。



<font color='orange'>`Example 2`</font>

```c++
class Test
{
private:
    int a;
    mutable int b;
public:

    void func2() const 
    {
        a++;  # 编译错误，不允许修改a
        b++;  # mutable修饰后允许修改
    }
}
```

> Note：
>
> - <font color='green'>`const`</font> 承诺的是一旦某个变量被其修饰，那么只要不使用强制转换 （<font color='green'>`const_cast`</font>），在任何情况下该变量的值都不会被改变，无论有意还是无意，而被 <font color='green'>`const`</font> 修饰的函数也一样，一旦某个函数被 <font color='green'>`const`</font> 修饰，那么它便不能直接或间接改变任何函数体以外的变量的值，即使是调用一个可能造成这种改变的函数都不行。这种承诺在语法上也作出严格的保证，任何可能违反这种承诺的行为都会被编译器检查出来。
> - <font color='green'>`mutable`</font> 的承诺是如果某个变量被其修饰，那么这个变量将永远处于可变的状态，即使在一个 <font color='green'>`const`</font> 函数中。这与 <font color='green'>`const`</font> 形成了一个对称的定义，一个永远不变，而另外一个是永远可变。



### 2. Lambda 表达式中的 `mutable`

​	1) C++11 引入了 <font color='green'>`Lambda`</font> 表达式，可以凭此创建匿名函数。

> 在 <font color='green'>`Lambda`</font> 表达式的设计中，捕获变量有几种方式；其中按值捕获（Caputre by Value）的方式不允许程序员在 <font color='green'>`Lambda`</font> 函数的函数体中修改捕获的变量。而以 <font color='green'>`mutable`</font> 修饰 <font color='green'>`Lambda`</font> 函数，则可以打破这种限制。

```c++
int x{0};
auto f1 = [=]() mutable {x = 42;};  // okay, 创建了一个函数类型的实例
auto f2 = [=]()         {x = 42;};  // error, 不允许修改按值捕获的外部变量的值
```


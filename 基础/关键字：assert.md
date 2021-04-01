# 关键字：assert

## 1. 介绍 assert

> 断言，是宏而不是一个函数。<font color='green'>`assert`</font>宏的原型定义在<font color='green'>`C/C++`</font>中，作用是如果它的条件使错误的，则立即终止程序。可以通过定义 <font color='green'>`NDEBUG`</font>来关闭 <font color='green'>`assert`</font> ，但是需要在源代码的开头，即使用<font color='green'>`include`</font>引用包之前。

```c++
#define NDEBUG
```

```c++
#include <assert.h>
void assert(int expression);
```

> <font color='green'>`assert`</font>通过计算表达式<font color='green'>`expression`</font>，若表达式为假，则通过<font color='green'>`abort`</font>来终止程序。
>
> 使用<font color='green'>`assert`</font>的缺点是，频繁的调用会极大影响程序的性能，增加额外的开销。



## 2. assert的用法和注意事项

- #### 用法

  -  在函数开始处检验传入参数的合法性
  - 每个assert只检验一个条件,因为同时检验多个条件时,如果断言失败,无法直观的判断是哪个条件失败
  - 不能使用改变环境的语句,因为assert只在DEBUG个生效,如果这么做,会使用程序在真正运行时遇到问题
  - assert和后面的语句应空一行,以形成逻辑和视觉上的一致感

- #### 原则

  - 使用断言捕捉不应该发生的非法情况。不要混淆非法情况与错误情况之间的区别，后者是必然存在的并且是一定要作出处理的。
  - 使用断言对函数的参数进行确认。
  - ASSERT 只有在 Debug 版本中才有效，如果编译为 Release 版本则被忽略。
# 关键词：Lambda

><font color='green'>`lambda`</font> 是字面上定义一个函数而不是持有一个函数。也就是我们可以使用函数指针的场景是可以使用 <font color='green'>`lambda`</font> 来代替的。

### 1. 基本语法

> 简单来说，Lambda函数也就是一个函数，它的语法定义如下：

```c++
[capture](parameters) mutable ->return-type{statement}
```

​	1.<font color='green'>`[capture]`</font>：捕捉列表。捕捉列表总是出现在Lambda函数的开始处。实际上，[]是Lambda引出符。编译器根据该引出符判断接下来的代码是否是Lambda函数。捕捉列表能够捕捉上下文中的变量以供Lambda函数使用;

​	2.<font color='green'>`(parameters)`</font>：参数列表。与普通函数的参数列表一致。如果不需要参数传递，则可以连同括号“()”一起省略;

​	3.<font color='green'>`mutable`</font>：mutable修饰符。默认情况下，Lambda函数总是一个const函数，mutable可以取消其常量性。在使用该修饰符时，参数列表不可省略（即使参数为空）;

​	4.<font color='green'>`->return-type`</font>：返回类型。用追踪返回类型形式声明函数的返回类型。我们可以在不需要返回值的时候也可以连同符号”->”一起省略。此外，在返回类型明确的情况下，也可以省略该部分，让编译器对返回类型进行推导;

​	5.<font color='green'>`{statement}`</font>：函数体。内容与普通函数一样，不过除了可以使用参数之外，还可以使用所有捕获的变量。
## 关键字：constexpr

### 1.  constexpr：C++ 11中新增加的，用于指示常量表达式的关键字

☆ **const** 和 **constexpr** 都可以用来**修饰对象和函数**

- **const** 和 **constexpr** 的区别：

  - **const** 修饰一个<u>对象表示它是常量</u>：对象一经初始化后就不会再变动。

    - const 只能用于非静态成员的函数而不是所有函数，它保证成员函数不修改任何非静态数据。

  - **constexpr** 修饰一个<u>常量表达式</u> 

    - constexpr 可以用于含参和无参函数。constexpr 函数适用于常量表达式，只有在下面的情况下编译器才会接受 constexpr 函数：

      1）函数体必须足够简单，除了typedef和静态元素，只允许有return语句。如构造函数只能有初始化列表，typedef和静态元素 (实际上在C++14标准中已经允许定义语句存在于 constexpr 函数体内了)

      2）参数和返回值必须是字面值类型

  

### 2. 常量表达式

- 必须在编译阶段可以被识别。

  ```c++
  template <int N>
  class fixed_size_list
  { /*...*/ };
  
  fixed_size_list<X> mylist;     // <-- X必须是字面值类型
  
  int numbers[X];   // <-- X必须是字面值类型
  ```

  <font color="green">**Tips:**</font> 字面值类型包括 算数类型（整形和浮点型）、引用和指针。某些自定义的类也属于字面值类型。
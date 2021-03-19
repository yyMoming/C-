## 关键字：union

#### 1. union（联合）：是一种特殊的类，一个union可以有多个数据成员

```c++
union Token{
   char cval;
   int ival;
   double dval;
};
// 定义了一个名为 Token 的联合，该联合包含 3 个数据成员
```

- 特点：
  - 默认访问控制符为 public
  - 可以含有构造函数、析构函数
  - 不能含有引用类型的成员
  - 不能继承自其他类，不能作为基类
  - 不能含有虚函数
  - 匿名 union 在定义所在作用域可直接访问 union 成员
  - 匿名 union 不能包含 protected 成员或 private 成员
  - 全局匿名联合必须是静态（static）的

#### 2. union的作用

- 互斥赋值：在任意时刻，`联合中只能有一个数据成员可以有值`。当给联合中某个成员赋值之后，该联合中的其它成员就变成**未定义状态**了。

> 初始化

![20180626155345986](/Users/admin/Documents/ywm_info/C++/C-/基础/images/20180626155345986.png)

红色方框内的数据即为token所占内存数据。<u>因为token中长度最大的变量是double类型，所以token的长度是8个字节</u>。



> cval 赋值

```c++
token.cval = 'a'; // 为token赋值
```

![](/Users/admin/Documents/ywm_info/C++/C-/基础/images/cval.png)

此时，token所占内存的第一个字节的值变为0x61，即字符’a’。



> ival 赋值

![](/Users/admin/Documents/ywm_info/C++/C-/基础/images/ival.png)

此时，token所占内存的前四个字节变为0x00000001，即为数字1。在对token的ival赋值之后，cval的值就变为了0x01，实际上就没有意义了。



- 为成员指定长度

```c++
union U {
 
unsigned short int aa;
 
struct {
 
unsigned int bb : 7;//(bit 0-6)
 
unsigned int cc : 6;//(bit 7-12)
 
unsigned int dd : 3;//(bit 13-15)
 
};
 
} u;
```

以上代码定义了一个名为U的联合，并且定义了U的变量u。联合U包含两个成员，一个是unsigned short int类型的变量，其大小为2个字节；另一个是一个自定义结构，该自定义结构中包含了3个unsigned int类型的变量。需要注意的是，每个unsigned int类型的变量的大小并不是默认的4个字节，而是通过冒号操作符指定了其大小，该大小的单位是比特。所以，联合u的大小是2个字节。

> 之后，对联合u中的aa进行赋值u

![](/Users/admin/Documents/ywm_info/C++/C-/基础/images/u.png)

此时，u.bb所处的位置是0-6比特；u.cc所处的位置是7-12比特；u.dd所处的位置是13-15比特

![](/Users/admin/Documents/ywm_info/C++/C-/基础/images/bbddcc.png)
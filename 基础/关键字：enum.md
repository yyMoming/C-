## 关键字：enum

------

1. ### 枚举 enum 的定义

   枚举 **enum** 是一个类型（class），可以保存一组由用户刻画的值。

1) **enum** 的具体定义：enum <类型名> {<枚举常量表>};

- 关键字 **enum** —— 指明其后的标识符是一个枚举类型的名字。
- 枚举常量表 —— 由枚举常量构成。"*枚举常量*"或称"*枚举成员*"，是以标识符形式表示的**整型量**，表示枚举类型的取值。枚举常量表列出枚举类型的所有取值，各枚举常量之间以"，"间隔，且必须各不相同。取值类型与条件表达式相同。

```c++
# enum的具体定义方法
enum my_enum
{
    my_enum1 = 0, // 注意此处是逗号
    my_enum2,
};
enum week {Sun=7, Mon=1, Tue, Wed, Thu, Fri, Sat};
//枚举常量Sun,Mon,Tue,Wed,Thu,Fri,Sat的值分别为7、1、2、3、4、5、6。
```

2）匿名方式定义 enum

```c++
enum
{
    my_enum1 = 0,
    my_enum2,
};
```

3) 与 typedef 结合使用

```c++
typedef enum 
{
    my_enum1 = 0,
    my_enum2,
}my_enum;
```



2. ### enum 关键字的特点

   - 全局作用域：**enum** 关键字的作用域是全局的，如果在enum A中声明了一个枚举类型 my)enum3，则无法在enum B中声明同样的枚举类型，如

     ```C++
     enum A
     {
         my_enum3 = 0,
     };
     
     enum B
     {
         my_enum3 = 0,
     };
     ```

     但是如果二者不在一个作用域里可以使用。

     ```c++
     enum A
     {
         my_enum3 = 0,
     };
     int main(){
     	enum B
         {
         	my_enum3 = 20,
         };
         cout << my_enum3 << endl;
     }
     >>> 20
     ```

     

   - 取值特点：枚举常量代表该枚举类型的变量可能取的值，编译系统为每个枚举常量指定一个整数值，默认状态下，这个整数就是所列举元素的序号，序号从0开始。可以在定义枚举类型时为部分或全部枚举常量指定整数值，在指定值之前的枚举常量仍按默认方式取值，而指定值之后的枚举常量按依次加1的原则取值。 各枚举常量的值可以重复。（<font color='red'>**枚举类型不能是字符常量，也不能是整型常量**</font>）

     ```c++
     enum letter_set {'a','d','F','s','T'}; //枚举常量不能是字符常量
     enum year_set{2000,2001,2002,2003,2004,2005}; //枚举常量不能是整型常量
     ```

   - 隐式转换为其他类型

     ```c++
     enum my_enum
     {
         my_enum1 = 0,
         my_enum2,
     
     };
     
     int my_int = my_enum1;
     ```

   - 传统 **enum** 无法指定底层所用的数据类型

     

   

3. ### enum 的相关操作

   - 枚举变量的使用

     ```c++
     enum my_enum
     {
         my_enum1 = 0,
         my_enum2,
     
     };
     my_enum m1, m2;
     ```

   - 赋值操作

     ```c++
     cout << m1 << endl; // 结果是0
     m1 = my_enum; // 将枚举常量值赋给枚举变量
     cout << m1 << endl; // 结果是1
     int i = m1; // 给整形变量赋值
     ```

   - 运算：==、<、>、<=、>=、!=

     ```c++
     //比较同类型枚举变量m1，m2是否相等
     if (m1==m2) cout<<"相等"；
     //输出的是变量color3与WHITE的比较结果，结果为1
     cout<< (m1 < my_enum2);
     ```

     

4. ### C++ 11 中 enum  class 和 enum struct

   ```c++
   enum class A
   {
       my_enum3 = 0,
   };
   
   enum class B
   {
       my_enum3 = 0,
   };
   
   enum C
   {
       my_enum3 = 0,
   };
   ```

   - **enum class**（与enum struct等同）解决了传统 enum 关键字作用域问题（<u>不能重复定义枚举常量表</u>）

     ```c++
     A a = A::my_enum3;
     B b = B::my_enum3;
     ```

   - **enum class** 可以指定底层数据类型

     ```
     enum class A: int /** 每个枚举都是int类型的 */
     {
         num = 0,
     };
     
     enum class B: unsigned char /** 每个枚举都是unsigned char类型的 */
     {
         num = 0,
     };
     ```

   

   5. ### 类中的枚举类型

      当希望某些常量只能在类中有效。由于 #define 定义的宏常量是全局的，不能达到目的，于是可以采用 const 关键字来修饰数据成员。**然而，const 数据成员只是在某个对象生存期内是常量，而对于整个类而言是可变的，因为类可以创建多个对象，不同的对象的 const 数据成员值是可以不一样的。**

      **<u>在C++ 11标准前，不能在类内声明中初始化 const 数据成员。</u>**

      若要在类内建立恒定的常量可以使用枚举常量来实现。

      ```c++
      class Person{
      public:
          typedef enum {
              BOY = 0,
              GIRL
          }SexType;
      };
      ```

      ### <font color='green'>Tips：</font>1）枚举常量不会占用对象的存储空间，在编译时会被全部求值。

      ### 			2）枚举常量的隐含数据类型是整数，最大值有限， 不能表示浮点数。
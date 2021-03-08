### 关键词：Const

------

1. #### const 含义

   ###### 将一个变量变成常量，使用类型修饰符 **const **说明的变量或对象的值是不能被更新的。

   实际上，被 **const** 修饰的值，只会在编译器保证其不被修改。程序运行时，通过特定方法是可以修改值的。

2. #### const 作用

   ###### （1）定义常量：定义常量后不能修改，且必须初始化

   ```c++
   const int a = 100; // 例1
   // 或者:
   int a = 100;
   const int b = a; //正确，例2
   b = 20; //错误,b不可以再被改变
   ```

   ☆ 对于例1，在编译时，编译器发现 $a$ 是**真常量**，所以会把后面使用到 $a$ 的地方直接进行替换，起到了 **$\#define$** 的作用。

   ###### （2）检查类型
   
   **const** 常量与被 **$\#define$** 宏定义常量的区别：
   
   - **const** 常量具有类型，编译器可以进行安全检查
   - **$\#define$** 宏定义没有数据类型，只是简单的字符串替换，不能进行安全检查
   - **const** 定义的变量只有类型为整数或枚举，且以常量表达式（就是表达式里面只有常量的式子）初始化时才能作为常量表达式，其他情况下知识一个 **const** 限定的变量，不一定是常量。
   
   ```
   PS: 通常来说，尽量以 const，enum，inline 替换 #define。  ————《Effective C++》
   PS：常量表达式是指值不会改变并且在编译过程就能得到计算结果的表达式。C++11 后允许将变量声明为 constexpr 类型以便由编译器来验证变量是否是一个常量表达式，声明为 constexpr 的变量一定是一个常量，必须用常量表达式初始化。
   ```
   
   ###### （3） 防止被程序修改，增强程序的健壮性
   
   ```c++
   void f(const int i){
       i = 100; //错误，const常量不能被改变
   }
   ```
   
   ###### （4）节省空间，避免不必要的内存分配
   
   **const** 定义常量从汇编角度来看，只是给出了对应的内存地址，而不像是 **$\#define$** 给出的是立即数（通常把在[立即寻址方式](https://baike.baidu.com/item/立即寻址方式/6686429)指令中给出的数称为立即数）。所以 const 定义的常量在程序运行过程中只有一份拷贝，而  **$\#define$** 定义的常量在内存中有若干个拷贝。

3. #### const 对象默认为文件局部变量

   **<font color="red">Note： </font>**非 **const** 变量默认为 $extern$。要使 **const** 变量能够在其他文件中被访问，必须在文件中显式地指定为 $extern$。

   ###### (1) 非 const 变量在不同文件之间的访问

   ```c++
   // file1.cpp
   int ext;
   // file2.cpp
   #include "file1.cpp"
   #include <iostream>
   using namespace std;
   extern int ext;
   int main(){
   	cout << ext << endl;
   }
   ```

   ###### (2) const 变量在不同文件之间访问

   ```c++
   // file1.cpp
   const int ext = 100; // extern const int ext = 100;也可以
   // file2.cpp
   #include "file1.cpp"
   #include <iostream>
   using namespace std;
   extern const int ext;
   int main(){
   	cout << ext << endl;
   }
   ```

   在 file1 中，**const** 常量必须要初始化，可以使用 $extern$ ，也可以不使用。而如果 file1 中只是普通变量，则不能使用  $extern$，可以不初始化（也可以初始化）。

4. #### 指针与 const

   ###### (1) 指向常量的指针

   ```c++
   const int *a; // 可以不被初始化，指针a不是常量，但*a即a指向的对象是常量
   int b = 20;
   a = &b; //正确
   *a = 20; //错误，*a是常量，不可更改，尽管原来b不是const常量
   ```

   <font color='green'>int</font> <font color='purple'>const</font> <font color='green'>*</font>a;

   其中 a 与上述代码 <font color='green'>const</font> <font color='purple'>int</font> <font color='green'>*</font>a; 中的a一致。

   ###### (2) 指针常量：必须初始化，且指针的值不能被修改，但指针指向的对象可以被修改

   ```c++
   int b = 20;
   int * const a = &b; // 必须被初始化
   int c = 200;
   a = &c; // 错误，指针常量不可以被修改
   *a = 200; // 正确，常量指针指向的对象不是const常量
   ```

   ###### (3) 指向 const 常量的指针常量

   ```c++
   const int p = 20;
   const int * const ptr = &p; // ptr所指向的对象必须是常量，且 pre必须被初始化
   ```

5. #### 函数中使用 const

   ###### (1) const 修饰函数返回值

   ```c++
   const int func1(); // 函数返回值的副本变为const常量，接收的对象也应该为const 常量
   const int * func2(); // 指针指向内容不可变
   int* const func3(); // 指针不可变
   ```

   ###### (2) const 修饰函数参数

   ```c++
   void func1(const int var) // var不可变，无意义
   void func2(int *const var) // 指针不可变，无意义
   void func3(const int *var) // 指针所指内容不可变
   void func4(const A& a) // 使用引用增加效率，且防止修改
   ```

6. #### 类中使用 const

   ###### (1) const 修饰类成员函数

   ```c++
   class people{
   	private:
   	 	string gender;
   	public:
   		people(int a):age(a);
   		const int age;
     	void get_age() const;
      	void set_gender(string g);
   }
   
   int main(){
     people p1(30);
     const people p2(20);
     p1.get_age(); // 正确，不会改变对象成员信息
     p1.set_gender("male"); // 正确，非const成员函数可以改变对象成员信息
     p2.set_gender("female"); // 错误，const对象成员信息不能被改变
   }
   ```

   - 类中 const 成员变量只能通过初始化列表进行初始化
   - const 对象只能访问 const 成员函数，非 const 对象可以访问任何成员函数

7. #### const 与 static 结合

   ```c++
   static const int age; // 类内定义
   const int people::age = 20; // 类外初始化
   static const int age = 10; // C++11可以在类内初始化
   ```

   
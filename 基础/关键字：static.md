## 关键字：static

1. ### 与不同类型一起使用时，static 拥有不同的含义

   - 静态变量
     - 静态局部变量（面向过程）
     - 静态全局变量（面向过程）
     - 静态成员变量（面向对象）
   - 静态函数
     - 静态成员函数（面向对象）
     - 静态函数（面向过程）

   

2. ### static：静态变量

   - 函数中的静态变量 —— 静态局部变量（面向过程）

     1）静态局部变量在 **全局数据区分配内存**（C++ 内存包括四部分：代码区，全局数据区，栈区和堆区）

     2）静态局部变量在**程序执行到该对象的声明处时第一次被初始化**，以后的函数调用将不再初始化

     3）静态局部变量一般在声明处初始化，如果没有显式初始化，会被程序自动初始化为0

     4）静态局部变量**始终保存在全局数据区直到程序结束**。但作用域只为局部作用域，当定义它的函数或语句块结束时，其作用域随之结束。

     ```c++
     void test01() {
     	static int num = 0;
     	cout << "num:" << num << endl;
     	num++;
     }
     int main()
     {
     	test01();
     	test01();
     	test01();
     }
     >>> num: 0
     >>> num: 1
     >>> num: 2
     ```

   - 静态全局变量（面向过程）

     1）该变量在 **全局数据区** 分配内存

     2）未经初始化的 **静态全局变量** 会被程序自动初始化为0

     3）静态全局变量在 **声明它的整个文件内都是可见的** ，在文件之外不可见，也不能被其他文件所用，其他文件中可以定义相同名字的变量，不会发生冲突。

     ```c++
     //Example 2
     //File1
     #include <iostream.h> 
     
     void fn();
     static int n; //定义静态全局变量 
     
     void main()
     {
     	n=20;
     	cout<<n<<endl;
     	fn();
     } 
     
     //File2 
     #include <iostream.h> 
     extern int n; // 错误，静态全局变量不能被其他文件所用
     void fn()
     {
     	n++;
     	cout<<n<<endl;
     } 
     ```

   - 静态成员变量（面向对象）

     1）静态成员变量是该类的 **所有对象所共有** 的。对于普通成员变量，每个类对象都有自己的一份拷贝。而静态成员变量一共就一份，无论这个类的对象被定义了多少个，静态成员变量只分配一次内存，由该类的所有对象共享访问。所以，静态数据成员的值对每个对象都是一样的，它的值可以更新

     2）**数据共享**：静态数据成员在全局数据区分配内存，由本类的所有对象共享，所以，它**不属于特定的类对象，不占用对象的内存**，而是在所有对象之外开辟内存，在没有产生类对象时其作用域就可见。因此，**在没有类的实例存在时，静态成员变量就已经存在**，我们就可以操作它

     3）**类内类外初始化**：静态成员变量存储在全局数据区。**静态成员变量必须初始化**，而且只能在**类体外**进行。否则，编译能通过，链接不能通过。但是 static 成员常量可以类内初始化（static const int s = 20;）

     4) **编译分配内存，运行结束时释放**：static 成员变量和普通 static 变量一样，**编译时**在静态数据区分配内存，**程序结束时释放**。

     5）静态数据成员初始化与一般数据成员初始化不同。初始化时可以不加 static，但必须要有数据类型。被 private、protected、public 修饰的 static 成员变量都可以用这种方式初始化。静态数据成员初始化的格式为：
     $$
     ＜数据类型＞＜类名＞::＜静态数据成员名＞=＜值＞
     $$
     6）**类的静态成员变量访问形式**：

     - $$
       ＜类对象名＞.＜静态数据成员名＞
       $$

       

     - $$
       ＜类类型名＞::＜静态数据成员名＞
       $$

       

     7）**遵循一般的访问规则**：静态数据成员和普通数据成员一样遵从public,protected,private访问规则

     ```c++
     // 普通对象
     class Apple 
     { 
         int i; 
         public: 
             Apple() 
             { 
                 i = 0; 
                 cout << "Inside Constructor\n"; 
             } 
             ~Apple() 
             { 
                 cout << "Inside Destructor\n"; 
             } 
     }; 
     
     int main() 
     { 
         int x = 0; 
         if (x==0) 
         { 
             Apple obj; 
         } 
         cout << "End of main\n"; 
     } 
     // 输出
     >>> Inside Constructor
     >>> Inside Destructor
     >>> End of main
     ```

     ```c++
     // 静态对象
     class Apple 
     { 
         int i; 
         public: 
             Apple() 
             { 
                 i = 0; 
                 cout << "Inside Constructor\n"; 
             } 
             ~Apple() 
             { 
                 cout << "Inside Destructor\n"; 
             } 
     }; 
     
     int main() 
     { 
         int x = 0; 
         if (x==0) 
         { 
             static Apple obj; 
         } 
         cout << "End of main\n"; 
     } 
     // 输出
     >>> Inside Constructor
     >>> End of main
     >>> Inside Destructor
     ```

     

3. ### static：静态函数

   - 类中的静态函数（面向对象）

   1）静态成员函数与静态成员变量一样，都是为 **类** 服务，而不是某一个类的具体对象服务。

   2）**没有 this 指针，不能访问非静态成员**：普通成员函数一般都隐含了 **“this”** 指针，**this** 指针指向类的对象，因为普通成员函数总是具体的属于某一个具体对象，当函数被调用时，系统会吧当前对象的起始地址赋给 **this** 指针。与普通函数相比，静态成员函数属于类本身，不属于某个具体的实例对象，因此不具有 **this** 指针。也正因为没有指向具体对象，因此**无法访问**属于某一对象的非静态成员变量和非静态成员函数。

   3）出现在类体外的函数定义不能指定关键字 **static**

   4）**非静态成员函数可以任意的访问**静态成员函数和静态数据成员

   5）由于没有this指针的额外开销，静态成员函数与类的全局函数相比速度上会稍快

   6）静态成员函数的调用

   ​		类名调用
   $$
   ＜类名＞::＜静态成员函数名＞
   $$
   ​		类成员调用：操作符（.）和（->）

   ```c++
   
   #include <iostream>
   using namespace std;
   
   class Student{
   private:
      char *name;
      int age;
      float score;
      static int num;  	//学生人数
      static float total;  //总分
   public:
      Student(char *, int, float);
      void say();
      static float getAverage();  //静态成员函数，用来获得平均成绩
   };
   
   int Student::num = 0;
   float Student::total = 0;
   
   Student::Student(char *name, int age, float score)
   {
      this->name = name;
      this->age = age;
      this->score = score;
      num++;
      total += score;
   }
   
   void Student::say()
   {
      cout<<name<<"的年龄是 "<<age<<"，成绩是 "<<score<<"（当前共"<<num<<"名学生）"<<endl;
   }
   
   float Student::getAverage()
   {
      return total / num;
   }
   
   int main()
   {
      (new Student("小明", 15, 90))->say();
      (new Student("李磊", 16, 80))->say();
      (new Student("张华", 16, 99))->say();
      (new Student("王康", 14, 60))->say();
      cout<<"平均成绩为 "<<Student::getAverage()<<endl;
      return 0;
   }
   
   运行结果：
   小明的年龄是 15，成绩是 90（当前共1名学生）
   李磊的年龄是 16，成绩是 80（当前共2名学生）
   张华的年龄是 16，成绩是 99（当前共3名学生）
   王康的年龄是 14，成绩是 60（当前共4名学生）
   平均成绩为 82.25
   ```

   **<font color='green'>Tips：</font>**在使用包含静态成员的类时，有时候会调用拷贝构造函数生成临时的隐藏的类对象，而这个临时对象在消亡时会调用析构函数有可能会对静态变量做操作（例如total_num--），可是这些对象在生成时却没有执行构造函数中的total_num++的操作。解决方案是为这个类写一个拷贝构造函数，在该拷贝构造函数中完成total_num++的操作。

- ​	普通静态函数（面向过程）

  1）在函数的返回类型前加上 static 关键字,函数即被定义为静态函数。静态函数与普通函数不同，它只能在声明它的文件当中可见，不能被其它文件使用。

  2）其它文件中可以定义相同名字的函数，不会发生冲突；

  ```c++
  #include <iostream.h>
  static void fn();//声明静态函数
  
  void main()
  {
  	fn();
  }
  
  void fn()//定义静态函数
  {
  	int n=10;
  	cout<<n<<endl;
  }
  ```

  




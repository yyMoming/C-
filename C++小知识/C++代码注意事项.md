# C++代码注意事项

1. ### C++编译错误error: expected identifier before numeric constant

   ```cpp
   #include<iostream>
   #include<vector>
   #include<queue>
   using namespace std;
   
   class Solution
   {
   private:
       vector<int> c(128,0);  //编译报错
       queue<char> q;
   };
   
   int main(int argc,char **argv)
   {
       Solution sol;
       return 0;
   }
   ```

   >原因是：在c++的类体中，方法以外的区域不允许有初始化，简单类型是可以的（例如int等简单类型变量，以及静态成员变量），但是有构造函数的复杂对象就不可以了（例如vector）。
    vector有显示的构造函数，因此一个函数强制转换来调用其构造函数。在上述代码vector<int> c也可以理解成类的成员对象，类的成员对象初始化必须在类的构造函数中初始化。



2. ### multiple definition of `xxxx`问题解决及其原理

> 查看代码，确实我在一个.h文件中定义了一个变量，而这个.h文件被多个文件包含，单独编译都没有问题，但是到链接的时候就出现问题了。
>
> **一般在.h文件中定义一个变量声明时，在其他文件中只要包含了这个.h文件，编译的时候就会独立被编译器解释，然后每个.C文件会生成独立的标识符和符号表，所以上述代码在单独编译的时候并不会报错，语法是合法的**。**但是，最后在编译器链接的时候，就会将工程中所有的符号整合在一起，由于文件中有重复的变量，于是就会出现重复定义的错误,系统就是提示你“multiple definition of `xxx`”。
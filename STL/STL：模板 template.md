## STL：模板 template

------

1. ### 模板技术：类型参数化，编写代码可以忽略类型

   ```c++
   template<typename T> // 告诉编译器，这是模板函数
   void MySwap(T& a, T& b){
   	T temp = a;
   	a = b;
   	b = a;
   }
   
   ```

   (1) 可以进行自动类型推导

   编译器推导调用模板参数时的参数类型

   ```c++
   void test01(){
   	int a = 10, b = 20;
   	MySwap(a, b);	// 编译器根据传值进行自动类型推导
   	cout << "a:" << a <<",b:"<< b << endl;
   }
   
   int main(){
       test01();
   }
   ```

   （2）显示指定类型

   直接说明调用的模板函数的参数类型

   ```c++
   void test01(){
   	int a = 10, b = 20;
   	MySwap<int> (a, b);	// 编译器根据传值进行自动类型推导
   	cout << "a:" << a <<",b:"<< b << endl;
   }
   
   int main(){
       test01();
   }
   ```

2. ### 函数模板和普通函数的区别

   - 函数模板不允许进行自动类型转化（严格的类型匹配）
   - 普通函数能够进行自动类型转化（如 double 可转为 int ）
   - C++ 编译器优先考虑普通函数
   - 如果模板函数可以得到更好的匹配，选择模板函数
   - 可以通过空模板实参列表 $ func\_name<>() $ 的语法**限定编译器**只能通过模板匹配

   ```c++
   template<class T>
   int MyAdd(T a, T b){
   	return a + b;
   }
   
   int MyAdd(int a, char b){
   	return a + b;
   }
   
   void test02(){
   	int a = 10;
   	int b = 20;
   	char c1 = 'a';
   	char c2 = 'b';
   	MyAdd(a, c1);	// 调用普通函数
   	MyAdd(a, b);	// 优先考虑普通函数，当普通函数不最满足的时候，调用模板进行匹配
       MyAdd<>(a, b);	// 特别选择调用模板
   	MyAdd(c1, a);	// 调用普通函数：可以进行自动类型转换
       
   }
   ```

   - 函数模板的重载

   ```c++
   template<class T>
   void Print(T a){
   	...
   }
   
   template<class T>
   void Print(T a, T b){
   	...
   }
   ```

3. ### C++  编译器模板机制剖析

   （1）C++ 编译流程

   ​		![C++编译过程](F:\YWM_work\C++\STL\images\C++编译过程.jpg)

   （2）模板函数调用原理

   ##### 函数模板  -- 具体模板函数 -- 调用：根据实际的调用情况，自动调用

   模板函数机制结论：

   - 编译器不是直接调用函数模板
   - 函数模板通过具体类型产生不同函数
   - 编译器会对函数模板进行<font color='red'>两次编译</font>，在声明的地方对模板代码本身进行编译，在调用的地方对参数替换后的代码进行编译。

4. ###  类模板

   ```c++
   template<class T>
   class Person{
   	public:
   		T id;
   	    Person(T id){
               this->id = id;
           }
   }
   // 要指定对象才能分配内存
   class subPerson: public Person<int>{}
   ```

   (1) 类模板派生类模板

   ```c++
   template<class T>
   class Animal{
   public:
   	void show(){
   		cout << mAge << endl;
   	}
   public:
   	T mAge;
   }
   
   template<class T>
   class Cat: public Animal<T>{}
   ```

   （2）类模板的类内实现

   （3）类模板的外部实现（.h 和 .cpp 分离）

   ```c++
   // Animal.h
   template<class T>
   class Animal{
   public:
   	void show(){
   		cout << mAge << endl;
   	}
   public:
   	T mAge;
   }
   ```

   




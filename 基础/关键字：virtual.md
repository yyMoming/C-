# 关键字：virtual

------

## 1.  介绍：

> <font color='green'>`virtual`</font> 是 C++ 面向对象的一个关键字。运用于描述虚函数和多态的运行。



## 2. 虚函数

> 虚函数源自类继承，是多态的一种。在c++中，一个基类的指针或者引用可以指向或者引用派生类的对象。同时，派生类可以重写基类中的成员函数。可以在基类中将被重写的成员函数设置为虚函数，其含义是：当通过基类的指针或者引用调用该成员函数时，将根据指针指向的对象类型确定调用的函数，而非指针的类型。

##### 		1）普通继承

​	<font color='green'>`Example 1：`</font>

```c++
class A {
public:
	A() {}
	A(int tmp) :a(tmp) {}
	void print() {
		cout << "base class func" << endl;
	}
private:
	int a;
};
// C类 以public机制继承了 A类
class C : public A {
public:
	C(int num) {
		A::A(num);
	}
	void print() {	// 覆写了基类中的函数
		cout << "son class func" << endl;
	}

private:
	int b;
};

int main() {
	A* a = new C(10);	// 类A的指针可以指向类C的实例
	a->print();	// 调用类A中的print函数
    C* c = new C(10);
    c->print(); // 调用类C中的print函数
}
// 输出
>>> "base class func"
>>> "son class func"
```

​	参照 <font color='green'>`Example 1`</font> ，在C类中不覆写基类中的函数。

```c++
class C : public A {
public:
	C(int num) {
		A::A(num);
	}
private:
	int b;
};
int main() {
	A* a = new C(10);	// 类A的指针可以指向类C的实例
	a->print();	// 调用类A中的print函数
    C* c = new C(10);
    c->print(); // 调用类C中的print函数
}
// 输出
>>> "base class func"
>>> "base class func"
```

> 可见，若C类中不存在`print` 函数，则C类的指针 `C* c`会到基类 A 中寻找这个函数（因为这个函数在A类中是`public`机制，可以被派生类使用）。

##### 	2）虚函数

​	参照 <font color='green'>`Example 1`</font> ，将 `print`函数用 <font color='green'>`virtual`</font>修饰。

​	<font color='green'>`Example 2：`</font>

```C++
class A {
public:
	A() {}
	A(int tmp) :a(tmp) {}
	virtual void print() {
		cout << "base class func" << endl;
	}
private:
	int a;
};
// C类 以public机制继承了 A类
class C : public A {
public:
	C(int num) {
		A::A(num);
	}
	void print() {	// 覆写了基类中的函数
		cout << "son class func" << endl;
	}
private:
	int b;
};

int main() {
	A* a = new C(10);	// 类A的指针可以指向类C的实例
	a->print();	// 调用类A中的print函数
    C* c = new C(10);
    c->print(); // 调用类C中的print函数
}
// 输出
>>> "son class func"
>>> "son class func"
```

> 可见，无论是A类指针还是C类指针指向C类的实例，当C类中覆写了A类中的虚函数时，指针会自动找到其指向的实例最“亲近”的函数。

##### 	3）纯虚函数：必须要被覆写的函数，基类不能被实例化

> 格式：<font color='green'>`vitual <ret_type> <func_name>(args) = 0`</font>

​	<font color='green'>`Example 3：`</font> 

```C++
class A {
public:
	A() {}
	A(int tmp) :a(tmp) {}
	virtual void print() = 0;	//必须被覆写
private:
	int a;
};
// C类 以public机制继承了 A类
class C : public A {
public:
	C(int num) {
		A::A(num);
	}
	void print() {	// 覆写了基类中的函数
		cout << "son class func" << endl;
	}
private:
	int b;
};
int main() {
	A* a = new C(10);	// 类A的指针可以指向类C的实例
	a->print();	// 调用类A中的print函数
    C* c = new C(10);
    c->print(); // 调用类C中的print函数
    A a_; // 错误，拥有虚函数的类不能被实例化
}
// 输出
>>> "son class func"
>>> "son class func"
```



## 3. C++虚函数的`vptr`与`vtable`

##### 	1）基础理论：

> 为了实现虚函数，C++使用一种称为虚拟表的特殊形式的后期绑定。该虚拟表是用于解决在动态/后期绑定方式的函数调用函数的查找表。虚拟表有时会使用其他名称，例如`“vtable”`，`“虚函数表”`，`“虚方法表”`或`“调度表”`。
>
> **每个使用虚函数的类（或者从使用虚函数的类派生）都有自己的虚拟表**。该表只是编译器在编译时设置的静态数组。虚拟表包含可由类的对象调用的每个虚函数的一个条目。此表中的每个条目只是一个函数指针，指向该类可访问的派生函数。
>
> 编译器还会添加一个隐藏指向基类的指针，我们称之为`vptr`。`vptr`在创建类实例时自动设置，以便指向该类的虚拟表。与this指针不同，`this`指针实际上是编译器用来解析自引用的函数参数，`vptr`是一个真正的指针。

```c++
class A {
public:
	int a;
	virtual void foo() {
		cout << "I'm foo of A" << endl;
	};
};
class B : public A {
public:
	char b;
	int b1;
	void print() {
		cout << "I'm bar of B" << endl;
	}
};

class C : public A {
public:
	char c;
	long c1;
	static int c2;
	virtual void foo() {
		cout << "I'm foo of C" << endl;
	}
	virtual void baz(){
		cout << "I'm baz of C" << endl;
	}
};


int main() {
	cout << "size of A = " << sizeof(A) << endl;
	cout << "size of B = " << sizeof(B) << endl;
	cout << "size of C = " << sizeof(C) << endl;
	A instA;
	B instB;
	C instC;
	cout << "Address of instA.a = " << int(&instA.a) << endl;
	cout << "Address of instB.a = " << int(&instB.a) << endl;
	cout << "Address of instB.b = " << int(&instB.b) << endl;
	cout << "Address of instB.b1 = " << int(&instB.b1) << endl;
	cout << "Address of instC.a = " << int(&instC.a) << endl;
	cout << "Address of instC.c = " << int(&instC.c) << endl;
	cout << "Address of instC.c1 = " << int(&instC.c1) << endl;
	
}
```

![image-20210324214220712](F:\YWM_work\C++\基础\images\virtual.jpg)

> 对齐原则：派生类的大小 + 基类的大小要为`vptr`大小的倍数。



## 4. 虚基类：菱形继承

> 在c++中，派生类可以继承多个基类。问题在于：如果这多个基类又是继承自同一个基类时，那么派生类是不是需要多次继承这“同一个基类”中的内容？虚基类可以解决这个问题。

```c++
class father{
public:
	int b;
	virtual void test(){
		cout << "base function" << endl;
	}
}

class son1: virtual public father{};
class son2: virtual public father{};
class grandson: public son1, public son2{};
```

![image-20210324221638569](F:\YWM_work\C++\基础\images\father.jpg)

![image-20210324221745792](F:\YWM_work\C++\基础\images\菱形继承.jpg)
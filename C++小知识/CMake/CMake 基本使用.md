# CMake 基本使用

### 一、基本使用

1. CMake 依靠读取 CmakeLists.txt 文件的内容决定对项目内的文件进行怎样的处理。一个基本的 CmakeLists.txt 文件最少要包含以下三行

   ```cmake
   cmake_minimum_required (VERSION 2.6)	# 规定CMake最低版本要求
   project (Tutorial)	# 给你的项目起个名字 例“Tutorial”
   add_executable(Tutorial tutorial.cxx)	# 项目中要求编译执行的cxx文件
   ```

   <font color='purple'>Tip：</font>CMake支持大小写混合

   

2. 创建一个 `.cxx ` 文件，例子 `tutorial.cxx`

```c
#include <stdio.h>
int main(int argc, char *argv[]){
	printf("hello world");
	return 0;
}
```



3. 构建程序：在包含 CMakeLists.txt  的目录下用 cmake 来编译这段代码，进入命令行执行内部构建命令：

```cmake
cmake .
```

之后会输出一段 log 信息：

> -- The C compiler identification is AppleClang 9.0.0.9000039
> -- The CXX compiler identification is AppleClang 9.0.0.9000039
> -- Check for working C compiler: XX/usr/bin/cc
> -- Check for working C compiler: XX/usr/bin/cc -- works
> -- Detecting C compiler ABI info
> -- Detecting C compiler ABI info - done
> -- Detecting C compile features
> -- Detecting C compile features - done
> -- Check for working CXX compiler: XX/usr/bin/c++
> -- Check for working CXX compiler: XX/usr/bin/c++ -- works
> -- Detecting CXX compiler ABI info
> -- Detecting CXX compiler ABI info - done
> -- Detecting CXX compile features
> -- Detecting CXX compile features - done
> -- Configuring done
> -- Generating done
> -- Build files have been written to: XX/Tutorial/XX



生成了三个文件：<font color='#ff502c'>`CMakeCache.txt`、`Makefile`、`cmake_install.cmake`和文件夹 `CmakeFiles`</font>

然后执行，即执行 `Makefile` 内容，即可生成可执行程序 `Tutorial`

```
make
```



### 二、基本语法

1. **添加版本号：**<font color='#ff502c'>`set(KEY VALUE)`</font> 接受两个参数声明变量。cmake 语法中使用 <font color='#ff502c'>`${KEY}` 来获取 `VALUE`</font>

```cmake
# The version number.
set (Tutorial_VERSION_MAJOR 1)
set (Tutorial_VERSION_MINOR 0)
# set the project name and version
project(Tutorial VERSION 1.0)
```



2. **配置头文件以将版本号传递给源代码：**

```cmake
configure_file(TutorialConfig.h.in TutorialConfig.h)
```

- 由于已配置的文件将被写入**二叉树**，因此我们必须将该目录添加到路径列表中以搜索包含文件。将代码添加到 <font color='#ff502c'>`CMakeLists.txt`</font> 文件的末尾：

```cmake
target_include_directories(Tutorial PUBLIC
             "${PROJECT_BINARY_DIR}"
             )
```

- 在源目录中创建 <font color='#ff502c'>`TutorialConfig.h.in`</font> 

```
// the configured options and settings for Tutorial
#define Tutorial_VERSION_MAJOR @Tutorial_VERSION_MAJOR@
#define Tutorial_VERSION_MINOR @Tutorial_VERSION_MINOR@
```

- 当 CMake 配置文件时， <font color='#ff502c'>`@Tutorial_VERSION_MAJOR@`</font> 和 <font color='#ff502c'>`@Tutorial_VERSION_MINOR@`</font> 都会被替换掉。



- 然后修改 `tutorial.cxx` 文件以包含配置头文件 <font color='#ff502c'>`TutorialConfig.h`</font> ，最后通过更新`tutorial.cxx`来打印版本号：

```C
#include <iostream>
#include "TutorialConfig.h"
using namespace std;
int main(){
  cout << "hello world" << endl;
  cout << " Version " << Tutorial_VERSION_MAJOR << "."
              << Tutorial_VERSION_MINOR << endl;
  cout << "Usage: " << " number" << endl;
  return 1;
}
```

![image-20210504163516773](/Users/admin/Documents/ywm_info/C++/C-/C++小知识/images/cmake_tutorial.png)

3. **指定 C++ 版本**

```cmake
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)
```


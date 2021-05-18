# FFmpeg C++使用过程

1. C++ 中调用C代码

>C＋＋和C是两种完全不同的编译链接处理方式，如果直接在C＋＋里面调用C函数，会找不到函数体，报链接错误。要解决这个问题，就要在 C＋＋文件里面显示声明一下哪些函数是C写的，要用C的方式来处理。

- 引用头文件前需要加上 extern “C”，以C的方式进行**链接**

```c++
#ifdef __cplusplus
extern "C"
{
#endif
#include "libavcodec/avcodec.h"

#ifdef __cplusplus
};
#endif
```



2. 编译代码

   1）命令行编译代码

```c++
g++ main.cpp -o Sightsing -I/usr/local/ffmpeg/include -L/usr/local/ffmpeg/lib -lavcodec -lavdevice -lavfilter -lavformat -lavutill/ffmpeg/lib -lavcodec -lavdevice -lavfilter
```

> `g++`执行参数：
>
> `-I`(i的大写)：从目录中寻找头文件
>
> `-Ldir`：指定编译的时候，搜索库的路径。比如你自己的库，可以用它制定目录，不然编译器将只在标准库的目录找。这个`dir`就是目录的名称。
>
> `-llibrary`：制定编译的时候使用的库library,其中lib可以省略



​	2）CMake

```cmake
cmake_minimum_required(VERSION 3.18)
project(Sightsing)

set(CMAKE_CXX_STANDARD 14)
include_directories(.)
include_directories(/usr/local/ffmpeg/include)
# link_directories 要放在 add_executable 前面，不然没办法找到 target_link_libraries里面的库
link_directories(/usr/local/ffmpeg/lib)
add_executable(Sightsing main.cpp)
# target_link_libraries 放在add_executable后面
target_link_libraries(Sightsing avcodec avdevice avfilter avformat avutil)
```

>link_directories 要放在 add_executable 前面，不然没办法找到 target_link_libraries里面的库
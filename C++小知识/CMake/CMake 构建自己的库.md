# CMake 构建自己的库

1. ### 构建库

```
├── CMakeLists.txt
├── MathFunctions
│   ├── CMakeLists.txt
│   ├── MathFunctions.h
│   └── mysqrt.cxx
├── TutorialConfig.h.in
└── tutorial.cxx
```

把库放到一个名为`mathfunction`的子目录中,在工程目录下新建`mathfunction`文件夹。这个文件夹中新建`MathFunctions/CMakeLists.txt`文件，包含以下一行代码 (创建`mysqrt.cxx`文件):

```cmake
add_library(MathFunctions mysqrt.cxx)
```

然后在这个文件夹中创建源文件`mysqrt.cxx`，它只有一个名为`mysqrt`的函数，与编译器的sqrt函数提供了类似的功能。

为了利用新库，我们在工程根目录下的`CMakeLists.txt`中添加 `add_subdirectory()` 来构建我们自己的库。

```cmake
include_directories ("${PROJECT_SOURCE_DIR}/MathFunctions")
add_subdirectory (MathFunctions) 
 
# add the executable
add_executable (Tutorial tutorial.cxx)
target_link_libraries (Tutorial MathFunctions)
```

我们还添加了另一个include目录,以便`MathFunctions / MathFunctions.h`可以为函数原型找到头文件,该文件代码如下：

```c
// MathFunctions / MathFunctions.h
double mysqrt(double x);
```



2. ### 构建可选选项

`MathFunctions`是我们自己构建的库，有时候我们需要控制这个库是否应该使用，那么可以为使用这个库添加一个开关，在构建大型项目时非常有用。

在项目根目录下的`CMakeLists.txt`文件中添加如下代码：

```cmake
# should we use our own math functions?
option (USE_MYMATH "Use tutorial provided math implementation" ON)

# add the MathFunctions library?
#
if (USE_MYMATH)
  include_directories ("${PROJECT_SOURCE_DIR}/MathFunctions")
  add_subdirectory (MathFunctions)
  set (EXTRA_LIBS ${EXTRA_LIBS} MathFunctions)
endif (USE_MYMATH)
 
# add the executable
add_executable (Tutorial tutorial.cxx)
target_link_libraries (Tutorial  ${EXTRA_LIBS})
```

> Set(<variable> <value>... [PARENT_SCOPE])
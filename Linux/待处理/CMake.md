先看一下基础知识

Cmake 入门实战：https://www.hahack.com/codes/cmake/



CMake官方教程— CMake 3.9.1rc1文档
https://cmake.org/cmake/help/v3.19/guide/tutorial/index.html

cmake使用示例与整理总结
https://blog.csdn.net/QTVLC/article/details/82380413

CMake命令/函数汇总（翻译自官方手册）
https://www.cnblogs.com/52php/p/5684588.html



编写CMakeLists.txt时候，关键字不区分大小写，但要统一

```cmake
# 声明要求的 cmake 最低版本
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

# 声明一个cmake 工程
PROJECT(Demo01)

# 设置编译模式
SET(CMAKE_BUILD_TYPE "Debug")

# 添加一个可执行程序
# 语法： add_executable(程序名 源代码文件)
ADD_EXECUTABLE(main hello.cpp)

```



### Step01

```cmake
# 默认第一句均是这句话，规定cmake所需最低版本
cmake_minimum_required(VERSION 2.8)

# 设置项目名称和版本
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# 添加可执行文件
add_executable(Tutorial tutorial.cxx)

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC "${PROJECT_BINARY_DIR}")
```



### Step02

```cmake
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
#if(USE_MYMATH)
#  add_subdirectory(MathFunctions)
#  list(APPEND EXTRA_LIBS MathFunctions)
#  list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")
#endif()
if (USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
  list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")
endif ()

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           ${EXTRA_INCLUDES}
                           )

```



### Step03


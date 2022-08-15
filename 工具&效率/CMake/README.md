# CMake

CMakeLists.txt 的语法比较简单，由命令、注释和空格组成，其中命令是不区分大小写的。符号 `#` 后面的内容被认为是注释。命令由命令名称、小括号和参数组成，参数之间使用空格进行间隔。



## 快速开始

```cmake
1、指定 cmake 的最小版本
cmake_minimum_required(VERSION 3.4.1)

2、设置项目名称，它会引入两个变量 demo_BINARY_DIR 和 demo_SOURCE_DIR，同时，cmake 自动定义了两个等价
的变量 PROJECT_BINARY_DIR 和 PROJECT_SOURCE_DIR。
project(demo)

3、设置编译类型，add_library 默认生成是静态库
add_executable(demo demo.cpp) # 生成可执行文件
add_library(common STATIC util.cpp) # 生成静态库
add_library(common SHARED util.cpp) # 生成动态库或共享库
以上命令将生成：
在 Linux 下是：
        demo
        libcommon.a
        libcommon.so
在 Windows 下是：
        demo.exe
        common.lib
        common.dll

4、明确指定包含哪些源文件
add_library(demo demo.cpp test.cpp util.cpp)

5、设置变量
5.1 set 直接设置变量的值
set(SRC_LIST main.cpp test.cpp)
add_executable(demo ${SRC_LIST})
set(ROOT_DIR ${CMAKE_SOURCE_DIR}) #CMAKE_SOURCE_DIR默认为当前cmakelist.txt目录

5.2 set追加设置变量的值
set(SRC_LIST main.cpp)
set(SRC_LIST ${SRC_LIST} test.cpp)
add_executable(demo ${SRC_LIST})

5.3 list追加或者删除变量的值
set(SRC_LIST main.cpp)
list(APPEND SRC_LIST test.cpp)
list(REMOVE_ITEM SRC_LIST main.cpp)
add_executable(demo ${SRC_LIST})

6、搜索文件
6.1 搜索当前目录下的所有.cpp文件，并命名为SRC_LIST，它会查找目录下的.c,.cpp ,.mm,.cc 等等C/C++语言后缀的文件名
aux_source_directory(. SRC_LIST) 
add_library(demo ${SRC_LIST})

6.2 自定义搜索规则
aux_source_directory(. SRC_LIST)
aux_source_directory(protocol SRC_PROTOCOL_LIST)
add_library(demo ${SRC_LIST} ${SRC_PROTOCOL_LIST})
或者
file(GLOB SRC_LIST "*.cpp" "protocol/*.cpp")
add_library(demo ${SRC_LIST})
# 或者
file(GLOB SRC_LIST "*.cpp")
file(GLOB SRC_PROTOCOL_LIST "protocol/*.cpp")
add_library(demo ${SRC_LIST} ${SRC_PROTOCOL_LIST})
# 或者
file(GLOB_RECURSE SRC_LIST "*.cpp") #递归搜索
FILE(GLOB SRC_PROTOCOL RELATIVE "protocol" "*.cpp") # 相对protocol目录下搜索
add_library(demo ${SRC_LIST} ${SRC_PROTOCOL_LIST})

7、设置包含的目录，头文件目录
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)
Linux 下还可以通过如下方式设置包含的目录
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -I${CMAKE_CURRENT_SOURCE_DIR}")

8、设置链接库搜索目录
link_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/libs
)
Linux 下还可以通过如下方式设置包含的目录
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -L${CMAKE_CURRENT_SOURCE_DIR}/libs")

9、设置 target 需要链接的库
9.1 指定链接动态库或静态库
target_link_libraries(demo libface.a) # 链接libface.a
target_link_libraries(demo libface.so) # 链接libface.so

9.2 指定全路径
target_link_libraries(demo ${CMAKE_CURRENT_SOURCE_DIR}/libs/libface.a)
target_link_libraries(demo ${CMAKE_CURRENT_SOURCE_DIR}/libs/libface.so)

9.3 指定链接多个库
target_link_libraries(demo
    ${CMAKE_CURRENT_SOURCE_DIR}/libs/libface.a
    boost_system.a
    boost_thread
    pthread)

10、打印信息
message(${PROJECT_SOURCE_DIR})
message("build with debug mode")
message(WARNING "this is warnning message")
message(FATAL_ERROR "this build has many error") # FATAL_ERROR 会导致编译失败

11.包含其它 cmake 文件
include(./common.cmake) # 指定包含文件的全路径
include(def) # 在搜索路径中搜索def.cmake文件
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake) # 设置include的搜索路径


12、条件控制
12.1 if…elseif…else…endif
逻辑判断和比较：
if (expression)：expression 不为空（0,N,NO,OFF,FALSE,NOTFOUND）时为真
if (not exp)：与上面相反
if (var1 AND var2)
if (var1 OR var2)
if (COMMAND cmd)：如果 cmd 确实是命令并可调用为真
if (EXISTS dir) if (EXISTS file)：如果目录或文件存在为真
if (file1 IS_NEWER_THAN file2)：当 file1 比 file2 新，或 file1/file2 中有一个不存在时为真，文件名需使用全路径
if (IS_DIRECTORY dir)：当 dir 是目录时为真
if (DEFINED var)：如果变量被定义为真
if (var MATCHES regex)：给定的变量或者字符串能够匹配正则表达式 regex 时为真，此处 var 可以用 var 名，也可以用 ${var}
if (string MATCHES regex)

数字比较：
if (variable LESS number)：LESS 小于
if (string LESS number)
if (variable GREATER number)：GREATER 大于
if (string GREATER number)
if (variable EQUAL number)：EQUAL 等于
if (string EQUAL number)

字母表顺序比较：
if (variable STRLESS string)
if (string STRLESS string)
if (variable STRGREATER string)
if (string STRGREATER string)
if (variable STREQUAL string)
if (string STREQUAL string)

12.2 while…endwhile
12.3 foreach…endforeach
foreach(i RANGE 1 9 2)
    message(${i})
endforeach(i)
# 输出：13579

13、常用变量
13.1 预定义变量
PROJECT_SOURCE_DIR：工程的根目录
PROJECT_BINARY_DIR：运行 cmake 命令的目录，通常是 ${PROJECT_SOURCE_DIR}/build
PROJECT_NAME：返回通过 project 命令定义的项目名称
CMAKE_CURRENT_SOURCE_DIR：当前处理的 CMakeLists.txt 所在的路径
CMAKE_CURRENT_BINARY_DIR：target 编译目录
CMAKE_CURRENT_LIST_DIR：CMakeLists.txt 的完整路径
CMAKE_CURRENT_LIST_LINE：当前所在的行
CMAKE_MODULE_PATH：定义自己的 cmake 模块所在的路径，SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)，然后可以用INCLUDE命令来调用自己的模块
EXECUTABLE_OUTPUT_PATH：重新定义目标二进制可执行文件的存放位置
LIBRARY_OUTPUT_PATH：重新定义目标链接库文件的存放位置

13.2 环境变量
$ENV{Name}
set(ENV{Name} value) # 这里没有“$”符号

13.3 系统信息
CMAKE_MAJOR_VERSION：cmake 主版本号，比如 3.4.1 中的 3
­CMAKE_MINOR_VERSION：cmake 次版本号，比如 3.4.1 中的 4
­CMAKE_PATCH_VERSION：cmake 补丁等级，比如 3.4.1 中的 1
­CMAKE_SYSTEM：系统名称，比如 Linux-­2.6.22
­CMAKE_SYSTEM_NAME：不包含版本的系统名，比如 Linux
­CMAKE_SYSTEM_VERSION：系统版本，比如 2.6.22
­CMAKE_SYSTEM_PROCESSOR：处理器名称，比如 i686
­UNIX：在所有的类 UNIX 平台下该值为 TRUE，包括 OS X 和 cygwin
­WIN32：在所有的 win32 平台下该值为 TRUE，包括 cygwin

14、主要开关选项
BUILD_SHARED_LIBS：这个开关用来控制默认的库编译方式，如果不进行设置，使用 add_library 又没有指定库类型的情况下，默认编译生成的库都是静态库。如果 set(BUILD_SHARED_LIBS ON) 后，默认生成的为动态库
CMAKE_C_FLAGS：设置 C 编译选项，也可以通过指令 add_definitions() 添加
CMAKE_CXX_FLAGS：设置 C++ 编译选项，也可以通过指令 add_definitions() 添加
add_definitions(-DENABLE_DEBUG -DABC) # 参数之间用空格分隔
```





##  CMake命令行说明

CMake命令行的选项可以在命令行终端上，输入`cmake --help`查看。更详尽的解释可以查看[cmake帮助手册](https://cmake.org/cmake/help/v3.7/manual/cmake.1.html)

```bash
- build
- main.cpp
- CMakelists.txt
```

在build目录下运行命令，`..`表示`build`的上一级目录存在`CMakeLists.txt`文件，按照此文件的配置在`build`目录下生成VS2022工程文件

```bash
cd build
cmake .. -G "Visual Studio 17 2022"
```

**`-G <generator-name>`**： 指定构建系统生成器，当前平台所支持的`generator-name`可以通过`cmake --help`查看。

**`-E`**： CMake命令行模式。CMake提供了一系列与平台无关的命令。例如：`copy`，`make_directory`，`echo`等，更多详细参见`cmake -E help`。

```bash
# display the current environment
cmake -E environment
```



## 常用命令

### cmake_minimum_required

 设置项目要求的CMake最低版本号，如果当前版本的CMake低于所需的值，它将停止处理项目并报告错误。注意在`project`之前调用该命令，一般在CMakeLists.txt文件开头调用。命令格式为：

```cmake
cmake_minimum_required(VERSION major.minor[.patch[.tweak]]
                       [FATAL_ERROR])
```

```cmake
cmake_minimum_required(VERSION 3.0)
```



### project

 为整个工程设置一个工程名。命令格式为：

```xml
project(<PROJECT-NAME>
        [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
        [LANGUAGES <language-name>...])
```

```undefined
project (HelloCMake)
```

### add_executable

 使用指定的源文件给项目添加一个可执行文件。命令格式为：

```css
add_executable(<name> [WIN32] [MACOSX_BUNDLE]
               [EXCLUDE_FROM_ALL]
               source1 [source2 ...])
```

 name：该命令调用列出的源文件来构建的可执行目标<name>。 <name>对应于逻辑目标名称，在项目中必须是全局唯一的。构建的可执行文件的实际文件名是基于本机平台的约定。

WIN32：如果给出WIN32，则在创建的目标上设置属性WIN32_EXECUTABLE。

MACOSX_BUNDLE：如果给定MACOSX_BUNDLE，将在创建的目标上设置相应的属性。

EXCLUDE_FROM_ALL：如果给定EXCLUDE_FROM_ALL，将在创建的目标上设置相应的属性。

source：源码列表。

```css
add_executable(HelloCMake hello_cmake.c)
```



### aux_source_directory

```cmake
aux_source_directory(<dir> <variable>)
```

```bash
#查找当前目录下所有源文件并保存至SRC_LIST变量中
aux_source_directory(. SRC_LIST)
```

### add_library

#### 添加一个库

```css
add_library(<name> [STATIC | SHARED | MODULE] [EXCLUDE_FROM_ALL] source1 source2 ... sourceN)
```

- 添加一个名为`<name>`的库文件
- 指定`STATIC, SHARED, MODULE`参数来指定要创建的库的类型, `STATIC`对应的静态库(.a)，`SHARED`对应共享动态库(.so)，`MODULE`不会被链接到其它目标中，但是可能会在运行时使用dlopen-系列的函数动态链接。如果不显示指定类型，默认是静态链接库。
- `[EXCLUDE_FROM_ALL]`, 如果指定了这一属性，对应的一些属性会在目标被创建时被设置(**指明此目录和子目录中所有的目标，是否应当从默认构建中排除, 子目录的IDE工程文件/Makefile将从顶级IDE工程文件/Makefile中排除**)
- `source1 source2 ... sourceN`用来指定源文件



#### 导入已有的库

```css
add_library(<name> [STATIC | SHARED | MODULE | UNKNOWN] IMPORTED)
```

导入了一个已存在的`<name>`库文件，导入库一般配合`set_target_properties`使用，这个命令用来指定导入库的路径,比如：

```bash
add_library(test SHARED IMPORTED)
set_target_properties(  test #指定目标库名称
                        PROPERTIES IMPORTED_LOCATION #指明要设置的参数
                        libs/src/${ANDROID_ABI}/libtest.so #设定导入库的路径)
```

### set_target_properties

设置目标的一些属性来改变它们构建的方式。命令格式为：

```undefined
set_target_properties(target1 target2 ...
                      PROPERTIES prop1 value1
                      prop2 value2 ...)
```

使用示例为：

```bash
set_target_properties(cocos2d
    PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
    VERSION "${COCOS2D_X_VERSION}"
)
```



### set

设置CMake变量

```bash
# 设置可执行文件的输出路径(EXCUTABLE_OUTPUT_PATH是全局变量)
set(EXECUTABLE_OUTPUT_PATH [output_path])
```

```bash
# 设置库文件的输出路径(LIBRARY_OUTPUT_PATH是全局变量)
set(LIBRARY_OUTPUT_PATH [output_path])
```

```bash
# 设置C++编译参数(CMAKE_CXX_FLAGS是全局变量)
set(CMAKE_CXX_FLAGS "-Wall std=c++11")
```

```bash
# 设置源文件集合(SOURCE_FILES是本地变量即自定义变量)
set(SOURCE_FILES main.cpp test.cpp ...)
```

### include_directories

设置头文件位置

```ruby
# 可以用相对或绝对路径，也可以用自定义的变量值
include_directories(./include ${MY_INCLUDE})
```

### add_executable

添加可执行文件

```bash
add_executable(<name> ${SRC_LIST})
```

### target_link_libraries

将若干库链接到目标库文件

```jsx
target_link_libraries(<name> lib1 lib2 lib3)
```

将`lib1, lib2, lib3`链接到`<name>`上

> NOTE: 链接的顺序应当符合gcc链接顺序规则，被链接的库放在依赖它的库的后面，即如果上面的命令中，lib1依赖于lib2, lib2又依赖于lib3，则在上面命令中必须严格按照`lib1 lib2 lib3`的顺序排列，否则会报错
>  也可以自定义链接选项, 比如针对lib1使用`-WL`选项,`target_link_libraries(<name> lib1 -WL, lib2 lib3)`

### add_definitions

为当前路径以及子目录的源文件加入由`-D`引入的宏定义

```CMAKE
add_definitions(-DFOO -DBAR ...)
```

```cmake
// 例如，添加宏定义WIN32
add_definitions(-DWIN32)
```



### add_dependencies

 使顶级目标依赖于其他顶级目标，以确保它们在该目标之前构建。这里的顶级目标是由`add_executable`，`add_library`或`add_custom_target`命令之一创建的目标。
 使用示例：

```bash
add_custom_target(mylib DEPENDS ${MYLIB})
add_executable(${APP_NAME} ${SRC_LIST})
add_dependencies(${APP_NAME} mylib)
```



### add_subdirectory

如果当前目录下还有子目录时可以使用`add_subdirectory`，子目录中也需要包含有`CMakeLists.txt`

```bash
# sub_dir指定包含CMakeLists.txt和源码文件的子目录位置
# binary_dir是输出路径， 一般可以不指定
add_subdirecroty(sub_dir [binary_dir])
```

### file

文件操作命令

```bash
# 将message写入filename文件中,会覆盖文件原有内容
file(WRITE filename "message")
```

```bash
# 将message写入filename文件中，会追加在文件末尾
file(APPEND filename "message")
```

```csharp
# 从filename文件中读取内容并存储到var变量中，如果指定了numBytes和offset，
# 则从offset处开始最多读numBytes个字节，另外如果指定了HEX参数，则内容会以十六进制形式存储在var变量中
file(READ filename var [LIMIT numBytes] [OFFSET offset] [HEX])
```

```xml
# 重命名文件
file(RENAME <oldname> <newname>)
```

```bash
# 删除文件， 等于rm命令
file(REMOVE [file1 ...])
```

```bash
# 递归的执行删除文件命令, 等于rm -r
file(REMOVE_RECURSE [file1 ...])
```

```css
# 根据指定的url下载文件
# timeout超时时间; 下载的状态会保存到status中; 下载日志会被保存到log; sum指定所下载文件预期的MD5值,如果指定会自动进行比对，如果不一致，则返回一个错误; SHOW_PROGRESS，进度信息会以状态信息的形式被打印出来
file(DOWNLOAD url file [TIMEOUT timeout] [STATUS status] [LOG log] [EXPECTED_MD5 sum] [SHOW_PROGRESS])
```

```bash
# 创建目录
file(MAKE_DIRECTORY [dir1 dir2 ...])
```

```bash
# 会把path转换为以unix的/开头的cmake风格路径,保存在result中
file(TO_CMAKE_PATH path result)
```

```objectivec
# 它会把cmake风格的路径转换为本地路径风格：windows下用"\"，而unix下用"/"
file(TO_NATIVE_PATH path result)
```

```css
# 将会为所有匹配查询表达式的文件生成一个文件list，并将该list存储进变量variable里, 如果一个表达式指定了RELATIVE, 返回的结果将会是相对于给定路径的相对路径, 查询表达式例子: *.cxx, *.vt?
NOTE: 按照官方文档的说法，不建议使用file的GLOB指令来收集工程的源文件
file(GLOB variable [RELATIVE path] [globbing expressions]...)
```

### set_directory_properties

设置某个路径的一种属性

```undefined
set_directory_properties(PROPERTIES prop1 value1 prop2 value2)
```

`prop1 prop`代表属性，取值为：

- INCLUDE_DIRECTORIES
- LINK_DIRECTORIES
- INCLUDE_REGULAR_EXPRESSION
- ADDITIONAL_MAKE_CLEAN_FILES

### set_property

在给定的作用域内设置一个命名的属性

```csharp
set_property(<GLOBAL | 
            DIRECTORY [dir] | 
            TARGET [target ...] | 
            SOURCE [src1 ...] | 
            TEST [test1 ...] | 
            CACHE [entry1 ...]>
             [APPEND] 
             PROPERTY <name> [value ...])
```

第一个参数决定了属性可以影响的作用域,必须为以下值：

- GLOBAL 全局作作用域,不接受名字
- DIRECTORY 默认为当前路径，但是同样也可以用[dir]指定路径
- TARGET 目标作用，可以是0个或多个已有的目标
- SOURCE 源作用域， 可以是0个过多个源文件
- TEST 测试作用域, 可以是0个或多个已有的测试
- CACHE 必须指定0个或多个cache中已有的条目

`PROPERTY`参数是必须的



### configure_file
 将文件复制到其他位置并修改其内容。命令格式为：

```css
configure_file(<input> <output>
               [COPYONLY] [ESCAPE_QUOTES] [@ONLY]
               [NEWLINE_STYLE [UNIX|DOS|WIN32|LF|CRLF] ])
```

```bash
configure_file("${PROJECT_SOURCE_DIR}/TutorialConfig.in" "${PROJECT_SOURCE_DIR}/TutorialConfig.h")
```

### find_file
 查找一个文件的完整路径。命令格式为：

```css
find_file (<VAR> name1 [path1 path2 ...])
```

```css
find_file(HELLO_H hello.h)
```

### find_library
 查找一个库文件。命令格式为：

```css
find_library (<VAR> name1 [path1 path2 ...])
```

使用示例：

```undefined
find_library(LUA lua5.1 /usr/lib /lib)
```

### find_package
 查找并加载外部项目的设置。命令格式为：

```css
find_package(<package> [version] [EXACT] [QUIET] [MODULE]
             [REQUIRED] [[COMPONENTS] [components...]]
             [OPTIONAL_COMPONENTS components...]
             [NO_POLICY_SCOPE])
```

使用示例为：

```undefined
find_package(Protobuf)
```

### find_path
 查找包含某个文件的路径。命令格式为：

```css
find_path (<VAR> name1 [path1 path2 ...])
```

使用示例：

```css
find_path(DIR_SRCS hello.h .)
```

### include_directories
 将给定的目录添加到编译器用于搜索包含文件的目录。相对路径则相对于当前源目录。命令格式为：

```css
include_directories([AFTER|BEFORE] [SYSTEM] dir1 [dir2 ...])
```

使用示例：

```bash
include_directories(
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${CMAKE_CURRENT_SOURCE_DIR}/cocos
  ${CMAKE_CURRENT_SOURCE_DIR}/cocos/platform
  ${CMAKE_CURRENT_SOURCE_DIR}/extensions
  ${CMAKE_CURRENT_SOURCE_DIR}/external
)
```

### include
 包含其他目录的CMakeLists.txt文件。命令格式为：

```php
include(<file|module> [OPTIONAL] [RESULT_VARIABLE <VAR>]
                      [NO_POLICY_SCOPE])
```

使用示例：

```objectivec
include(platform/CMakeLists.txt)
```

### link_directories
 指定链接器查找库的路径。命令格式为：

```undefined
link_directories(directory1 directory2 ...)
```

使用示例：

```bash
link_directories(${PROJECT_SOURCE_DIR}/lib)
```



### option
 提供用户可以选择的选项。命令格式为：

```csharp
option(<option_variable> "help string describing option"
       [initial value])
```

使用示例：

```bash
option (USE_MYMATH "Use tutorial provided math implementation" ON) 
```



### message
 向用户显示消息。命令格式为：

```bash
message([<mode>] "message to display" ...)
```

参数说明：
 mode：
 可选的值为none，STATUS，WARNING，AUTHOR_WARNING，SEND_ERROR，FATAL_ERROR，DEPRECATION。
 使用示例：

```bash
message(STATUS "This is BINARY dir " ${HELLO_BINARY_DIR})
```

**option**：
 提供用户可以选择的选项。命令格式为：

## 常用变量

使用`${}`进行变量的引用。例如：`message(${Hello_VERSION})`，Hello为工程名。CMake提供了很多有用的变量。以下仅列举常用的变量：

**`CMAKE_BINARY_DIR`**：构建树的顶层路径

**`CMAKE_COMMAND`**：指向CMake可执行文件的完整路径

**`CMAKE_CURRENT_BINARY_DIR`**：当前正在被处理的二进制目录的路径。

**`CMAKE_CURRENT_SOURCE_DIR`**：指向正在被处理的源码目录的路径。

**`CMAKE_HOME_DIRECTORY`**：指向源码树顶层的路径。

**`CMAKE_PROJECT_NAME`**：当前工程的工程名。

**`CMAKE_ROOT`**：CMake的安装路径。

**`CMAKE_SOURCE_DIR`**：源码树的顶层路径。

**`CMAKE_VERSION`**：cmake的完整版本号。

**`PROJECT_BINARY_DIR`**：指向当前编译工程构建的全路径。

**`<PROJECT-NAME>_BINARY_DIR`**：指向当前编译工程构建的全路径。

**`<PROJECT-NAME>_SOURCE_DIR`**：指向构建工程的全路径。

**`PROJECT_SOURCE_DIR`**：指向构建工程的全路径。

**`PROJECT_NAME`**：project命令传递的工程名参数。

**`<PROJECT-NAME>_VERSION`**：项目的完整版本号。





## 项目实战

### 单个源文件

> 通过命令行传入两个数字，第一个作为基数，第二个作为指数，计算这两个数的幂。项目地址：[Demo01](https://github.com/qiuyeyijian/cmake-demo/tree/main/Demo01)

最基本的项目是从源代码文件构建的可执行文件。对于简单的项目，只需要一个三行`CMakeLists.txt`文件。

```cmake
# cmake的最低版本号要求
cmake_minimum_required(VERSION 3.10)

# 设置项目名称
project(Demo01)

# 将名为main.cpp的源文件编译成一个名为Demo01的可执行文件
add_executable(Demo01 main.cpp)
```

* 将`CMakeLists.txt`文件与`main.cpp`文件放在同一目录下，并创建`build`目录。

* 进入到`build`目录，执行`cmake ..`。意思是根据上一级目录的`CMakeLists.txt`文件，在当前目录生成当前平台的工程文件。也可以通过`-G`参数执行平台。

* 执行`cmake --build .`构建当前工程（Linux下直接执行`make`就行）

* 在Windows平台下，会生成`Debug`文件夹，通过命令行执行里面的可执行文件

```bash
./Debug/Demo01.exe 2 2
# The program path is: D:\Workspace\Temp\cmake-demo\Demo01\build\Debug\Demo01.exe
# 2.000000 ^ 2.000000 is: 4.000
```



### 同一目录，多个源文件

> 在同一目录下实现了一个日志打印功能，对于两个文件`log.h`和`log.cpp`，最后在`main.cpp`中调用。项目地址：[Demo02](https://github.com/qiuyeyijian/cmake-demo/tree/main/Demo02)

```
- build
- main.cpp
- log.h
- log.cpp
- CMakeLists.txt
```

我们可以将当前目录下的所有源文件保存在一个变量中，这样我们就不用在`add_executable`中逐个添加源文件

```cmake
# cmake的最低版本号要求
cmake_minimum_required(VERSION 3.10)

# 设置项目名称
project(Demo02)

# 查找当前目录下的所有源文件，并将名称保存到 DIR_SRCS 变量中
aux_source_directory(. DIR_SRCS)

# 将名为main.cpp的源文件编译成一个名为Demo01的可执行文件
add_executable(Demo02 ${DIR_SRCS})
```



### 不同目录，多个源文件

> 将日志功能文件移动到log目录下，作为一个静态链接库提供给`main.cpp`调用。项目地址：[Demo03](https://github.com/qiuyeyijian/cmake-demo/tree/main/Demo03)

```
- build
- log
	- log.h
	- log.cpp
	- CMakeLists.txt
- main.cpp
- CMakeLists.txt
```

```cmake
# 根目录下的CMakeLists.txt

# cmake的最低版本号要求
cmake_minimum_required(VERSION 3.10)

# 设置项目名称
project(Demo03)

# 查找当前目录下的所有源文件，并将名称保存到 DIR_SRCS 变量中
aux_source_directory(. DIR_SRCS)

# 将名为main.cpp的源文件编译成一个名为Demo03的可执行文件
add_executable(Demo03 ${DIR_SRCS})
target_link_libraries(Demo03 log)

# 添加log子目录
add_subdirectory(log)
```

```cmake
# log 目录下的CMakeLists.txt
# 查找当前目录下的所有源文件，并将名称保存到 DIR_LIB_SRCS 变量中
aux_source_directory(. DIR_LIB_SRCS)

#生成链接库
add_library(log STATIC ${DIR_LIB_SRCS})
```



### 自定义编译选项

> 使用一个变量`LINUX_OS`来控制是否使用自己的log库。设计这个Demo的灵感来源是在Linux和Windows平台上改变终端字体颜色需要采取不同的措施。项目地址：[Demo04](https://github.com/qiuyeyijian/cmake-demo/tree/main/Demo04)

```
- build
- log
	- log.h
	- log.cpp
	- CMakeLists.txt
- config.h.in
- main.cpp
- CMakeLists.txt
```

```cmake
# 根目录下的CMakeLists.txt

# cmake的最低版本号要求
cmake_minimum_required(VERSION 3.10)

# 设置项目名称
project(Demo04)

# 加入一个配置头文件，用于处理CMake对源码的设置
configure_file(
    "${PROJECT_SOURCE_DIR}/config.h.in" "${PROJECT_SOURCE_DIR}/config.h"
)

# 是否使用自己的log库
option(LINUX_OS "Use log if the platform is Linux" OFF)

# 查找当前目录下的所有源文件，并将名称保存到 DIR_SRCS 变量中
aux_source_directory(. DIR_SRCS)

# 将名为main.cpp的源文件编译成一个名为Demo03的可执行文件
add_executable(Demo04 ${DIR_SRCS})

# 是否加入log库
if(LINUX_OS)
    include_directories("${PROJECT_SOURCE_DIR}/log")
    add_subdirectory(log)
    target_link_libraries(Demo04 log)
endif(LINUX_OS)
```

```cmake
# log 目录下的CMakeLists.txt
# 查找当前目录下的所有源文件，并将名称保存到 DIR_LIB_SRCS 变量中
aux_source_directory(. DIR_LIB_SRCS)

#生成链接库
add_library(log STATIC ${DIR_LIB_SRCS})
```





### 安装和测试

CMake 也可以指定安装规则，以及添加测试。这两个功能分别可以通过在产生 Makefile 后使用 `make install` 和 `make test` 来执行。这里只对安装做简单介绍。

> 项目的功能和Demo04相同，这里演示可以将生成的库和可执行文件安装到指定目录内。项目地址：[Demo5](https://github.com/qiuyeyijian/cmake-demo/tree/main/Demo05)。

```
- build
- log
	- log.h
	- log.cpp
	- CMakeLists.txt
- config.h.in
- main.cpp
- CMakeLists.txt
```

首先在`log/CMakeLists.txt`文件内，添加下面命令

```cmake
# 指定 log 库的安装路径
install(TARGETS log DESTINATION ${PROJECT_SOURCE_DIR}/bin)
install(FILES log.h DESTINATION ${PROJECT_SOURCE_DIR}/include)
```

然后在根目录的`CMakeLists.txt`文件内，添加下面命令

```cmake
# 指定安装路径
install(TARGETS Demo05 DESTINATION ${PROJECT_SOURCE_DIR}/bin)
install(FILES "${PROJECT_SOURCE_DIR}/config.h" DESTINATION ${PROJECT_SOURCE_DIR}/bin)
```

最后执行cmake命令，指定项目工程为`MinGW Makefiles`

```bash
cmake .. -G "MinGW Makefiles"
make install
```



### 支持GDB

让 CMake 支持 gdb 的设置也很容易，只需要指定 `Debug` 模式下开启 `-g` 选项。之后可以直接对生成的程序使用 gdb 来调试。

```cmake
set(CMAKE_BUILD_TYPE "Debug")
set(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb")
set(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")
```



### 添加环境检查

> 可以参考下面的过程，就不写Demo了

有时候可能要对系统环境做点检查，例如要使用一个平台相关的特性的时候。在这个例子中，我们检查系统是否自带 pow 函数。如果带有 pow 函数，就使用它；否则使用我们定义的 power 函数。

#### 添加 CheckFunctionExists 宏

首先在顶层 CMakeLists 文件中添加 CheckFunctionExists.cmake 宏，并调用 `check_function_exists` 命令测试链接器是否能够在链接阶段找到 `pow` 函数。

```cmake
# 检查系统是否支持 pow 函数
include (${CMAKE_ROOT}/Modules/CheckFunctionExists.cmake)
check_function_exists (pow HAVE_POW)
```

将上面这段代码放在 `configure_file` 命令前。

#### 预定义相关宏变量

接下来修改` config.h.in`文件，预定义相关的宏变量。

```cmake
// does the platform provide pow function?
#cmakedefine HAVE_POW
```

#### 在代码中使用宏和函数

最后一步是修改 `main.cc` ，在代码中使用宏和函数：

```cpp
#ifdef HAVE_POW
    printf("Now we use the standard library. \n");
    double result = pow(base, exponent);
#else
    printf("Now we use our own Math library. \n");
    double result = power(base, exponent);
#endif
```



### 添加版本号

>  本节对应的源代码所在目录：[Demo7](https://github.com/qiuyeyijian/cmake-demo/tree/main/Demo07)。

```
- build
- main.cpp
- config.h.in
- CMakeLists.txt
```

给项目添加和维护版本号是一个好习惯，这样有利于用户了解每个版本的维护情况，并及时了解当前所用的版本是否过时，或是否可能出现不兼容的情况。

首先修改顶层 `CMakeLists `文件，在项目名称后面加上版本

```cmake
# 设置项目名称和版本
project(Demo07 VERSION 3.4)
```

也可以分别指定当前的项目的主版本号和副版本号。

```cmake
# 分别设置系统主版本号和副版本号
set(Demo07_VERSION_MAJOR 5)
set(Demo07_VERSION_MINOR 6)
```

之后，为了在代码中获取版本信息，我们可以修改 `config.h.in`文件，添加两个预定义变量。之后就可以在项目中使用变量了。

```
// the configured options and settings for Tutorial
#define Demo_VERSION_MAJOR @Demo_VERSION_MAJOR@
#define Demo_VERSION_MINOR @Demo_VERSION_MINOR@
```





## Reference

* [CMake 入门实战](https://www.hahack.com/codes/cmake/)






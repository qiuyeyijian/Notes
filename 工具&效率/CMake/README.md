# CMake

CMakeLists.txt 的语法比较简单，由命令、注释和空格组成，其中命令是不区分大小写的。符号 # 后面的内容被认为是注释。命令由命令名称、小括号和参数组成，参数之间使用空格进行间隔。

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

**cmake_minimum_required(VERSION 3.4.1)**

指定需要的最小的cmake版本

### aux_source_directory

查找源文件并保存到相应的变量中:

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
- 指定`STATIC, SHARED, MODULE`参数来指定要创建的库的类型, `STATIC`对应的静态库(.a)，`SHARED`对应共享动态库(.so)
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

## 4. set

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

## 5. include_directories

设置头文件位置

```ruby
# 可以用相对货绝对路径，也可以用自定义的变量值
include_directories(./include ${MY_INCLUDE})
```

## 6. add_executable

添加可执行文件

```bash
add_executable(<name> ${SRC_LIST})
```

## 7. target_link_libraries

将若干库链接到目标库文件

```jsx
target_link_libraries(<name> lib1 lib2 lib3)
```

将`lib1, lib2, lib3`链接到`<name>`上

> NOTE: 链接的顺序应当符合gcc链接顺序规则，被链接的库放在依赖它的库的后面，即如果上面的命令中，lib1依赖于lib2, lib2又依赖于lib3，则在上面命令中必须严格按照`lib1 lib2 lib3`的顺序排列，否则会报错
>  也可以自定义链接选项, 比如针对lib1使用`-WL`选项,`target_link_libraries(<name> lib1 -WL, lib2 lib3)`

## 8. add_definitions

为当前路径以及子目录的源文件加入由`-D`引入得`define flag`

```undefined
add_definitions(-DFOO -DDEBUG ...)
```

## 9. add_subdirectory

如果当前目录下还有子目录时可以使用`add_subdirectory`，子目录中也需要包含有`CMakeLists.txt`



```bash
# sub_dir指定包含CMakeLists.txt和源码文件的子目录位置
# binary_dir是输出路径， 一般可以不指定
add_subdirecroty(sub_dir [binary_dir])
```

## 10. file

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

## 11. set_directory_properties

设置某个路径的一种属性



```undefined
set_directory_properties(PROPERTIES prop1 value1 prop2 value2)
```

`prop1 prop`代表属性，取值为：

- INCLUDE_DIRECTORIES
- LINK_DIRECTORIES
- INCLUDE_REGULAR_EXPRESSION
- ADDITIONAL_MAKE_CLEAN_FILES

## 12. set_property

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









## CMake语法及CMakeLists.txt编写

用CMake构建一个项目工程，是通过一个或多个CMakeLists.txt文件来控制的。CMakeLists.txt中包含一系列命令来描述需要执行的构建。在CMakeLists.txt中的命令的语法，都是形如下面这种格式：

```bash
# command：是命令的名字。args：是参数的列表，多个参数使用空格隔开。
command(args...)
```



### 常用的命令

**cmake_minimum_required**：
 设置项目要求的CMake最低版本号，如果当前版本的CMake低于所需的值，它将停止处理项目并报告错误。注意在`project`之前调用该命令，一般在CMakeLists.txt文件开头调用。命令格式为：

```css
cmake_minimum_required(VERSION major.minor[.patch[.tweak]]
                       [FATAL_ERROR])
```

```css
cmake_minimum_required(VERSION 3.0)
```

**add_custom_command**：
 该命令可以为生成的构建系统添加一条自定义的构建规则。这里又包含两种使用方式，一种是通过自定义命令在构建中生成输出文件，另外一种是向构建目标添加自定义命令。命令格式分别为：
 (1)生成文件

```csharp
add_custom_command(OUTPUT output1 [output2 ...]
                   COMMAND command1 [ARGS] [args1...]
                   [COMMAND command2 [ARGS] [args2...] ...]
                   [MAIN_DEPENDENCY depend]
                   [DEPENDS [depends...]]
                   [BYPRODUCTS [files...]]
                   [IMPLICIT_DEPENDS <lang1> depend1
                                    [<lang2> depend2] ...]
                   [WORKING_DIRECTORY dir]
                   [COMMENT comment]
                   [DEPFILE depfile]
                   [VERBATIM] [APPEND] [USES_TERMINAL])
```

参数介绍：
 OUTPUT：
 指定命令预期产生的输出文件。如果输出文件的名称是相对路径，即相对于当前的构建的源目录路径。输出文件可以指定多个output1,output2(可选)等。

COMMAND：
 指定要在构建时执行的命令行。如果指定多个COMMAND，它们讲按顺心执行。`ARGS`参数是为了向后兼容，为可选参数。args1和args2为参数，多个参数用空格隔开。

MAIN_DEPENDENCY：
 可选命令，指定命令的主要输入源文件。

DEPENDS：
 指定命令所依赖的文件。

BYPRODUCTS：
 可选命令，指定命令预期产生的文件，但其修改时间可能会比依赖性更新，也可能不会更新。

IMPLICIT_DEPENDS：
 可选命令，请求扫描输入文件的隐式依赖关系。给定的语言指定应使用相应的依赖性扫描器的编程语言。目前只支持C和CXX语言扫描器。必须为IMPLICIT_DEPENDS列表中的每个文件指定语言。从扫描中发现的依赖关系在构建时添加到自定义命令的依赖关系。请注意，IMPLICIT_DEPENDS选项目前仅支持Makefile生成器，并且将被其他生成器忽略。

WORKING_DIRECTORY：
 可选命令，使用给定的当前工作目录执行命令。如果它是相对路径，它将相对于对应于当前源目录的构建树目录。

COMMENT：
 可选命令，在构建时执行命令之前显示给定消息。

DEPFILE：
 可选命令，为Ninja生成器指定一个`.d` depfile。 `.d`文件保存通常由自定义命令本身发出的依赖关系。对其他生成器使用DEPFILE是一个错误。

使用实例：



```bash
add_executable(MakeTable MakeTable.cxx) 
add_custom_command (
  OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/Table.h
  COMMAND MakeTable ${CMAKE_CURRENT_BINARY_DIR}/Table.h
  DEPENDS MakeTable
  COMMENT "This is a test"
  )
```

(2)自定义构建事件

```csharp
add_custom_command(TARGET <target>
                   PRE_BUILD | PRE_LINK | POST_BUILD
                   COMMAND command1 [ARGS] [args1...]
                   [COMMAND command2 [ARGS] [args2...] ...]
                   [BYPRODUCTS [files...]]
                   [WORKING_DIRECTORY dir]
                   [COMMENT comment]
                   [VERBATIM] [USES_TERMINAL])
```

参数介绍：
 TARGET：
 定义了与构建指定<target>相关联的新命令。当<target>已经存在是，相应的command将不再执行。

PRE_BUILD：
 在目标中执行任何其他规则之前运行。这仅在Visual Studio 7或更高版本上受支持。对于所有其他生成器PRE_BUILD将被视为PRE_LINK。

PRE_LINK：
 在编译源之后运行，但在链接二进制文件或运行静态库的库管理器或存档器工具之前运行。

POST_BUILD：
 在目标中的所有其他规则都已执行后运行。

使用实例：

```bash
  add_custom_command(TARGET ${APP_NAME} 
             PRE_BUILD
                     COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_CURRENT_SOURCE_DIR}/Resources ${CMAKE_CURRENT_BINARY_DIR})
```

**add_custom_target**：
 该命令可以给指定名称的目标执行指定的命令，该目标没有输出文件，并始终被构建。命令的格式为：



```css
add_custom_target(Name [ALL] [command1 [args1...]]
                  [COMMAND command2 [args2...] ...]
                  [DEPENDS depend depend depend ... ]
                  [BYPRODUCTS [files...]]
                  [WORKING_DIRECTORY dir]
                  [COMMENT comment]
                  [VERBATIM] [USES_TERMINAL]
                  [SOURCES src1 [src2...]])
```

参数介绍(上面介绍过含义相同的参数，这里就不再赘述了)：
 Name：
 指定目标的名称。

ALL：
 表明此目标应添加到默认构建目标，以便每次都将运行（该命令名称不能为ALL）

SOURCES：
 指定要包括在自定义目标中的其他源文件。指定的源文件将被添加到IDE项目文件中，以方便编辑，即使它们没有构建规则。

使用示例：



```bash
add_custom_target(APP ALL
      DEPENDS ${APP_NAME} # 依赖add_custom_command输出的jar包
      COMMENT "building cassdk_jni.jar"
    )
```

**add_definitions**：
 为源文件的编译添加由-D引入的宏定义。命令格式为：



```undefined
add_definitions(-DFOO -DBAR ...)
```

使用示例：



```undefined
add_definitions(-DWIN32)
```

**add_dependencies**：
 使顶级目标依赖于其他顶级目标，以确保它们在该目标之前构建。这里的顶级目标是由`add_executable`，`add_library`或`add_custom_target`命令之一创建的目标。
 使用示例：



```bash
add_custom_target(mylib DEPENDS ${MYLIB})
add_executable(${APP_NAME} ${SRC_LIST})
add_dependencies(${APP_NAME} mylib)
```

**add_executable**：
 使用指定的源文件给项目添加一个可执行文件。命令格式为：



```css
add_executable(<name> [WIN32] [MACOSX_BUNDLE]
               [EXCLUDE_FROM_ALL]
               source1 [source2 ...])
```

参数介绍：
 name：
 该命令调用列出的源文件来构建的可执行目标<name>。 <name>对应于逻辑目标名称，在项目中必须是全局唯一的。构建的可执行文件的实际文件名是基于本机平台的约定。

WIN32：
 如果给出WIN32，则在创建的目标上设置属性WIN32_EXECUTABLE。

MACOSX_BUNDLE:
 如果给定MACOSX_BUNDLE，将在创建的目标上设置相应的属性。

EXCLUDE_FROM_ALL：
 如果给定EXCLUDE_FROM_ALL，将在创建的目标上设置相应的属性。

source：
 源码列表。

使用示例：



```css
add_executable(HelloCMake hello_cmake.c)
```

**add_library**：
 使用指定的源文件给项目添加一个库。命令格式为：



```css
add_library(<name> [STATIC | SHARED | MODULE]
            [EXCLUDE_FROM_ALL]
            source1 [source2 ...])
```

参数介绍：
 name：
 该命令调用列出的源文件来构建的库目标<name>。<name>对应于逻辑目标名称，在项目中必须是全局唯一的。

STATIC：
 静态库，在链接其他目标时使用。

SHARED：
 动态链接库，运行时加载。

MODULE：
 不会被链接到其它目标中，但是可能会在运行时使用dlopen-系列的函数动态链接。

使用示例：



```css
add_library(HelloCMake hello_cmake.c)
```

**add_subdirectory**:
 向构建中添加子目录。命令格式为：



```css
add_subdirectory(source_dir [binary_dir]
                 [EXCLUDE_FROM_ALL])
```

使用示例：



```bash
add_subdirectory(${SRC_ROOT})
```

**aux_source_directory**：
 查找目录中的所有源文件。命令格式为：



```xml
aux_source_directory(<dir> <variable>)
```

查找指定目录dir中所有源文件的名称，并将列表存储在提供的variable中。

使用示例：



```bash
aux_source_directory(. DIR_SRCS)
add_executable(${APP_NAME} ${DIR_SRCS})
```

**configure_file**：
 将文件复制到其他位置并修改其内容。命令格式为：



```css
configure_file(<input> <output>
               [COPYONLY] [ESCAPE_QUOTES] [@ONLY]
               [NEWLINE_STYLE [UNIX|DOS|WIN32|LF|CRLF] ])
```

使用示例：



```bash
configure_file (
  "${PROJECT_SOURCE_DIR}/Config.h.in"
  "${PROJECT_BINARY_DIR}/Config.h"
  )
```

**file**：
 文件操作相关的命令。命令格式为：



```xml
file(WRITE <filename> <content>...)
file(APPEND <filename> <content>...)
file(READ <filename> <variable>
     [OFFSET <offset>] [LIMIT <max-in>] [HEX])
file(STRINGS <filename> <variable> [<options>...])
file(<MD5|SHA1|SHA224|SHA256|SHA384|SHA512> <filename> <variable>)
file(GLOB <variable>
     [LIST_DIRECTORIES true|false] [RELATIVE <path>]
     [<globbing-expressions>...])
file(GLOB_RECURSE <variable> [FOLLOW_SYMLINKS]
     [LIST_DIRECTORIES true|false] [RELATIVE <path>]
     [<globbing-expressions>...])
file(RENAME <oldname> <newname>)
file(REMOVE [<files>...])
file(REMOVE_RECURSE [<files>...])
file(MAKE_DIRECTORY [<directories>...])
file(RELATIVE_PATH <variable> <directory> <file>)
file(TO_CMAKE_PATH "<path>" <variable>)
file(TO_NATIVE_PATH "<path>" <variable>)
file(DOWNLOAD <url> <file> [<options>...])
file(UPLOAD   <file> <url> [<options>...])
file(TIMESTAMP <filename> <variable> [<format>] [UTC])
file(GENERATE OUTPUT output-file
     <INPUT input-file|CONTENT content>
     [CONDITION expression])
file(<COPY|INSTALL> <files>... DESTINATION <dir>
     [FILE_PERMISSIONS <permissions>...]
     [DIRECTORY_PERMISSIONS <permissions>...]
     [NO_SOURCE_PERMISSIONS] [USE_SOURCE_PERMISSIONS]
     [FILES_MATCHING]
     [[PATTERN <pattern> | REGEX <regex>]
      [EXCLUDE] [PERMISSIONS <permissions>...]] [...])
file(LOCK <path> [DIRECTORY] [RELEASE]
     [GUARD <FUNCTION|FILE|PROCESS>]
     [RESULT_VARIABLE <variable>]
     [TIMEOUT <seconds>])
```

以上都是文件相关的操作，这里就不详细解释。
 使用示例为：



```bash
# 查找src目录下所有以hello开头的文件并保存到SRC_FILES变量里
file(GLOB SRC_FILES "src/hello*")
# 递归查找
file(GLOB_RECURSE SRC_FILES "src/hello*")
```

**find_file**：
 查找一个文件的完整路径。命令格式为：



```css
find_file (<VAR> name1 [path1 path2 ...])
```

使用示例：



```css
find_file(HELLO_H hello.h)
```

**find_library**：
 查找一个库文件。命令格式为：



```css
find_library (<VAR> name1 [path1 path2 ...])
```

使用示例：



```undefined
find_library(LUA lua5.1 /usr/lib /lib)
```

**find_package**：
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

**find_path**：
 查找包含某个文件的路径。命令格式为：



```css
find_path (<VAR> name1 [path1 path2 ...])
```

使用示例：



```css
find_path(DIR_SRCS hello.h .)
```

**include_directories**：
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

**include**：
 包含其他目录的CMakeLists.txt文件。命令格式为：



```php
include(<file|module> [OPTIONAL] [RESULT_VARIABLE <VAR>]
                      [NO_POLICY_SCOPE])
```

使用示例：



```objectivec
include(platform/CMakeLists.txt)
```

**link_directories**：
 指定链接器查找库的路径。命令格式为：



```undefined
link_directories(directory1 directory2 ...)
```

使用示例：



```bash
link_directories(${PROJECT_SOURCE_DIR}/lib)
```

**list**：
 列表相关的操作。命令格式为：



```xml
list(LENGTH <list> <output variable>)
list(GET <list> <element index> [<element index> ...]
     <output variable>)
list(APPEND <list> [<element> ...])
list(FILTER <list> <INCLUDE|EXCLUDE> REGEX <regular_expression>)
list(FIND <list> <value> <output variable>)
list(INSERT <list> <element_index> <element> [<element> ...])
list(REMOVE_ITEM <list> <value> [<value> ...])
list(REMOVE_AT <list> <index> [<index> ...])
list(REMOVE_DUPLICATES <list>)
list(REVERSE <list>)
list(SORT <list>)
```

使用示例：



```bash
list(APPEND SRC_LIST
    ${PROTO_SRC}
)
```

**message**：
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



```csharp
option(<option_variable> "help string describing option"
       [initial value])
```

使用示例：



```bash
option (USE_MYMATH "Use tutorial provided math implementation" ON) 
```

**project**：
 为整个工程设置一个工程名。命令格式为：



```xml
project(<PROJECT-NAME> [LANGUAGES] [<language-name>...])
project(<PROJECT-NAME>
        [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
        [LANGUAGES <language-name>...])
```

使用示例：



```undefined
project (HelloCMake)
```

**set**：
 将一个CMAKE变量设置为给定值。命令格式为：



```csharp
set(<variable> <value>... [PARENT_SCOPE])
```

使用示例：



```bash
set(COCOS2D_ROOT ${CMAKE_SOURCE_DIR}/cocos2d)
```

**set_target_properties**：
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

**string**：
 字符串相关操作。命令格式为：



```xml
string(FIND <string> <substring> <output variable> [REVERSE])
string(REPLACE <match_string>
       <replace_string> <output variable>
       <input> [<input>...])
string(REGEX MATCH <regular_expression>
       <output variable> <input> [<input>...])
string(REGEX MATCHALL <regular_expression>
       <output variable> <input> [<input>...])
string(REGEX REPLACE <regular_expression>
       <replace_expression> <output variable>
       <input> [<input>...])
string(APPEND <string variable> [<input>...])
string(CONCAT <output variable> [<input>...])
string(TOLOWER <string1> <output variable>) 
string(TOUPPER <string1> <output variable>)
string(LENGTH <string> <output variable>)
string(SUBSTRING <string> <begin> <length> <output variable>)
string(STRIP <string> <output variable>)
string(GENEX_STRIP <input string> <output variable>)  
string(COMPARE LESS <string1> <string2> <output variable>)
string(COMPARE GREATER <string1> <string2> <output variable>)
string(COMPARE EQUAL <string1> <string2> <output variable>)
string(COMPARE NOTEQUAL <string1> <string2> <output variable>)
string(COMPARE LESS_EQUAL <string1> <string2> <output variable>)
string(COMPARE GREATER_EQUAL <string1> <string2> <output variable>)
string(<MD5|SHA1|SHA224|SHA256|SHA384|SHA512>
       <output variable> <input>)
string(ASCII <number> [<number> ...] <output variable>)
string(CONFIGURE <string1> <output variable>
       [@ONLY] [ESCAPE_QUOTES])
string(RANDOM [LENGTH <length>] [ALPHABET <alphabet>]
       [RANDOM_SEED <seed>] <output variable>)
string(TIMESTAMP <output variable> [<format string>] [UTC])
string(MAKE_C_IDENTIFIER <input string> <output variable>)
string(UUID <output variable> NAMESPACE <namespace> NAME <name>
       TYPE <MD5|SHA1> [UPPER])
```

使用示例：



```bash
string(REPLACE "${PROJECT_SOURCE_DIR}/hello.c" "" DIR_SRCS "${DIR_ROOT}")
```

**target_link_libraries**：
 将给定的库链接到一个目标上。命令格式为：



```xml
target_link_libraries(<target> ... <item>... ...)
```

使用示例：



```undefined
target_link_libraries(luacocos2d cocos2d)
```

##### 3.1.2 常用的变量

使用${}进行变量的引用。例如：message(${Hello_VERSION})，Hello为工程名。CMake提供了很多有用的变量。以下仅列举常用的变量：

**`CMAKE_BINARY_DIR`**：
 构建树的顶层路径

**`CMAKE_COMMAND`**：
 指向CMake可执行文件的完整路径

**`CMAKE_CURRENT_BINARY_DIR`**：
 当前正在被处理的二进制目录的路径。

**`CMAKE_CURRENT_SOURCE_DIR`**：
 指向正在被处理的源码目录的路径。

**`CMAKE_HOME_DIRECTORY`**：
 指向源码树顶层的路径。

**`CMAKE_PROJECT_NAME`**：
 当前工程的工程名。

**`CMAKE_ROOT`**：
 CMake的安装路径。

**`CMAKE_SOURCE_DIR`**：
 源码树的顶层路径。

**`CMAKE_VERSION`**：
 cmake的完整版本号。

**`PROJECT_BINARY_DIR`**：
 指向当前编译工程构建的全路径。

**`<PROJECT-NAME>_BINARY_DIR`**：
 指向当前编译工程构建的全路径。

**`<PROJECT-NAME>_SOURCE_DIR`**：
 指向构建工程的全路径。

**`PROJECT_SOURCE_DIR`**：
 指向构建工程的全路径。

**`PROJECT_NAME`**：
 project命令传递的工程名参数。

**`<PROJECT-NAME>_VERSION`**：
 项目的完整版本号。

##### 3.2 CMakeLists.txt编写

有了上面的基础，再编写CMakeLists.txt自然会事半功倍。下面，以几个小实例来说下通过CMakeLists.txt的来构建项目。

这里cJSON库为例来说明下CMakeLists.txt的写法。当然，这里的代码并严谨，仅用来演示CMakeList的用法。

##### 3.2.1 将cJSON构建为静态库

(1)在本地建立cJSONdemo1的目录工程，并将cJSON库源代码拷贝到目录中，并在该目录新建`CMakeLists.txt`文件。目录结构如下：



```css
cJSONdemo1  
├── cJSON_Utils.h  
├── cJSON_Utils.c  
├── cJSON.h
├── cJSON.c
└── CMakeLists.txt 
```

CMakeLists.txt文件内容如下：



```swift
cmake_minimum_required(VERSION 2.8.5)
project(cJSON-lib)
set(CJSON_SRC cJSON.c cJSON_Utils.c)
add_library(cjson STATIC ${CJSON_SRC})
```

在终端下执行如下操作：

![img](https:////upload-images.jianshu.io/upload_images/3084440-63b70456aa84a48e.png?imageMogr2/auto-orient/strip|imageView2/2/w/722/format/webp)



(2)自动搜索目录源码
 在上面cJSONdemo1的基础上做一些改进。前面提过`set(<variable> <value>...)`，可以预见在cJSON库源码越来越多的情况下，会变成这样：



```swift
set(CJSON_SRC cJSON.c cJSON1.c cJSON2.c cJSON3.c cJSON4.c cJSON5.c)
```

这样，源文件越多，需要添加次数就越多。而且，每增加一个源文件就需要修改`CMakeLists.txt`文件，“耦合性”太大。这里，可以使用`aux_source_directory`来自动查找源文件。`CMakeLists.txt`文件最终如下：



```bash
cmake_minimum_required(VERSION 2.8.5)
project(cJSON-lib)
aux_source_directory(. CJSON_SRC)
add_library(cjson STATIC ${CJSON_SRC})
```

(3)递归搜索目录源码
 若将cJSONdemo改成包含子目录，子目录中又包含源码的形式，有多级目录。如下



```css
cJSONdemo1  
  │── cJSON_Utils.h  
  │── cJSON_Utils.c  
  │── cJSON.h
  │── cJSON.c
  │── CMakeLists.txt 
  └── foo 
      ├── cJSON1.h
      ├── cJSON1.c
      ├── cJSON2.h
      ├── cJSON2.c
      └── goo 
          ├── cJSON3.h
          ├── cJSON3.c
          ├── cJSON4.h
          └── cJSON4.c
```

可以使用`file`命令，来自动递归查找相应的源文件。`CMakeLists.txt`文件最终如下：



```bash
cmake_minimum_required(VERSION 2.8.5)
project(cJSON-lib)
file(GLOB_RECURSE CJSON_SRC ${CMAKE_CURRENT_SOURCE_DIR}/*.c)
add_library(cjson STATIC ${CJSON_SRC})
```

(4)指定构建库的名字，路径和前缀。`CMakeLists.txt`文件最终如下：



```bash
cmake_minimum_required(VERSION 2.8.5)
project(cJSON-lib)
file(GLOB_RECURSE CJSON_SRC ${CMAKE_CURRENT_SOURCE_DIR}/*.c)
add_library(cjson STATIC ${CJSON_SRC})
set_target_properties(cjson PROPERTIES OUTPUT_NAME "json")
set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/static)
set(CMAKE_STATIC_LIBRARY_PREFIX "")
```

最终效果如图：

![img](https:////upload-images.jianshu.io/upload_images/3084440-6030ea3c6b551053.png?imageMogr2/auto-orient/strip|imageView2/2/w/872/format/webp)


 会生成`cJSONdemo1/static/json.a`。



##### 3.2.1 将cJSON外部依赖库链接进可执行文件中

通过上面过程了解了将cJSON库构建文件静态库的过程。下面，再添加测试代码来调用cJSON库，并最终构建为可执行文件。目录如下：



```css
cJSONdemo2  
  │── test.c
  │── CMakeLists.txt 
  └── lib 
      ├── cJScJSON_UtilsON1.h
      ├── cJSON_Utils.c
      ├── cJSON.h
      ├── cJSON.c
      └── CMakeLists.txt 
```

**test.c**：



```cpp
#include <stdio.h>
#include <stdlib.h>
#include "lib/cJSON.h"

void parser(char* text) {
    char *out;
    cJSON *json;

    json = cJSON_Parse(text);
    if (!json) {
        printf("Error before: [%s]\n", cJSON_GetErrorPtr());
    }else {
        out = cJSON_Print(json);
        cJSON_Delete(json);
        printf("%s\n", out);
        free(out);
    }
}

int main(int argc, char * argv[]) {
    char text[]="[\"Sunday\", \"Monday\", \"Tuesday\", \"Wednesday\", \"Thursday\", \"Friday\", \"Saturday\"]";
    parser(text);

    return 0;
}
```

**cJSONdemo2/CMakeLists.txt**：



```bash
cmake_minimum_required(VERSION 2.8.5)

project(cjson-example)

aux_source_directory(. CJSON_EXAMPLE_SRC)

add_subdirectory(./lib)

add_executable(cjson-example ${CJSON_EXAMPLE_SRC})

target_link_libraries(cjson-example cjson)
```

**cJSONdemo2/lib/CMakeLists.txt**：



```bash
cmake_minimum_required(VERSION 2.8.5)

aux_source_directory(. CJSON_SRC)

add_library(cjson STATIC ${CJSON_SRC})
```

在终端下执行如下操作：



![img](https:////upload-images.jianshu.io/upload_images/3084440-4324801881b7072b.png?imageMogr2/auto-orient/strip|imageView2/2/w/708/format/webp)

##### 3.2.2 将cJSON库改为可选

在上面cJSONdemo2的基础上，新建`cJSONConfig.h.in`并相应修改`test.c`。目录如下：



```css
cJSONdemo3  
  │── test.c
  │── cJSONConfig.h.in
  │── CMakeLists.txt 
  └── lib 
      ├── cJScJSON_UtilsON1.h
      ├── cJSON_Utils.c
      ├── cJSON.h
      ├── cJSON.c
      └── CMakeLists.txt 
```

**cJSONConfig.h.in**：



```bash
#cmakedefine USE_CJSON
```

**test.c**：



```cpp
#include <stdio.h>
#include <stdlib.h>
#ifdef USE_MYMATH
#include "lib/cJSON.h"

void parser(char* text) {
    char *out;
    cJSON *json;

    json = cJSON_Parse(text);
    if (!json) {
        printf("Error before: [%s]\n", cJSON_GetErrorPtr());
    }else {
        out = cJSON_Print(json);
        cJSON_Delete(json);
        printf("%s\n", out);
        free(out);
    }
}

#endif

int main(int argc, char * argv[]) {
    char text[]="[\"Sunday\", \"Monday\", \"Tuesday\", \"Wednesday\", \"Thursday\", \"Friday\", \"Saturday\"]";
    #ifdef USE_MYMATH
        parser(text);
    #else
        printf("use other json library\n");
    #endif

    return 0;
}
```

最终效果如下：

![img](https:////upload-images.jianshu.io/upload_images/3084440-2622f2db371dd5f9.png?imageMogr2/auto-orient/strip|imageView2/2/w/886/format/webp)



以上只是通过一些简单的示例来说明了CMake基础及相关应用。更多高级功能需要日常的实践及查询CMake官方文档。


> 
> 对于我自己来说，学习新框架或技术的最佳方式是同时获得实践经验，在本文中，你将自己通过编写代码来学习shell脚本的基础知识！本文包含语法，shell脚本的基础知识到中级shell编程，通过这篇文章你可以学习shell的相关知识，并且通过shell来实现Unix/Linux之间的接口



![img](Shell%E6%95%99%E7%A8%8B.assets/v2-1770e9c89878049d6841c4792cf30a35_720w.jpg)Welcome Friend



## 介绍

您可能已经多次遇到过“脚本”这个词，但脚本的的含义是什么意思呢？简单的来说，脚本是包含一系列要执行的命令。这些命令由解释器执行。一切你可以在命令行中输入的命令，你都可以把它放到脚本中。而且，脚本非常适合自动化任务。如果你发现自己频繁重复一些命令，你可以创建一个脚本来实现它！



![img](Shell%E6%95%99%E7%A8%8B.assets/v2-c9dc671d006e98d217079abcca6f3e5c_720w.jpg)脚本对比图



> The Linux philosophy is ‘Laugh in the face of danger’. Oops. Wrong One. ‘Do it yourself’. Yes, that’s it.
>  — Linus Torvalds

## 我们的第一个脚本

```
script.sh
#!/bin/bash
echo "My First Script!"
```

运行脚本

```bash
$ chmod 755 script.sh # chmod +x script.sh
$ ./script.sh
```



![img](Shell%E6%95%99%E7%A8%8B.assets/v2-61287cc62fe70ce318d2862fcb4e49ba_720w.jpg)chmod



**好流弊😯！你刚刚编写了你的第一个bash脚本。**我知道你不理解这个脚本，特别对于脚本中的第一行。不要担心我将在本文中详细介绍shell脚本，在进入任何主题之前，我总是建议在脑海中形成路线图或适当的内容索引，并明确我们将要学习的内容。**因此，以下是我们将在文章中讨论的一些要点。**

- 脚本解释器
- 变量
- 用户输入
- 测试
- 条件判断
- 循环语句
- 脚本参数传递
- 退出状态码
- 逻辑操作符
- 函数
- 通配符
- 调试

所以，这将是我们讨论的顺序，在本文的最后，我相信你会有足够的信心编写自己的shell脚本:)

**Happy Learning Guys**

![img](Shell%E6%95%99%E7%A8%8B.assets/v2-1635d37cd8b895d3b6eefa1982d78705_720w.jpg)

## 脚本解释器

你可以从上面脚本的第一行看到 `#!/bin/bash` 这行指定了你的程序将使用那个解释器，基本上是将路径引用到解释器。Linux/Unix中有很多解释器，其中一些是：bash，zsh，sh，csh和ksh等。

这里推荐一个玩命令行必须知道的一个开源项目[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)



![img](Shell%E6%95%99%E7%A8%8B.assets/v2-2030b13b493e820084fed216b5f8ac12_720w.jpg)



> All the best people in life seem to like LINUX. — Steve Wozniak

查看你的系统中有那些脚本解释器

```bash
cat /etc/shells
```

bash: `#!/bin/bash`zsh: `#!/bin/zsh`ksh: `#!/bin/ksh`csh: `#!/bin/csh`and so on…

**注意⚠️**如果脚本不包含解释器，则使用你的默认shell执行命令，因此代码可能正常运行，虽然是这样，但是不推荐这样做，使用`echo $SHELL`可以知道你当前使用的解释器

**注释**注释以`#`开始，`#`后面的内容会被解释器忽略，但是`#!`另当别论

**变量**变量指向内存中的一块区域，变量有对应的变量名和值，可以存储一些可以在将来更改的数据，shell中定义变量不需要指定变量的类型

```bash
VARIABLE_NAME="Value"
```

当命名一个变量是你必须记得以下几点 *变量名是区分大小写的*
为了方便，变量名最好大写
\* 要使用变量，必须在变量前面加`$`符号

**例子🌰**

```bash
#!/bin/bash
MY_NAME="shellhub"
echo "Hello, I am $MY_NAME"
```

OR

```bash
#!/bin/bash
MY_NAME="shellhub"
echo "Hello, I am ${MY_NAME}"
```

**提示:** 可以把命令执行后的输入结果赋值给一个变量

```bash
LIST=$(ls)
SERVER_NAME=$(hostname)
```



![img](Shell%E6%95%99%E7%A8%8B.assets/v2-5ef272a5d93053773f68919fda4a9dbb_720w.jpg)



**合法的变量名**

```bash
THIS3VARIABLE=”ABC”
THIS_IS_VARIABLE=”ABC”
thisIsVariable=”ABC”
```

**不合法的变量名**

```bash
4Number=”NUM”
This-Is-Var=”VAR”
# No special character apart from underscore is allowed!
```

## 用户输入

`read` 命令接收键盘的输入，标准输入(Standard Input)

```bash
read -p "PROMPT MESSAGE" VARIABLE
```

其中`PROMPT MESSAGE`为提示用户的信息，变量`VARIABLE`可以保存用户的输入，可以在程序中使用该变量

```bash
#!/bin/bash
read -p "Please Enter You Name: " NAME
echo "Your Name Is: $NAME"
```

## 测试

测试主要用于条件判断。`[ condition-to-test-for ]` ，如`[ -e /etc/passwd ]`，注意的是`[]`前后必须有空格，如`[-e /etc/passwd]`是错误的写法

1. 文件测试操作

```bash
-d FILE_NAM  # True if FILE_NAM is a directory
-e FILE_NAM  # True if FILE_NAM exists
-f FILE_NAM  # True if FILE_NAM exists and is a regular file
-r FILE_NAM  # True if FILE_NAM is readable
-s FILE_NAM  # True if FILE_NAM exists and is not empty
-w FILE_NAM  # True if FILE_NAM has write permission
-x FILE_NAM  # True if FILE_NAM is executable
```

1. 字符串测试操作

```bash
-z STRING  # True if STRING is empty
-n STRING  # True if STRING is not empty
STRING1 = STRIN2 # True if strings are equal
STRING1 != STRIN2 # True if strings are not equal
```

1. 算术测试操作

```bash
var1 -eq var2  # True if var1 is equal to var2
var1 -ne var2  # True if var1 not equal to var2
var1 -lt var2  # True if var1 is less than var2
var1 -le var2  # True if var1 is less than or equal to var2
var1 -gt var2  # True if var1 is greater than var2
var1 -ge var2  # True if var1 is greater than or equal to var2
```

## 条件判断

和其他编程语言一样，shell脚本也能基于条件进行判断，我们可以使用`if-else`或`if-elif-else`



![img](Shell%E6%95%99%E7%A8%8B.assets/v2-847fab2faf2ddc12d1099019e5168e61_720w.jpg)



> Avoid the Gates of Hell. Use Linux!

1. `if` 语句

```bash
if [ condition-is-true ]
then
  command 1
  command 2
    ...
    ...
  command N
fi
if-else
if [ condition-is-true ]
then
  command 1
elif [ condition-is-true ]
then
  command 2
elif [ condition-is-true ]
then
  command 3
else
  command 4
fi
```

```bash
a=10
b=20
if [ $a == $b ]
then
   echo "a 等于 b"
elif [ $a -gt $b ]
then
   echo "a 大于 b"
elif [ $a -lt $b ]
then
   echo "a 小于 b"
else
   echo "没有符合的条件"
fi
```



1. `case`语句`case`可以实现和`if`一样的功能，但是当条件判断很多的时候，使用`if`不太方便，比如使用`if`进行值的比较

```bash
case "$VAR" in
  pattern_1)
    # commands when $VAR matches pattern 1
    ;;
  pattern_2)
    # commands when $VAR matches pattern 2
    ;;
  *)
    # This will run if $VAR doesnt match any of the given patterns
    ;;
esac
```

**例子🌰**

```bash
#!/bin/bash
read -p "Enter the answer in Y/N: " ANSWER
case "$ANSWER" in
  [yY] | [yY][eE][sS])
    echo "The Answer is Yes :)"
    ;;
  [nN] | [nN][oO])
    echo "The Answer is No :("
    ;;
  *)
    echo "Invalid Answer :/"
    ;;
esac
```

## 迭代语句 - 循环

可以通过循环执行同一个代码块很多次
\1. `for`循环

```bash
for VARIABLE_NAME in ITEM_1 ITEM_N
do
  command 1
  command 2
    ...
    ...
  command N
done
```

**Example**

```bash
#!/bin/bash
COLORS="red green blue"
for COLOR in $COLORS
do
  echo "The Color is: ${COLOR}"
done
```

**Another Example**

```bash
for (( VAR=1;VAR<N;VAR++ ))
do
  command 1
  command 2
    ...
    ...
  command N
done
```

在当前所有txt文件前面追加`new`实现重命名

```bash
#!/bin/bash
FILES=$(ls *txt)
NEW="new"
for FILE in $FILES
do
  echo "Renaming $FILE to new-$FILE"
  mv $FILE $NEW-$FILE
done
```

1. `while` 循环
   当所给的条件为`true`时，循环执行`while`里面的代码块

```bash
while [ CONNDITION_IS_TRUE ]
do
  # Commands will change he entry condition
  command 1
  command 2
    ...
    ...
  command N
done
```

判断条件可以是任意的测试或者命令，如果测试或命令返回0，则表示条件成立，如果为非0则退出循环，如果一开始条件就不成立，则循环永远不会执行。
如果你不知道退出状态码是什么请不要担心，我后面会告诉你 :)



![img](Shell%E6%95%99%E7%A8%8B.assets/v2-8adac6e7ff7fb41607b2635fbdf7b4cd_720w.jpg)



**例子** 一行一行读取文件内容

```bash
#!/bin/bash
LINE=1
while read CURRENT_LINE
do
  echo "${LINE}: $CURRENT_LINE"
  ((LINE++))
done < /etc/passwd
# This script loops through the file /etc/passwd line by line
```

**注意⚠️**`continue` 用于结束本次循环`break` 用于结束整个循环

## 参数传递

当我们运行脚本的时候，可以传递参数供脚本内部使用`$ ./script.sh param1 param2 param3 param4`这些参数将被存储在特殊的变量中

```bash
$0 -- "script.sh"
$1 -- "param1"
$2 -- "param2"
$3 -- "param3"
$4 -- "param4"
$@ -- array of all positional parameters
```

这些变量可以在脚本中的任何地方使用，就像其他全局变量一样

## 退出状态码

任何一个命令执行完成后都会产生一个退出状态码，范围`0-255`，状态码可以用来检查错误 *0 表示正确执行并正常退出*
非0表示执行过程中出错，没有正常退出

上一条命令执行后的退出状态码被保存在变量`$?`中

**例子** 使用`ping`检查主机和服务器之间是否可以抵达

```bash
#!/bin/bash
HOST="google.com"
ping -c 1 $HOST     # -c is used for count, it will send the request, number of times mentioned
RETURN_CODE=$?
if [ "$RETURN_CODE" -eq "0" ]
then
  echo "$HOST reachable"
else
  echo "$HOST unreachable"
fi
```

**自定义退出状态码**默认的状态码是上一条命令执行的结果，我们可以通过`exit`来自定义状态码

```bash
exit 0
exit 1
exit 2
  ...
  ...
exit 255
```

## 逻辑操作符

shell脚本支持逻辑与和逻辑或
逻辑与 `&&`逻辑或 `||`

Example`mkdir tempDir && cd tempDir && mkdir subTempDir`这个例子中，如果创建tempDir成功，执行后面的`cd`，继续创建subTempDir

## 函数

可以把一些列的命令或语句定义在一个函数内，从程序的其他地方调用



![img](Shell%E6%95%99%E7%A8%8B.assets/v2-90db711db7efb9d8b74ec3862ac60b80_720w.jpg)



**注意⚠️** *函数包含了一些列你要重复调用的指令(函数必须先定义后调用)*
把函数定义在程序开始或主程序之前是一个最佳实践

**语法**

```bash
function function_name() {
    command 1
    command 2
    command 3
      ...
      ...
    command N
}
```

**调用函数** 简单的给出函数名字

```bash
#!/bin/bash
function myFunc () {
    echo "Shell Scripting Is Fun!"
}
myFunc # call
```

## 函数参数传递

和脚本一样，也可以给函数传递参数完成特殊的任务，第一个参数存储在变量`$1`中，第二个参数存储在变量`$2`中...，`$@`存储所有的参数，参数之间使用空格分割 `myFunc param1 param2 param3 ...`

**变量的作用范围**

**全局变量:** 默认情况下，shell中的变量都定义为全局变量，你可以从脚本中的任何位置使用变量，但是变量在使用之前必须先定义**本地变量:** 本地变量只能在方法内部访问，可以通过`local`关键词定义一个本地变量，定义一个本地变量的最佳实践是在函数内部

## 通配符

使用通配符可以完成特定的匹配
一些常用的通配符`*` 可以通配一个或多个任意字符

```bash
*.txt
hello.*
great*.md
```

`?`匹配一个字符

```bash
?.md
Hello?
```

`[]`匹配括号内部的任意一个字符

```bash
He[loym], [AIEOU]
```

`[!]`不匹配括号内的任何字符

```bash
`[!aeiou]`
```

**预先定义的通配符** *[[:alpha:]]*
[[:alnum:]] *[[:space:]]*
[[:upper:]]] *[[:lower:]]*
[[:digit:]]

**匹配通配符** 有些情况下我们想匹配`*`或`?`等特殊字符，可以使用转义字符`\*\?`

## 调试





bash提供一些可选项帮助你调试程序**Some Options:**1. `-x` option
它在执行时打印命令和参数。它被称为打印调试，跟踪或x跟踪。我们可以通过修改第一行来使用它
\2. `-e` option
它代表“出错”。如果命令以非零退出状态退出，这将导致脚本立即退出。
\3. `-v` option
它在读取时打印shell命令/输入行。

**注意:**这些选项可以组合使用，一次可以使用多个选项！

```bash
#!/bin/bash-xe
#!/bin/bash-ex
#!/bin/bash-x-e
#!/bin/bash-e-x
```

尽管有许多工具可用于帮助调试，但只有在了解其工作原理的情况下才能调试代码。所以，确保花足够的时间实际练习编写脚本并自己调试它们，这样你就可以知道你可以犯错误的地方，这样你就不会再这样做:)
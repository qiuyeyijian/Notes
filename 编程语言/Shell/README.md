## Shell

### sh/bash/csh/Tcsh/ksh/pdksh等shell的区别

- **sh(全称 Bourne Shell):**  是UNIX最初使用的 shell，而且在每种 UNIX 上都可以使用。

  Bourne Shell 在 shell 编程方面相当优秀，但在处理与用户的交互方面做得不如其他几种 shell。

- **bash（全称 Bourne Again Shell）**: LinuxOS 默认的，它是 Bourne Shell 的扩展。 与 Bourne Shell 完全兼容，并且在 Bourne Shell 的基础上增加了很多特性。可以提供命令补全，命令编辑和命令历史等功能。它还包含了很多 C Shell 和 Korn Shell 中的优点，有灵活和强大的编辑接口，同时又很友好的用户界面。

- **csh(全称 C Shell)**: 是一种比 Bourne Shell更适合的变种 Shell，它的语法与 C 语言很相似。

- **Tcsh**: 是 Linux 提供的 C Shell 的一个扩展版本。
  Tcsh 包括命令行编辑，可编程单词补全，拼写校正，历史命令替换，作业控制和类似 C 语言的语法，他不仅和 Bash Shell 提示符兼容，而且还提供比 Bash Shell 更多的提示符参数。

- **ksh (全称 Korn Shell)**: 集合了 C Shell 和 Bourne Shell 的优点并且和 Bourne Shell 完全兼容。

-  **pdksh**: 是 Linux 系统提供的 ksh 的扩展。
  pdksh 支持人物控制，可以在命令行上挂起，后台执行，唤醒或终止程序。

> 1、**chmod +x file** 加上执行权限，否则会提示无执行权限。
>
> 2、注意执行脚本时候或者全目录，或者 **./file.sh** ，如果不加的话，linux 默认会从PATH 里去找该 file.sh。
>
> 3、看了这篇教程，发现脚本后缀名可以任意修改，仍然可以正常运行。
>
> 4、语法类PHP，方便学习。

```bash
#!/bin/bash
echo "Hello World !"
```



### 常用预定义变量



| 预定义变量 | 说明                                              |
| ---------- | ------------------------------------------------- |
| $#         | 传递到脚本或函数的参数数量                        |
| $*         | 传递到脚本或函数的全部参数                        |
| $?         | 前一个命令执行情况，返回0表示成功，其他值表示失败 |
| $$         | 当前进程的ID                                      |
| $0         | 当前脚本名称                                      |



### Shell脚本调试

```bash
sh -n xx.sh					//不执行脚本，仅检查脚本语法问题
sh -v xx.sh					//将执行过的每个脚本命令都原样输出到终端
sh -x xx.sh					//在执行过程中的每个命令的行首添加 “+” 号显示在终端，并显示变量								//的值。使用该选项可以更方便跟踪程序的执行过程
```



### shell 字符串

> shell 命名赋值时，变量名和等号之间不能有空格 `str="example"`

单引号字符串的限制：

- 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
- 单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。

双引号的优点：

- 双引号里可以有变量
- 双引号里可以出现转义字符



###  shell 字符串截取



#### "#", "##", "%", "%%"

**#**、**##** 表示从左边开始删除。一个 **#** 表示从左边删除到第一个指定的字符；两个 **#** 表示从左边删除到最后一个指定的字符。

**%**、**%%** 表示从右边开始删除。一个 **%** 表示从右边删除到第一个指定的字符；两个 **%** 表示从左边删除到最后一个指定的字符。

删除包括了指定的字符本身。

Linux 的字符串截取很有用。有八种方法。

假设有变量 var=http://www.aaa.com/123.htm

**1. # 号截取，删除左边字符，保留右边字符。**

```
echo ${var#*//}
```

其中 var 是变量名，# 号是运算符，*// 表示从左边开始删除第一个 // 号及左边的所有字符

即删除 http://

结果是 ：www.aaa.com/123.htm

**2. ## 号截取，删除左边字符，保留右边字符。**

```
echo ${var##*/}
```

\##*/ 表示从左边开始删除最后（最右边）一个 / 号及左边的所有字符

即删除 http://www.aaa.com/

结果是 123.htm

**3. %号截取，删除右边字符，保留左边字符**

```
echo ${var%/*}
```

%/* 表示从右边开始，删除第一个 / 号及右边的字符

结果是：http://www.aaa.com

**4. %% 号截取，删除右边字符，保留左边字符**

```
echo ${var%%/*}
```

%%/* 表示从右边开始，删除最后（最左边）一个 / 号及右边的字符

结果是：http:

**5. 从左边第几个字符开始，及字符的个数**

```
echo ${var:0:5}
```

其中的 0 表示左边第一个字符开始，5 表示字符的总个数。

结果是：http:

**6. 从左边第几个字符开始，一直到结束。**

```
echo ${var:7}
```

其中的 7 表示左边第8个字符开始，一直到结束。

结果是 ：www.aaa.com/123.htm

**7. 从右边第几个字符开始，及字符的个数**

```
echo ${var:0-7:3}
```

其中的 0-7 表示右边算起第七个字符开始，3 表示字符的个数。

结果是：123

**8. 从右边第几个字符开始，一直到结束。**

```
echo ${var:0-7}
```

表示从右边第七个字符开始，一直到结束。

结果是：123.htm

**注：**（左边的第一个字符是用 0 表示，右边的第一个字符用 0-1 表示）



### read命令用于获取键盘输入信息

它的语法形式一般是：

```
read [-options] [variable...]
```

以下实例读取键盘输入的内容并将其赋值给shell变量，为：-p 参数由于设置提示信息：

```
read -p "input a val:" a    #获取键盘输入的 a 变量数字
read -p "input b val:" b    #获取键盘输入的 b 变量数字
r=$[a+b]                    #计算a+b的结果 赋值给r  不能有空格
echo "result = ${r}"        #输出显示结果 r
```

测试结果：

```
input a val:1
input b val:2
result = 3
```



### Shell 传递参数

我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：**$n**。**n** 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推……

```
#!/bin/bash

echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```

另外，还有几个特殊字符用来处理参数：

| 参数处理 | 说明                                                         |
| :------- | :----------------------------------------------------------- |
| $#       | 传递到脚本的参数个数                                         |
| $*       | 以一个单字符串显示所有向脚本传递的参数。 如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。 |
| $$       | 脚本运行的当前进程ID号                                       |
| $!       | 后台运行的最后一个进程的ID号                                 |
| $@       | 与$*相同，但是使用时加引号，并在引号中返回每个参数。 如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。 |
| $-       | 显示Shell使用的当前选项，与[set命令](https://www.runoob.com/linux/linux-comm-set.html)功能相同。 |
| $?       | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。 |



在为shell脚本传递的参数中**如果包含空格，应该使用单引号或者双引号将该参数括起来，以便于脚本将这个参数作为整体来接收**。

在有参数时，可以使用对参数进行校验的方式处理以减少错误发生：

```bash
if [ -n "$1" ]; then
    echo "包含第一个参数"
else
    echo "没有包含第一参数"
fi
```





### Shevimll 中 let 和 expr 执行表达式

1. let 语法格式

```bash
let 表达式

let nu++
let nu+=10
let a=1+2
```

2. let 和 expr 的区别

let 不需要空格隔开表达式的各个字符。而 expr 后面的字符需要空格隔开各个字符。

```bash
# expr 实例
$ s=`expr 2 + 3`
$ echo $s

5

# let 实例
$ let s=(2+3)*4
$ echo $s

20

```

**expr** 后面的括号 **()**, 乘法 ***** 符号需要用 **\** 转译，且用空格隔开:

```bash
# expr 实例
$ s=`expr \( 2 + 6 \) \* 3`
$ echo $s
24
```







###  把命令执行后的输入结果赋值给一个变量

```bash
LIST=$(ls)
SERVER_NAME=$(hostname)
```





### 自动输入回车或者输入yes/no

#### 方法一

需要单独安装TCL的Expect, 平台通用性不高，比较麻烦。

```bash
expect <<EOF
set timeout -1
spawn ssh-keygen -t rsa
expect "(/root/.ssh/id_rsa):"
send "\r"
expect "(empty for no passphrase):"
send "\r"
expect "again:"
send "\r"
expect eof
EOF
```

#### 方法二

```bash
echo [要回答的内容] | [要执行的脚本]
```



例如要执行一个脚本`centos.sh`, 这个脚本执行过程中要求执行回车，则可以：

```bash
echo | centos.sh
```

如果要求输入`yes/no`，则可以：

```bash
echo yes | centos.sh
```



### 比较判断

#### 字符串

| 参数      | 说明                     |
| :-------- | :----------------------- |
| =         | 等于则为真               |
| !=        | 不相等则为真             |
| -z 字符串 | 字符串的长度为零则为真   |
| -n 字符串 | 字符串的长度不为零则为真 |



#### 数字

| 参数 | 说明           |
| :--- | :------------- |
| -eq  | 等于则为真     |
| -ne  | 不等于则为真   |
| -gt  | 大于则为真     |
| -ge  | 大于等于则为真 |
| -lt  | 小于则为真     |
| -le  | 小于等于则为真 |



* 文件测试操作

```bash
-d FILE_NAM  # 是文件夹则为真
-e FILE_NAM  # 文件存在则为真
-f FILE_NAM  # 文件存在并且是一个普通文件则为真
-r FILE_NAM  # 文件可读则为真
-s FILE_NAM  # 文件存在并且非空则为真
-w FILE_NAM  # 文件可写则为真
-x FILE_NAM  # 文件可执行则为真
```

* 字符串测试操作

```bash
-z STRING  # 字符串为空则为真
-n STRING  # 字符串非空则为真
STRING1 = STRIN2 # 字符串相等则为真
STRING1 != STRIN2 # 字符串不相等则为真
```

* 算术测试操作

```bash
var1 -eq var2  # 变量1等于变量2则为真
var1 -ne var2  # 变量1等于变量2则为真
var1 -lt var2  # 变量1小于变量2则为真
var1 -le var2  # 变量1小于或等于变量2则为真
var1 -gt var2  # 变量1大于变量2则为真
var1 -ge var2  # 大于或等于
```



* 举例测试

```bash
#! /bin/bash

file="/home/qiuyeyijian/test.sh"

if [ -d ${file} ]; then
	echo "是文件夹"
else
	echo "不是文件夹"
fi

if [ -e ${file} ]; then
	echo "文件存在"
else
	echo "文件不存在"
fi

...	
```









### 条件控制语句

#### if 语句

* if

```bash
if [ condition-is-true ]
then
  command 1
  command 2
    ...
    ...
  command N
fi
```

* if-else

```bash
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

* 例子:ice_cream:


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



#### case 语句

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

* 例子🌰

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



### 循环语句

#### for 循环

1. `for`循环

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

#### while 循环
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



### Shell 数组

1. 定义

```bash 
array_name=(value0 value1 ...)				//注意是空格隔开，而不是逗号
或者
array_name[0]=value0
array_name[1]=value1
...
```

2. 获取

```
${array_name[index]}				//获取数组中某个元素    
${array_name[@]}					//获取数组中的全部元素
${array_name[*]}					//获取数组中的全部元素

${#array_name[@]}					//获取数组中的全部元素
${#array_name[*]}					//获取数组中的全部元素
```

3. 遍历数组

```bash
#! /bin/bash

# 定义一个数组
arr=(1 2 3 4 5)

# For循环
# ${arr[@]}之所以要加双引号是因为避免数组元素中有不连续变量，导致打印错误（如：“zhang san"）
for i in “${arr[@]}”;do
	echo ${i}
done



# While 循环
j=0
while [ $j -lt ${#arr[@]} ];do
	echo ${arr[j]}
	let j++
done

```





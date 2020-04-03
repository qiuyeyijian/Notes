## Shell



### 1. sh/bash/csh/Tcsh/ksh/pdksh等shell的区别

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



### 2. shell 字符串

> shell 命名赋值时，变量名和等号之间不能有空格 `str="example"`

单引号字符串的限制：

- 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
- 单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。

双引号的优点：

- 双引号里可以有变量
- 双引号里可以出现转义字符



### 3. shell 字符串截取



#### "#", "##", "%", "%%"

**#**、**##** 表示从左边开始删除。一个 **#** 表示从左边删除到第一个指定的字符；两个 **#** 表示从左边删除到最后一个指定的字符。

**%**、**%%** 表示从右边开始删除。一个 **%** 表示从右边删除到第一个指定的字符；两个 **%** 表示从左边删除到最后一个指定的字符。

删除包括了指定的字符本身。



### 4. **read命令用于获取键盘输入信息**

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
### 一、实验预习报告

#### 1、实验相关知识的简述

shell是UNIX/Linux系统的一个重要层次，它是用户与系统交互的界面。它是一种高级程序设计语言，有相应的变量、关键字、控制语句，有自己的语法结构。

#### 2、实验原理的预习情况

将shell程序设计语言与Linux命令结合可以极大地提高工作效率。试验之前了解shell的特点和主要种类。掌握shell脚本的建立和执行方式。

#### 3. 实验注意事项

注意shell语法与c++与java的不同之处，注意语法结构问题，防止出现意外错误。

### 二、实验报告

#### 1、实验目的与要求

1. 了解shell的特点和主要种类。

2. 掌握shell脚本的建立和执行方式。

3. 掌握bash的基本语法。

4. 学会编写shell脚本。

#### 2、实验仪器或材料

1. Windows 10操作系统
2. Vmware 15 安装的Centos 8 虚拟机
3. Moba Xterm SSH终端软件

#### 3、实验原理

1.使用系统调用对文件进行操作。

2.使用系统调用对进程进行控制。

3.使用管道机制进行I/O。

4.使用信号机制进行进程通信。

#### 4、实验过程及数据记录

1. 打印九九乘法表

```c
#!/bin/bash
for i in `seq 9`
do
  	for j in `seq $i`
   	do
       	echo -n "$j*$i=$[i*j]  "
   	done
    echo
done
```

![image-20200618215138680](%E5%AE%9E%E9%AA%8C%E5%9B%9B%20shell%E7%BC%96%E7%A8%8B.assets/image-20200618215138680.png)



2. 打印国际棋盘

```c
#!/bin/bash
# 打印国际象棋棋盘
# 设置两个变量,i 和 j,一个代表行,一个代表列,国际象棋为 8*8 棋盘
# i=1 是代表准备打印第一行棋盘,第 1 行棋盘有灰色和蓝色间隔输出,总共为 8 列
# i=1,j=1 代表第 1 行的第 1 列;i=2,j=3 代表第 2 行的第 3 列
# 棋盘的规律是 i+j 如果是偶数,就打印蓝色色块,如果是奇数就打印灰色色块
# 使用 echo ‐ne 打印色块,并且打印完成色块后不自动换行,在同一行继续输出其他色块
for i in {1..8}
do
  	for j in {1..8}
  	do
  		sum=$[i+j]
		if [  $[sum%2] -eq 0 ];then
 			echo -ne "\033[46m  \033[0m"
		else
			echo -ne "\033[47m  \033[0m"
		fi
  	done
  	echo
done
```



![image-20200618215334429](%E5%AE%9E%E9%AA%8C%E5%9B%9B%20shell%E7%BC%96%E7%A8%8B.assets/image-20200618215334429.png)



3. 对输入的三个数排序

```c
#!/bin/bash
# 依次提示用户输入 3 个整数,脚本根据数字大小依次排序输出 3 个数字
read -p "请输入一个整数:" num1
read -p "请输入一个整数:" num2
read -p "请输入一个整数:" num3
# 不管谁大谁小,最后都打印 echo "$num1,$num2,$num3"
# num1 中永远存最小的值,num2 中永远存中间值,num3 永远存最大值
# 如果输入的不是这样的顺序,则改变数的存储顺序,如:可以将 num1 和 num2 的值对调
tmp=0
# 如果 num1 大于 num2,就把 num1 和和 num2 的值对调,确保 num1 变量中存的是最小值
if [ $num1 -gt $num2 ];then   
	tmp=$num1
	num1=$num2
	num2=$tmp
fi
# 如果 num1 大于 num3,就把 num1 和 num3 对调,确保 num1 变量中存的是最小值
if [ $num1 -gt $num3 ];then   
  	tmp=$num1
  	num1=$num3
  	num3=$tmp
fi
# 如果 num2 大于 num3,就把 num2 和 num3 对标,确保 num2 变量中存的是小一点的值
if [ $num2 -gt $num3 ];then
  	tmp=$num2
  	num2=$num3
  	num3=$tmp
fi
echo "排序后数据(从小到大)为:$num1,$num2,$num3"
```

![image-20200618215658852](%E5%AE%9E%E9%AA%8C%E5%9B%9B%20shell%E7%BC%96%E7%A8%8B.assets/image-20200618215658852.png)



#### 5、实验结果分析

通过编写shell程序，可以将一些繁琐的工作简单化。同时可以实现强大的管理功能。

### 三、实验总结

通过本次实验，我熟悉了shell基本编程语法，shell语言的控制结构。可以通过编写简单的shell程序来管理Linux系统。通过实验的几个内容，我体会到了shell语言的开放和强大。


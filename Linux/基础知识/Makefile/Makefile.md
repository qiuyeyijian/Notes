# Makefile



### make/makefile/cmake/qmake

> 1. make 是用来执行Makefile的
> 2. Makefile是类unix环境下(比如Linux)的类似于批处理的"脚本"文件。其基本语法是: **目标+依赖+命令**，只有在**目标**文件不存在，或**目标**比**依赖**的文件更旧，**命令**才会被执行。由此可见，Makefile和make可适用于任意工作，不限于编程。比如，可以用来管理latex。
> 3. Makefile+make可理解为类unix环境下的项目管理工具，但它太基础了，抽象程度不高，而且在windows下不太友好(针对visual studio用户)，于是就有了跨平台项目管理工具cmake
> 4. cmake是跨平台项目管理工具，它用更抽象的语法来组织项目。虽然，仍然是目标，依赖之类的东西，但更为抽象和友好，比如你可用math表示数学库，而不需要再具体指定到底是math.dll还是libmath.so，在windows下它会支持生成visual studio的工程，在linux下它会生成Makefile，甚至它还能生成eclipse工程文件+-
> 4. 也就是说，从同一个抽象规则出发，它为各个编译器定制工程文件。
> 5. cmake是抽象层次更高的项目管理工具，cmake命令执行的CMakeLists.txt文件
> 6. qmake是Qt专用的项目管理工具，对应的工程文件是*.pro，在Linux下面它也会生成Makefile，当然，在命令行下才会需要手动执行qmake，完全可以在qtcreator这个专用的IDE下面打开*.pro文件，使用qmake命令的繁琐细节不用你管了。

**总结一下，make用来执行Makefile，cmake用来执行CMakeLists.txt，qmake用来处理*.pro工程文件。Makefile的抽象层次最低，cmake和qmake在Linux等环境下最后还是会生成一个Makefile。cmake和qmake支持跨平台，cmake的做法是生成指定编译器的工程文件，而qmake完全自成体系。**

具体使用时，Linux下，小工程可手动写Makefile，大工程用automake来帮你生成Makefile，要想跨平台，就用cmake。如果GUI用了Qt，也可以用qmake+*.pro来管理工程，这也是跨平台的。当然，cmake中也有针对Qt的一些规则，并代替qmake帮你将qt相关的命令整理好了。

另外，需要指出的是，make和cmake主要命令只有一条，make用于处理Makefile，cmake用来转译CMakeLists.txt，而qmake是一个体系，用于支撑一个编程环境，它还包含除qmake之外的其它多条命令(比如uic，rcc,moc)。

上个简图，其中cl表示visual studio的编译器，gcc表示linux下的编译器



### Makefile 基本知识

```makefile
# 可以设置变量，使用的时候需要用 $()
CC = gcc

main: main.c foo.o tool.o
        $(CC) main.c foo.o tool.o -o main
foo.o: foo.c
        gcc -c foo.c
tool.o: tool.c
        gcc -c tool.c
clean:
        rm *.o main

```



### Makefile自动化变量

```makefile
main: main.o a.o b.o
	gcc main.o a.o b.o -o main
main.o: main.c
	gcc -c main.c -o main.o
a.o: a.c
	gcc -c a.c -o a.o
b.o: b.c
	gcc -c b.c -o b.o

clean:
	rm *.o main
```

上面可以使用自动化变量代替，可以简化Makefile编写

```makefile
main: main.o a.o b.o
	gcc *.o -o $@	# $@ 匹配要生成的目标 main
%.o:%.c
	gcc -c $< -o $@ # $< 首先取出main.c 然后目标$@就是 main.o 执行cc -c操作
					# 接着$< 取出a.c 然后目标 $@ 就是 a.o 执行gcc -c操作。
					# ...
clean:
	rm *.o main
```

> $@ : 表示规则中的目标文集。在模式规则中，如果有多个目标，那么，​\$@ 就是匹配于目标中模式定义的集合。
>
> \$< :一个个取出依赖，和\$@ 相匹配，匹配成功则执行操作。



### 命令参数变量

一般看到Makefile中都有`${CFLAGS}, ${CXXFLAGS}`等参数，这些表示编译器的参数，比如`-g, -c, -o`等都是参数。如果没有指明其值，则默认值为空

> * CFLAGS:  C语言编译器参数
> * CXXFLAGS：C++语言编译器参数
>
> ...



### Reference

https://www.zhihu.com/question/27455963/answer/36722992

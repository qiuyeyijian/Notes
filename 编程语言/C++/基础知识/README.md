# C++

## 语言特性

**基于对象的程序设计**：把功能包含到类中，定义一个类对象并通过该对象调用各种成员函数实现各种功能的程序书写方式

**面向对象的程序设计**：把继承性和多态性技术融入基于对象的程序设计中

>不同的C++编译器会使用不同的文件后缀名：
>
>* `.h、.cpp`是一般常见的后缀名
>* `.cc、.cxx`一般在GNU编译器上比较常见
>* `.m`文件是纯Object-C 文件。`.mm`是Object-C和C++混合文件

C++头文件一般以`.h`居多，但是也有`.hpp`。`.hpp`一般来讲就是把定义和实现都包含在一个文件里，有一些公共开源库就是这样做，**主要是能有效减少编译次数**。

在C++98标准之后，很多熟悉的C语言头文件，在C++中都转变成去掉`.h`，并以`c`开头，例如`cstdio`



## 命名空间

命名空间就是为了防止名字冲突而引入的一种机制。系统中可以定义多个命名空间，**每个命名空间不可以同名。**可以把命名空间看成一个作用域，这个命名空间定义的函数与另一个命名空间定义的函数，即便同名也互不影响。

**命名空间的定义可以不连续，可以写在不同的位置，甚至写在不同的源文件中。**

* 如果以前没有定义命名空间`test01`,那么相当于定义一个新的命名空间`test01`。
* 如果以前已经定义了命名空间`test01`,那么当再次定义时，相当于打开该命名空间并追加内容

```cpp
#include <cstdio>
#include <iostream>
#include <string>

namespace ns01 {
void log() { printf("ns01\n"); }
}  // namespace ns01

namespace ns02 {
void log() { printf("ns02\n"); }
}  // namespace ns02

// 再次定义时相当于往ns01中追加内容
namespace ns01 {
void show() { printf("ns01\n"); }
}  // namespace ns01

int main(int argc, char const *argv[]) {
  ns01::log();
  ns02::log();

  // using namespace 也可以写在函数体内，只在函数体内有效
  using namespace ns01;
  show();

  return 0;
}
```



### 全局命名空间

```cpp
string getColor() { return "#ffffff"; }

class Wechat {
 private:
  string color;

 public:
  Wechat &getColor() {
    // 双冒号前面为空表示使用全局命名空间
    // 这样可以使用同名的全局函数，否则会报错
    color = ::getColor();
    return *this;
  }
};
```









## 变量声明

C++11新标准，可以使用`{}`在定义变量的时候初始化，也可以使用`()`，省略`=`。

```cpp
int a{5};
int arr[3]{1,2,3};

int b(5);


int c{3.14};	// 错误，不会发生隐式类型转换，直接报错，提高程序健壮性
```



## 基本输入/输出

### std::cout

```cpp
ostream& std::cout.operator << (...);		// << 函数重载定义
```

`<<`原本是左移运算符，但是和`cout`连在一起时发生了运算符重载，成了“输出运算符”。

对于`A << B `，`<<`可以看成一个函数调用，`A`是第一个参数，`B`是第二个参数，作用是将B写到A中。



### std::endl

`std::endl`是一个函数模板名，相当于函数指针，建议暂时理解成函数。一般都在语句末尾，有两个作用：

1. 输出换行符`\n`
2. 刷新输出缓冲区。调用`flush`强制输出缓冲区中所有数据，然后把缓冲区中数据清除。（缓冲区的出现是为了解决内存和外设速度不匹配问题）





## auto、头文件防卫、引用与常量

### auto

auto 变量的自动类型推断，有时可以避免书写很长的类型名。

### 头文件防卫

避免重复包含头文件内容，引发编译错误

```cpp
#ifndef __HEAD01__
#define __HEAD01__
...
    
#endif
```

**注意：#ifndef 后面定义的名字不能重名**





## 结构体与类

在C语言中，定义一个属于某个结构的变量，会称为“结构变量”。在C++中，定义一个数据某个类的变量，称为“对象”。其实都是一块能够存储数据并具有某种类型的内存空间。

C++的结构体除了具备C中结构体的所有功能外，还增加许多扩展功能，最突出的就是不仅有成员变量，还可以定义成员函数（方法）。所以C++的结构体和类差别不是很大。

|              | C++结构体 | C++类   |
| ------------ | --------- | ------- |
| 默认访问级别 | public    | private |
| 继承默认是   | public    | private |
|              |           |         |

C++中如果定义结构或者类的成员变量和成员函数时，都明确写出访问级别，那么结构体和类就没什么区别。

在书写C++程序的时候，无论代码想实现一个什么样的功能，都应该设法通过写一个或多个类来达到目的，因为C++语言中最核心的部件就是类。



## 函数新特性、inline内联函数与const详解

### 前置/后置返回类型

函数的返回类型位于函数声明或者定义语句的开头，叫做“前置返回类型”。

```cpp
int func(int a, int b);
```

在C++11中还引入了一种新的语法，叫“后置返回类型”，也就是函数的返回类型位于函数声明或者定义语句的后面。对于有一些返回类型比较复杂的情形，这种写法可能更容易让人看懂。

```cpp
auto func(int a, int b) -> int;			// 函数声明
auto func(int a, int b) -> int {...}	// 函数定义
```

> 一个函数内的代码不要太长，不同功能尽量分解到多个函数中1去写。建议包含百十行代码就行。否则会增加阅读难度。



### inline

```cpp
inline int func(int a) {
    printf("%d\n", a);
}
```

函数的调用需要进行压栈、出栈动作以及处理函数调用和返回问题。对于一些函数体很小，调用有很频繁的函数所耗费的性能问题，引入了inline关键字。

在编译阶段完成对inline函数的处理，使用函数本体取代函数调用。inline函数要尽量简单，代码尽量少，尤其是各种复杂的循环、分支递归调用等。否则，编译器很可能因为代码复杂拒绝让这个函数成为内联函数。



### 函数特殊写法总结

1、不能返回局部变量的指针或者引用。

2、对于函数重载，const 在比较同名函数时会被忽略掉。

```cpp
void func(const int a){}
void func(int a) {}			// 构不成函数重载
```

3、函数形参中最高使用**常量引用**

```cpp
void func(const int &a)
```



### 常量指针与指针常量

**常量指针：**`const`在`*`之前，**指向的值不可以更改**，但指针的指向可以更改

**指针常量：**`const`在`*`之后，**指向的值可以更改**，但指针的指向不可以更改

**常量引用：** 一般用在形参列表里

```cpp
int main() {

	int a = 10;
	int b = 10;
    
    // 常量指针
	//const在*前，指向可以改（指向不同的内存区域）指针指向的值不可以更改（不能通过指针修改内存区域的值）
    // 也可定义为 int const* p1 = &a;
	const int * p1 = &a; 
    //*p1 = 100;  报错，不可以修改指向的值
	p1 = &b; //正确，可以修改指向
	
    
	//const修饰的是常量，指向不可以改(指向固定内存区域)，指针指向的值可以更改（内存区域存的值可以改变）
	int * const p2 = &a;
    *p2 = 100; //正确，可以修改指向的值
	//p2 = &b; //错误，不可以修改指向
	

    //const既修饰指针又修饰常量
	const int * const p3 = &a;
	//p3 = &b; //错误
	//*p3 = 100; //错误
    
	system("pause");

	return 0;
}
```

> 关于指针与常量，只要记着一点：指针常量，指针是常量



### 指针与函数

指针函数：返回类型是指针的函数

函数指针：指向函数的指针

```cpp
// addition是指针函数，一个返回类型是指针的函数
int* addition(int a, int b) {
  int* sum = new int(a + b);
  return sum;
}

int subtraction(int a, int b) { return a - b; }

int operation(int x, int y, int (*func)(int, int)) { return (*func)(x, y); }

// miuns是函数指针，指向函数的指针
int (*minus)(int, int) = subtraction;

int* m = addition(1, 2);
int n = operation(3, *m, minus);
```



## 迭代器

### reverse_iterator

```cpp
for (vector<int>::reverse_iterator rit = vect.rbegin(); rit != vect.rend(); rit++) {
    printf("%d ", *rit);
}
```



### const_iterator 

从感觉上来讲const_iterator像是一个常量指针，通过const_iterator只能读取容器内的元素。当容器对象是一个常量时，就必须用此迭代器。

```cpp
const std::vector<int> v = {1, 2, 3, 4, 5};
std::vector<int>::const_iterator cit;
for (cit = v.cbegin(); cit != v.cend(); ++cit) {
    printf("%d ", *cit);
}
```

C++11引入了`cbegin`和`cend`成员函数，与`begin`和`end`非常相似。但是不管容器是否是常量容器，`cbegin`和`cend`返回的都是常量迭代器`const_iterator`



### 迭代器失效

**在操作迭代器的过程中（使用了迭代器的之中循环体），千万不要试图改变容器对象的容量，也就是不要增加或者删除vector容器中的元素。**任何一种能够改变vector对象容量的操作都会使当前vector对象迭代器失效。

```cpp
std::vector<int> v = {1, 2, 3, 4};

for(auto it = v.begin(); it != v.end(); it++) {
    // 类似这种循环内千万不要改变容器的容量
}

// 循环删除容器的最佳操作
while(!v.empty()) {
    auto it = v.begin();
    v.erase(it);
}

// 或者直接调用函数
v.clear();
```



## 类型转换

C语言中有隐式和显示强制类型转发之分

```cpp
// 隐式
int x = 3.2;

// 显示
int a = (int)3.2;
int b = int(3.2);		// 函数风格
```

C++兼容C，并且强制类型转换分为4种：`static_cast`、`dynamic_cast`、`const_cast`、`reinterpret_cast`。

```cpp
强制转换类型名<type>(express)
```

### static_cast

静态转换，编译时类型检查。可以理解成正常的类型转换。属于编译的时候就会进行的类型转换检查。不要想复杂了。与C语言中强制类型转换差不多，程序员需要保证安全性和正确性。一般的编译器能够执行的隐式类型转换也都可以用`static_cast`来显示完成。

```cpp
  // 1. 整型和实型之间转换
  double f = 100.34f;
  int i = static_cast<int>(100.34f);

  // 2. 子类向父类转换
  class Father {};
  class Son : public Father {};
  Son son;
  Father father = static_cast<Father>(son);

  // 3. void* 与其他类型指针转换
  void* p = &i;
  int* q = static_cast<int*>(p);

  // 4. 不能用于指针类型转换，如int* 转 double* 、float* 转 double*
```



### dynamic_cast

动态转换，运行时类型检查。主要用来进行父类型转成子类型，检查的代价很昂贵，但也保证了安全性。



### const_cast

去除**指针或者引用**的const属性，编译时类型检查。`const_cast`功能单一，但其他函数无法替代。

```cpp
const int ai = 90;        // ai不是指针或者引用，所以不能用const_cast转
const int* pai = &ai;
int* pai2 = const_cast<int*>(pai);        // 正确
```



### reinterpret_cast

用来处理无关类型转换，编译时类型检查。可以将两个无关类型自由转换，相当随意，一般不轻易使用。

> 使用`reinterpret_cast`非常危险，而使用`const_cast`总是意味着设计缺陷。









## 其他

### 字符串分割，以空格为界

`istringstream` 是将字符串变成字符串迭代器一样，将字符串流依次拿出，比较好的是，它不会将空格作为流。这样就实现了字符串的空格切割。

```c++
#include <iostream>
#include <sstream>
using namespace std;

int main(int argc, char** argv) {
	istringstream str(" this is a   text");	
	string out;

	while (str >> out) {
		cout << out << endl;
	}
}
```



### "->" 和 "."的区别

`->`主要用于类类型的指针访问类的成员du，而`.`运算符，主要用于类类型的对象访问类的成员。

1、A.B则A为对象或者结构体；

2、A->B则A为指针，->是成员提取，A->B是提取A中的成员B，A只能是指向类、结构、联合的指针；

3、::是作用域运算符，A::B表示作用域A中的名称B，A可以是名字空间、类、结构；

4、：一般用来表示继承；





### 字符串与数字之间的转换

```c++
#include<iostream>
#include<sstream>
using namespace std;

int main() {
    
    //字符串转数字
    string str1 = "1234";		//单纯数字
    string str2 = "John 1234";	//字符串与数字混合
    int num1 = 0;
    
    istringstream istr1(str1);   
    istr1 >> num1;
    
    istringstream istr2(str2);
    string temp1;
    str2 >> temp1 >> 1234;
    
    
    //数字转字符串
    string str = to_string(25);		//单纯数字转字符串
    
    //数字和字符串的拼接，这里主要练习使用ostringstream
    string temp2 = "John";
    int num2 = 1234
    
    ostringstream ostr;
    ostr << temp2 << num2 << "\n";
    cout << ostr.str();
    
    
    return 0;
    
}
```



### cin.getline()函数有时候不起作用

用户如果之前使用过输入流 `cin` 来接收数据，这时候，`cin	`会留下一个换行符，如果此时用户再使用`cin.getline()`的话，就会发现好像系统自动 输入了，`cin.getline()`不起作用了

cin.getline默认以换行符为结束标志，要消耗掉前面的换行符需要多调用一次，如下所示即可

```c++
//使用两次cin.getline()
char Status[50];
cin.getline(Status, 50);		//第一次消耗掉之前的换行符
cin.getline(Status, 50);

//或者第一次使用cin.ignore()，忽略掉之前的空格
cin.ignore();
cin.getline(Status,50)

```



### C++ 打印特定精度的小数

```c++
#include<iostream>
#include<iomanip>	//可以使用 setprecision(m)函数, m表示整数和小数一共多少位
#include<stdio.h>
using namespace std;

int main() {
	int r;
	double PI = 3.14159265358979323;
	cin >> r;
	printf("%.7f", PI * r * r);
	//cout << setprecision(9) << PI * r * r << endl;
	
	return 0;
} 
```





### sscanf 和 sprintf

```c
int a, b;
char buff[100];

sprintf(buff, "%d", a);		// 将字符串buff转成int 
sscanf(buff, "%d", b);		//  将int 转成字符串buff
```



## union联合体（共用体）

```cpp
union Student {
  int method1;
  int method2;
  int method3;
  float method4;
};

int main(int argc, char const *argv[]) {
  Student stu;
  stu.method1 = 10;
  stu.method2 = 100;

  cout << stu.method3 << endl;
  //cout << stu.method4 << endl;	// 发生了隐式强制类型转化，导致结果有误
  return 0;
}
```

union联合体的属性只能存在一个，相当于给一个存储空间起了不同的别名。**这个存储空间的大小以需要最大存储空间的成员为准的，他们使用的是同一个空间。**可以想象成是一个变量有多个名字，我们可以用不同的名字去使用它们。

https://blog.csdn.net/huqinwei987/article/details/23597091



## C输出格式

| 控制符                    | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| %d                        | 按十进制整型数据的实际长度输出。                             |
| %ld                       | 输出长整型数据。                                             |
| %md                       | m 为指定的输出字段的宽度。如果数据的位数小于 m，则左端补以空格，若大于 m，则按实际位数输出。 |
| %u                        | 输出无符号整型（unsigned）。输出无符号整型时也可以用 %d，这时是将无符号转换成有符号数，然后输出。但编程的时候最好不要这么写，因为这样要进行一次转换，使 CPU 多做一次无用功。 |
| %c                        | 用来输出一个字符。                                           |
| %f                        | 用来输出实数，包括单精度和双精度，以小数形式输出。不指定字段宽度，由系统自动指定，整数部分全部输出，小数部分输出 6 位，超过 6 位的四舍五入。 |
| %.mf                      | 输出实数时小数点后保留 m 位，注意 m 前面有个点。             |
| %o                        | 以八进制整数形式输出，这个就用得很少了，了解一下就行了。     |
| %s                        | 用来输出字符串。用 %s 输出字符串同前面直接输出字符串是一样的。但是此时要先定义字符数组或字符指针存储或指向字符串，这个稍后再讲。 |
| %x（或 %X 或 %#x 或 %#X） | 以十六进制形式输出整数，这个很重要。                         |



## new 和 delete

```cpp
int *a = new int(9);
int *arr = new int[100];

delete a;
delete[] arr;
```



### 何时需要用new

内存的分配方式有三种    

（1）从静态存储区域分配。内存在程序编译的时候就已经分配好，这块内存在程序的整个运行期间都存在。例如全局变量，static 变量。    

（2）  在栈上创建。在执行函数时，函数内局部变量的存储单元都可以在栈上创建，函数执行结束后在将这些局部变量的内存空间回收。在栈上分配内存空间效率很高，但是分配的内存容量有限。  

（3） 从堆上分配的。程序在运行的时候用 malloc 或 new 申请任意多少的内存，程序员自己负责在何时用 free 或 delete 释放内存。

**不使用new创建对象时**，对象的内存空间是在栈中的，其作用范围只是在函数内部，函数执行完成后就会调用析构函数，删除该对象。

而**使用new创建对象**是创建在堆中的，必须要程序员手动的去管理该对象的内存空间。也就是说如果用new创建对象，就必须手动用delete销毁。

```cpp
Father fa01;		// 生成一个临时对象，函数执行完毕后就自动执行析构函数销毁
Father* fa02 = new Father;		// 需要使用delete手动销毁，否则系统长时间运行，可能会造成内存泄露

delete fa02;
fa02 = nullptr;			// 删除之后，将指针指向null是个好习惯
```







### printf 打印 string类型

```cpp
// c_str()将string类型转换成等效的字符数组
printf("%s\n", s1.c_str());
```



### 使用引用传递来提高效率

https://blog.csdn.net/excpp/article/details/84052336



### lambda表达式

https://blog.csdn.net/weixin_43055404/article/details/103299156




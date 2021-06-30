# C++

## 指针

### 数组指针、指针数组、指针常量

**数组指针**本质上是一个指针，但这个指针指向的是一个数组

**指针数组**本质上是一个数组，但这个数组里存放的都是指针

**常量指针**本质上是一个指针，但这个指针指向一个常量

**指针常量**本质上是一个常量，但这个常量是一个指针

从编译原理角度来看，优先级和右结合让我不再迷茫，[]优先级高于\*,编译器读int \*p[]的时候，先让[]与p进行左结合，然后再与结合，然后再与int结合，也就是说p首先被确定是一个数组，然后该数组中的元素都是int \*类型的。 

而int (\*p)[]，则表明p首先是一个指针了，指向的类型是一个int 

**明确一个优先级顺序：()>[]>\***，所以：

(\*p)[n]：根据优先级，先看括号内，则p是一个指针，这个指针指向一个一维数组，数组长度为n，这是“数组的指针”，即数组指针；

\*p[n]：根据优先级，先看[]，则p是一个数组，再结合*，这个数组的元素是指针类型，共n个元素，这是“指针的数组”，即指针数组。

```cpp
int *p[];       //指针数组，指针型数组，是一个数组，类型为*
int (*p)[];     //数组指针，数组型的指针，是一个指针，指向一个int []数组
```

```cpp
const int p; // p 为常量，初始化后不可更改
 
const int* p; // *p 为常量，不能通过*p改变它指向的内容 
 
int const* p; // *p 为常量，同上 
 
int* const p; // p 为常量，初始化后不能再指向其它内容
```





### 文件读写

```c++
#include  <iostream>
#include <fstream>    // 读写文件的头文件
#include <string>
using namespace std;
/*
 1 文本文件 写文件
     1 包含头文件
            #include <fstream>
     2 创建流对象
            ofstream ofs;
     3 指定路径和打开方式
            ofs.open(路径, 打开方式);
        打开方式：
            ios::in        读文件打开
            ios::out    写文件打开
            ios::ate    从文件尾打开
            ios::app    追加方式打开
            ios::trunc    如果已经有文件 先删除在撞见
            ios::binary    二进制方式
     4 写内容
             ofs << "写点数据" << endl;
     5 关闭文件
            ofs.close();
*/
void write() {
    // 1 包含头文件 #include <fstream>
    // 2 创建流对象
    ofstream ofs;
    // 3 指定路径和打开方式
    ofs.open("text.txt", ios::out);
    // 4 写内容
    ofs << "写点数据" << endl;
    ofs << "写点数据2" << endl;
    ofs << "写点数据3" << endl;

    // 5 关闭文件
    ofs.close();
}

/*
2 文本文件 读文件
     1 包含头文件
            #include <fstream>
     2 创建流对象
            ifstream ifs;
     3 指定路径和打开方式
            ifs.open(路径, 打开方式);
        打开方式：
            ios::in        读文件打开
            ios::out    写文件打开
            ios::ate    从文件尾打开
            ios::app    追加方式打开
            ios::trunc    如果已经有文件 先删除在撞见
            ios::binary    二进制方式
     4 读取 四种方式
            ifs << "写点数据" << endl;
     5 关闭文件
            ifs.close();
*/

void read() {
    // 1 头文件
    // 2 创建流对象
    ifstream ifs;
    // 3 打开文件 判断是否打开成功
    ifs.open("text.txt", ios::in);
    if (!ifs.is_open()) {
        cout << "文件打开失败！" << endl;
        return;
    }
    // 4 读数据 四种方式
    // 第一种方式
    //char buf[1024] = { 0 };
    //while (ifs >> buf) {
    //    cout << buf << endl;
    //}

    // 第二种
    //char buf[1024];
    //while (ifs.getline(buf, sizeof(buf))) {
    //    cout << buf << endl;
    //}

    // 第三种 逐行读取
    //string buf;
    //while (getline(ifs, buf)) {
    //    cout << buf << endl;
    //}

    // 第四种 不推荐用
    char c;
    while ((c=ifs.get()) != EOF) {
        cout << c;
    }


    // 5 关闭流
    ifs.close();
}

int main() {

    read();

    system("pause");
    return 0;
}
```



```cpp
#include<iostream>
#include<sstream>
#include<fstream>
#include<string>
#include<set>
#include<Windows.h>

using namespace std;

//从文件中读取进程相关数据，初始化进程
void init() {
	ifstream ifs;		//创建流对象
	ifs.open("PCB.txt", ios::in);
	if (!ifs.is_open()) {
		cout << "文件打开失败" << endl;
		return;
	}

	string buff;
	int row = 0;		//行数，跳过第0行的中文字段说明
	while (getline(ifs, buff)) {
		if (row) {
			istringstream str(buff);
			string out;
			int column = 0;		//列数
			while (str >> out) {
				switch (column) {
				case 0:
					pcb[row - 1].name = out;
					break;
				case 1:
					pcb[row - 1].runTime = atoi(out.c_str());		//c语言转换形式，string 转 int
					break;
				case 2:
					pcb[row - 1].priority = atoi(out.c_str());
					break;
				case 3:
					pcb[row - 1].status = out;
					break;
				default:
					break;
				}
				column++;
			}
		}
		row++;
	}
}
```



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



### 栈和队列的用法

C++中 栈和队列已经被封装好，我们使用时只需要按照如下步骤调用即可。

```c++
#include<stack>		//包含栈的头文件
#include<queue>		//包含队列的头文件

stack<int> s;		//定义栈
queue<int> q;		//定义队列
```

栈和队列提供了如下的操作：

```c++
s.empty() 	//如果栈为空返回true，否则返回false
s.size()	//返回栈中元素的个数
s.pop() 	//删除栈顶元素但不返回其值
s.top() 	//返回栈顶的元素，但不删除该元素
s.push() 	//在栈顶压入新元素
    
q.empty() 	//如果队列为空返回true，否则返回false
q.size() 	//返回队列中元素的个数
q.pop() 	//删除队列首元素但不返回其值
q.front() 	//返回队首元素的值，但不删除该元素
q.push() 	//在队尾压入新元素
q.back() 	//返回队列尾元素的值，但不删除该元素
```



### list 容器

```c++
push_back(elem);		//在容器尾部加入一个元素
pop_back();				//删除容器中最后一个元素
push_front(elem);		//在容器开头插入一个元素
pop_front();			//在容器开头插入移除一个元素
insert(pos, elem);		//在pos位置插elem元素的拷贝，返回新数据的位置
insert(pos,n,elem);		//在pos位置插入n个elem元素，无返回值
insert(pos, beg, end);	//在pos位置插入[beg, end]区间的数据，无返回值
clear();				//移除容器的所有数据
erase(beg,end);			//删除pos位置的数据，返回下一个数据的位置
erase(pos);				//删除pos位置的数据，返回下一个数据的位置
remove(elem);			//删除容器中所有与elem值匹配的元素
```



### "->" 和 "."的区别

`->`主要用于类类型的指针访问类的成员du，而`.`运算符，主要用于类类型的对象访问类的成员。

1、A.B则A为对象或者结构体；

2、A->B则A为指针，->是成员提取，A->B是提取A中的成员B，A只能是指向类、结构、联合的指针；

3、::是作用域运算符，A::B表示作用域A中的名称B，A可以是名字空间、类、结构；

4、：一般用来表示继承；



### printf 打印 string类型



```cpp
// c_str()将string类型转换成等效的字符数组
	printf("%s\n", s1.c_str());
	printf("%s\n", s2.c_str());
```





### 仿函数

可以联想C#的委托

```cpp
//仿函数，用于自定义数据类型排序
class Compare {
public:
	bool operator() (const PCB p1, const PCB p2) const {
		return (p1.priority > p2.priority)			//按照优先级降序排列，如果优先级相同，则名字大的先运行
			|| (p1.priority == p2.priority && p1.name > p2.name);
	}
};

//打印集合中的所有元素
void printSet(multiset<PCB, Compare> s) {
	for (multiset<PCB, Compare>::iterator it = s.begin(); it != s.end(); it++) {
		cout << toString(it) << endl;
	}
}
```





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



### fill 和 memset

> * memset对于char型数组，可以正常赋值。对于int型数组，只能赋0或-1，因为memset是按照字节来赋值的。
> * memset一维数组和二维数组赋值是一样的
> * **使用fill()对二维数组data[r] [c]进行初始化时，数组起始地址为data[0], 因为二维数组相当于数组的数组，数组名所在的数组里存储的是一串地址，而数据真正存的地址是data[0].**



```c++
#include<stdio.h>
#include <string.h>	// memeset 所需头文件 
#include<algorithm>	// fill 所需头文件 

int main() {
	int arr1[29];
	char arr2[29][29];
	
	// 使用memset 给一维数组赋值
	memeset(arr1, 0, sizeof(arr1)); 
	
	// 使用memset 给二维数组赋值 
	memset(arr2, '.', sizeof(arr2));
	
	// 使用fill给一维数组赋值 
	fill(arr1, arr1+29, 1);
	
	// 使用fill给二维数组赋值 
	fill(arr2[0], arr2[0] + 29*29, '$');
	
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






## C++ 学习笔记



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





### 字符串分割，以空格为界

istringstream 是将字符串变成字符串迭代器一样，将字符串流依次拿出，比较好的是，它不会将空格作为流。这样就实现了字符串的空格切割。

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




## C++ 数组填充：fill() 与 memset() 比较

### 一. 结论

> **C++对数组进行初始化时，尽量使用fill()函数，不易出错**

### 二. 区别

#### 2.1 头文件

> fill(): # 属于C++的STL函数
>
> memset(): # 属于C语言函数

#### 2.2 填充效果

> fill(): 可以对数组赋任意值
>
> memset(): 对于char型数组，可以正常赋值。对于int型数组，只能赋0或-1
>
> PS：memset() 无法对int型数组正常赋值的原因是memset()是按照字节来赋值的。

#### 2.3 参数

> fill() 函数参数：fill(first,last,val);
> // first 为容器的首迭代器，last为容器的末迭代器，val为将要替换的值
>
> memset() 函数参数：void *memset(void *s,int c,size_t n)

### 三. 注意事项：

> **使用fill()对二维数组data[r] [c]进行初始化时，数组起始地址为data[0], 因为二维数组相当于数组的数组，数组名所在的数组里存储的是一串地址，而数据真正存的地址是data[0].**

### 三. 代码测试

```
//  main.cpp
//  pat1003

//  Created by 赵泉斌 on 2019/9/10.
//  Copyright © 2019 zqb. All rights reserved.

#include <cstring>
#include <algorithm>
#define maxn 5

using namespace std;

int data[maxn];
int G[maxn][maxn];

int (int argc, const char * argv[]) {
    fill(G[0], G[0] + maxn * maxn, 0115);
    cout << "使用fill（）填充二维数组任意值效果为：" << endl;
    for(int i = 0; i < maxn; i++){
        for(int j = 0; j < maxn; j++){
            cout << G[i][j] << " ";
        }
        cout << endl;
    }
    cout << "使用memset（）填充 0 效果为：" << endl;
    memset(data, 0, sizeof(data));
    for(int i = 0; i < maxn; i++){
        cout << data[i] << " ";
    }
    cout << endl;
    cout << "使用memset（）填充 -1 效果为：" << endl;
    memset(data, -1, sizeof(data));
    for(int i = 0; i < maxn; i++){
        cout << data[i] << " ";
    }
    cout << endl;
    cout << "使用memset（）填充10效果为：" << endl;
    memset(data, 10, sizeof(data));
    for(int i = 0; i < maxn; i++){
        cout << data[i] << " ";
    }
    cout << endl;
    return 0;
}
```

结果为：

```
使用fill（）填充二维数组任意值效果为：
115 115 115 115 115 
115 115 115 115 115 
115 115 115 115 115 
115 115 115 115 115 
115 115 115 115 115 
使用memset（）填充 0 效果为：
0 0 0 0 0 
使用memset（）填充 -1 效果为：
-1 -1 -1 -1 -1 
使用memset（）填充10效果为：
168430090 168430090 168430090 168430090 168430090 
Program ended with exit code: 0
```
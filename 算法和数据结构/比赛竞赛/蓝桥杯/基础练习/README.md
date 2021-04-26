# 基础练习

### BASIC-1 闰年判断

> **问题描述**
>
> 给定一个年份，判断这一年是不是闰年。
>
> 当以下情况之一满足时，这一年是闰年：
>
> 1. 年份是4的倍数而不是100的倍数；
>
> 2. 年份是400的倍数。
>
> 其他的年份都不是闰年。
>
> **输入格式**
>
> 输入包含一个整数y，表示当前的年份。
>
> **输出格式**
>
> 输出一行，如果给定的年份是闰年，则输出yes，否则输出no。

| 样例输入 | 样例输出 |
| -------- | -------- |
| 2013     | no       |
| 2016     | yes      |

```c
// 数据规模与约定
1990 <= y <= 2050
//提示
当试题指定你输出一个字符串作为结果（比如本题的yes或者no，你需要严格按照试题中给定的大小写，写错大小写将不得分。
```

**参考源代码**

```c++
#include<iostream>

using namespace std;

int main() {
	int y;
	cin >> y;
	
	if((y%4==0 && y%100 != 0) || y % 400 == 0)  {
		cout << "yes" << endl;
	} else {
		cout << "no" << endl;
	}
	
	return 0;
} 
```



### BASIC-2 01字串

> **问题描述**
>
> 对于长度为5位的一个01串，每一位都可能是0或1，一共有32种可能。它们的前几个是：
>
> 00000
>
> 00001
>
> 00010
>
> 00011
>
> 00100
>
> **输入格式**
>
> 本试题没有输入。
>
> **输出格式**
>
> 输出32行，按从小到大的顺序每行一个长度为5的01串。

| 样例输入 | 样例输出 |
| -------- | -------- |
|          |          |
|          |          |

```c
// 数据规模与约定

//提示

```

**参考源代码**

```c++
#include<iostream>

using namespace std;

int main() {
	for (int i = 0; i < 32; i++) {
		for(int j = 4; j >= 0; j--) {
			// 从最高位开始，将每一位都移到最后一位
			// 与00001相与， 即可判断此位是0还是1 
			cout << ((i >> j) & 1);
		}
		cout << endl;
	}
		
	return 0;
} 
```





### BASIC-3 字母图形

> **问题描述**
>
> 利用字母可以组成一些美丽的图形，下面给出了一个例子：
>
> ABCDEFG
>
> BABCDEF
>
> CBABCDE
>
> DCBABCD
>
> EDCBABC
>
> 这是一个5行7列的图形，请找出这个图形的规律，并输出一个n行m列的图形。
>
> **输入格式**
>
> 输入一行，包含两个整数n和m，分别表示你要输出的图形的行数的列数。
>
> **输出格式**
>
> 输出n行，每个m个字符，为你的图形。

| 样例输入 | 样例输出                                                |
| -------- | ------------------------------------------------------- |
| 5 7      | ABCDEFG<br/>BABCDEF<br/>CBABCDE<br/>DCBABCD<br/>EDCBABC |
|          |                                                         |

```c
// 数据规模与约定
1 <= n, m <= 26
//提示

```

**参考源代码**

```c++
#include <iostream>

using namespace std;

int main(){
	int m, n;
	
	cin >> n >> m;
	
	for(int i = 0; i < n; i++) {
		for(int j = i; j >= 0 && j > i-m; j--) {
			cout << (char)('A' + j);
		}
		
		for(int k = 1; k < m-i; k++) {
			cout << (char)('A' + k);
		}
		cout << endl;
	}
	
	
	return 0;
}

```





### BASIC-4 数列特征

> **问题描述**
>
> 给出n个数，找出这n个数的最大值，最小值，和。
>
> **输入格式**
>
> 第一行为整数n，表示数的个数。
>
> 第二行有n个数，为给定的n个数，每个数的绝对值都小于10000。
>
> **输出格式**
>
> 输出三行，每行一个整数。第一行表示这些数中的最大值，第二行表示这些数中的最小值，第三行表示这些数的和。

| 样例输入          | 样例输出        |
| ----------------- | --------------- |
| 5<br />1 3 -2 4 5 | 5<br/>-2<br/>11 |
|                   |                 |

```c
// 数据规模与约定
1 <= n <= 10000
//提示

```

**参考源代码**

```c++
#include<iostream>
#define INF 1000001

using namespace std;

int main() {
	int n, temp;
	int sum = 0;
	int max = -INF;
	int min = INF;
	
	cin >> n;
	
	while(n--) {
		cin >> temp;
		sum += temp;
		
		if(temp > max) max = temp;
		if(temp < min) min = temp;
	}
	cout << max << endl;
	cout << min << endl;
	cout << sum << endl;
	
	return 0;
}

// 另一种思路，使用 STL容器
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

int main() {
	int n, m;
	int sum = 0;
	vector<int> myVec;
	
	cin >> n;
	
	for(int i = 0; i < n; i++) {
		cin >> m;
		myVec.push_back(m);
	}

	sort(myVec.begin(), myVec.end());
	
	for(vector<int>::iterator it = myVec.begin(); it != myVec.end(); it++) {
		sum += *it;
		//cout << *it;
	}
	cout << myVec.back() << endl;
	cout << myVec.front() << endl;
	cout << sum << endl;
	
	return 0;
}
```



### BASIC-5 查找整数

> **问题描述**
>
> 给出一个包含n个整数的数列，问整数a在数列中的第一次出现是第几个。
>
> **输入格式**
>
> 第一行包含一个整数n。
>
> 第二行包含n个非负整数，为给定的数列，数列中的每个数都不大于10000。
>
> 第三行包含一个整数a，为待查找的数。
>
> **输出格式**
>
> 如果a在数列中出现了，输出它第一次出现的位置(位置从1开始编号)，否则输出-1。

| 样例输入                | 样例输出 |
| ----------------------- | -------- |
| 6<br/>1 9 4 8 3 9<br/>9 | 2        |
|                         |          |

```c
// 数据规模与约定
1 <= n <= 1000。
//提示

```

**参考源代码**

```c++
#include<iostream>
#include<vector>

using namespace std;

int main() {
	int n, a, temp;
	vector<int> myVec;
	vector<int>::iterator it;
	
	cin >> n;
	
	while(n--) {
		cin >> temp;
		myVec.push_back(temp);
	}
	
	cin >> a;
	
	for( it = myVec.begin(); it != myVec.end(); it++) {
		if(*it == a) {
			cout << it - myVec.begin() + 1;
			break;
		}
		
	} 
	if(it == myVec.end()) {
		cout << -1;
	}
	
	return 0;
}
```



### BASIC-6 杨辉三角形

> **问题描述**
>
> 杨辉三角形又称Pascal三角形，它的第i+1行是(a+b)i的展开式的系数。
>
> 它的一个重要性质是：三角形中的每个数字等于它两肩上的数字相加。
>
> 下面给出了杨辉三角形的前4行：　
>
>   1
>
>  1 1
>
>  1 2 1
>
> 1 3 3 1　
>
> 给出n，输出它的前n行。
>
> **输入格式**
>
> 输入包含一个数n。
>
> **输出格式**
>
> 输出杨辉三角形的前n行。每一行从这一行的第一个数开始依次输出，中间使用一个空格分隔。请不要在前面输出多余的空格。

| 样例输入 | 样例输出                        |
| -------- | ------------------------------- |
| 4        | 1<br/>1 1<br/>1 2 1<br/>1 3 3 1 |
|          |                                 |

```c
// 数据规模与约定
1 <= n <= 34
    
//提示

```

**参考源代码**

```c++
#include<iostream>

using namespace std;

int main() {
	int n;
	cin >> n;

	int arr[34][34];

	for (int i = 0; i < n; i++) {
		for (int j = 0; j <= i; j++) {
			if (j == 0 || j == i) {
				arr[i][j] = 1;
				cout << arr[i][j] << " ";
			}
			else {
				arr[i][j] = arr[i - 1][j] + arr[i-1][j - 1];
				cout << arr[i][j] << " ";
			}
		}
		cout << endl;
	}

	return 0;
}
```



### BASIC-7 特殊的数字

> **问题描述**
>
> 153是一个非常特殊的数，它等于它的每位数字的立方和，即153=1*1*1+5*5*5+3*3*3。编程求所有满足这种条件的三位十进制数。
>
> **输入格式**
>
> 无
>
> **输出格式**
>
> 按从小到大的顺序输出满足条件的三位十进制数，每个数占一行。

| 样例输入 | 样例输出 |
| -------- | -------- |
|          |          |
|          |          |

```c
// 数据规模与约定

//提示

```

**参考源代码**

```c++
#include<iostream>

using namespace std;

int main() {
	for(int i = 100; i < 1000; i++) {
		int c = i % 10;
		int b = i / 10 % 10;
		int a = i / 100;
		
		if((a*a*a + b*b*b + c*c*c) == i) {
			cout << i << endl;
		}
	}
	
	return 0;
}
```



### BASIC-8 回文数

> **问题描述**
>
> 1221是一个非常特殊的数，它从左边读和从右边读是一样的，编程求所有这样的四位十进制数。
>
> **输入格式**
>
> 
>
> **输出格式**
>
> 按从小到大的顺序输出满足条件的四位十进制数。

| 样例输入 | 样例输出 |
| -------- | -------- |
|          |          |
|          |          |

```c
// 数据规模与约定

//提示

```

**参考源代码**

```c++
#include<iostream>

using namespace std;

int main() {
	for(int i = 1; i < 10; i++) {
		for(int j = 0; j < 10; j++) {
			cout << i << j << j << i << endl;
		}
	}
	
	return 0;
}
```





### BASIC-9 特殊回文数

> **问题描述**
>
> 123321是一个非常特殊的数，它从左边读和从右边读是一样的。
> 　　输入一个正整数n， 编程求所有这样的五位和六位十进制数，满足各位数字之和等于n 。
>
> **输入格式**
>
> 输入一行，包含一个正整数n。
>
> **输出格式**
>
> 按从小到大的顺序输出满足条件的整数，每个整数占一行。

| 样例输入 | 样例输出                     |
| -------- | ---------------------------- |
| 52       | 899998<br/>989989<br/>998899 |
|          |                              |

```c
// 数据规模与约定
1<=n<=54
//提示

```

**参考源代码**

```c++
#include<iostream>

using namespace std;

int main() {
	int n;
	cin >> n;
	
	for(int i = 1; i < 10; i++) {
		for(int j = 0; j < 10; j++) {
			for (int k = 0; k < 10; k++) {
				if(i + j + k + j + i == n) {
					cout << i << j << k << j << i << endl;
				}
			}
		}
	}
	
	for(int i = 1; i < 10; i++) {
		for(int j = 0; j < 10; j++) {
			for (int k = 0; k < 10; k++) {
				if(i + j + k + k + j + i == n) {
					cout << i << j << k << k << j << i << endl;
				}
			}
		}
	}
	
	return 0;
}
```





### BASIC-10 十进制转十六进制

> **问题描述**
>
> 十六进制数是在程序设计时经常要使用到的一种整数的表示方式。它有0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F共16个符号，分别表示十进制数的0至15。十六进制的计数方法是满16进1，所以十进制数16在十六进制中是10，而十进制的17在十六进制中是11，以此类推，十进制的30在十六进制中是1E。
> 　　给出一个非负整数，将它表示成十六进制的形式。
>
> **输入格式**
>
> 输入包含一个非负整数a，表示要转换的数。0<=a<=2147483647
>
> **输出格式**
>
> 输出这个整数的16进制表示

| 样例输入 | 样例输出 |
| -------- | -------- |
| 30       | 1E       |
|          |          |

```c
// 数据规模与约定

//提示

```

**参考源代码**

```c++
#include<iostream>
#include<list>
using namespace std;

int main() {
	int a;
	list<int> myList;
	cin >> a;
	
	while(a/16) {
		myList.push_front(a % 16);
		a /= 16;
	}
	
	myList.push_front(a);
	
	for(list<int>::iterator it = myList.begin(); it != myList.end(); it++) {
		if(*it <10) {
			cout << (char)(*it + 48);
		} else {
			cout << (char)(*it + 55);
		}
	}
	return 0;
} 
```



### BASIC-11 十六进制转十进制

> **问题描述**
>
> 从键盘输入一个不超过8位的正的十六进制数字符串，将它转换为正的十进制数后输出。
> 　　注：十六进制数中的10~15分别用大写的英文字母A、B、C、D、E、F表示。
>
> **输入格式**
>
> 
>
> **输出格式**
>
> 

| 样例输入 | 样例输出 |
| -------- | -------- |
| FFFF     | 65535    |
|          |          |

```c
// 数据规模与约定

//提示

```

**参考源代码**

```c++
#include<iostream>
#include<math.h>
#include<string>

using namespace std;

int main() {
	string str;
	unsigned long int result = 0;
	cin >> str;
	
	int len = str.length();
	for(int i = 0; i < str.length(); i++) {
		if(str[i] > '9') {
			result += (str[i] - 55) * pow(16, len-1);
		} else {
			result += (str[i] - 48) * pow(16, len-1);
		}
		len--; 
	}
	cout << result << endl; 
	
	return 0;
}
```





### BASIC-12 十六进制转八进制

> **问题描述**
>
> 给定n个十六进制正整数，输出它们对应的八进制数。
>
> **输入格式**
>
> 输入的第一行为一个正整数n （1<=n<=10）。
> 　　接下来n行，每行一个由0~9、大写字母A~F组成的字符串，表示要转换的十六进制正整数，每个十六进制数长度不超过100000。
>
> **输出格式**
>
> 输出n行，每行为输入对应的八进制正整数。
>
> **注意**
> 　　输入的十六进制数不会有前导0，比如012A。
> 　　输出的八进制数也不能有前导0。

| 样例输入                    | 样例输出           |
| --------------------------- | ------------------ |
| 2<br/>　　39<br/>　　123ABC | 71<br/>　　4435274 |
|                             |                    |

```c
// 数据规模与约定

//提示
先将十六进制数转换成某进制数，再由某进制数转换成八进制。
```

**参考源代码**

```c++
#include<iostream>
#include<string>
#include<math.h>
#include<map>
#include<list>

using namespace std;

void hexToOct(string str) {
	list<int> binList;
	
	for(int i = 0; i < str.length(); i++) {
		int temp = 0;
		// 将单个字符转成2进制 
		str[i] > '9' ? temp = str[i] - 55 : temp = str[i] - 48;
		for(int j = 3; j >=0; j--) {
			binList.push_back((temp >> j) & 1);
		} 
	}
	
	// 二进制转八进制可以按照三个二进制划分
	// 所以不是三的倍数需要最高位补0 
	if(binList.size() % 3 != 0) {
		int temp = 3 - binList.size() % 3;
		while(temp--) {
			binList.push_front(0);
		}	
	}
	
	// 如果前3位为0，则去掉 
	list<int>::iterator it = binList.begin();
	if(*it + *(++it) + *(++it) == 0) {
		binList.pop_front();
		binList.pop_front();
		binList.pop_front();
	}
	
	int flag = 0;
	string octStr;
	for(list<int>::iterator it = binList.begin(); it != binList.end(); it++) {
		// 3位一组，转换成8进制并输出 
		octStr[flag++] = *it;
		if(flag > 2) {
			cout << octStr[0]*2*2 + octStr[1]*2 + octStr[2];
			flag = 0;
		}
	} 

}

int main() {
	int n;
	string str;
	list<string> myList;
	cin >> n;
	
	while(n--) {
		cin >> str;
		myList.push_back(str);
	}
	
	for(list<string>::iterator it = myList.begin(); it != myList.end(); it++) {
		hexToOct(*it);
		cout << endl;
	}
		
	return 0;
}
```





### BASIC-13 数列排序

> **问题描述**
>
> 给定一个长度为n的数列，将这个数列按从小到大的顺序排列。1<=n<=200
>
> **输入格式**
>
> 第一行为一个整数n。
> 　　第二行包含n个整数，为待排序的数，每个整数的绝对值小于10000。
>
> **输出格式**
>
> 输出一行，按从小到大的顺序输出排序后的数列。

| 样例输入        | 样例输出  |
| --------------- | --------- |
| 5<br/>8 3 6 4 9 | 3 4 6 8 9 |
|                 |           |

```c
// 数据规模与约定

//提示

```

**参考源代码**

使用vector容器和算法模板

```c++
#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;

int main() {
	int n, a;
	vector<int> myVec;
	cin >> n;
	
	while(n--) {
		cin >> a;
		myVec.push_back(a);
	}
	
	sort(myVec.begin(), myVec.end());
	
	for(vector<int>::iterator it = myVec.begin(); it != myVec.end(); it++) {
		cout << *it << " ";
	}
	return 0;
}
```

使用map容器

```c++
#include<iostream>
#include<algorithm>
#include<map>
using namespace std;

int main() {
	int n, a;
	multimap<int, int> myMap;
	
	cin >> n;
	
	while(n--) {
		cin >> a;
		// map/multimap 默认以key的升序排列
        // 将用户输入的值作为key,可以利用此特性排序
		myMap.insert(pair<int, int>(a, n));
	}
	
	for(multimap<int, int>::iterator it = myMap.begin(); it != myMap.end(); it++) {
		cout << it->first << " ";
	}
	
	return 0;
} 
```

动态数组和冒泡排序

```c++
#include<iostream>
#include<stdlib.h>
using namespace std;

int main() {
	int n, a;
	cin >> n;
	
	int *arr = (int *)malloc(sizeof(int) * n);
	
	for(int i = 0; i < n; i++) {
		cin >> a;
		arr[i] = a;
	}
	
	for(int i = 0; i < n; i++) {
		for(int j = n - 1; j > i; j--) {			
			if(arr[j-1] > arr[j]) {
				int temp = arr[j-1];
				arr[j-1] = arr[j];
				arr[j] = temp;
				isSorted = false;
			}
		}
		cout << arr[i] << " ";
	}
	
	return 0;
} 
```


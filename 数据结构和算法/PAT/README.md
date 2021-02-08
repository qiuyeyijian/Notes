# PAT

## 1001 害死人不偿命的(3n+1)猜想

卡拉兹(Callatz)猜想：

对任何一个正整数 *n*，如果它是偶数，那么把它砍掉一半；如果它是奇数，那么把 (3*n*+1) 砍掉一半。这样一直反复砍下去，最后一定在某一步得到 *n*=1。卡拉兹在 1950 年的世界数学家大会上公布了这个猜想，传说当时耶鲁大学师生齐动员，拼命想证明这个貌似很傻很天真的命题，结果闹得学生们无心学业，一心只证 (3*n*+1)，以至于有人说这是一个阴谋，卡拉兹是在蓄意延缓美国数学界教学与科研的进展……

我们今天的题目不是证明卡拉兹猜想，而是对给定的任一不超过 1000 的正整数 *n*，简单地数一下，需要多少步（砍几下）才能得到 *n*=1？

### 输入格式：

每个测试输入包含 1 个测试用例，即给出正整数 *n* 的值。

### 输出格式：

输出从 *n* 计算到 1 需要的步数。

### 输入样例：

```in
3
```

### 输出样例：

```out
5
```

### 题目分析

没什么难度，根据题意编程即可



### 参考代码

```cpp
#include <stdio.h>

int main() {
  int n;        // 待输入的正整数n
  int ans = 0;  // 要输出的答案

  scanf("%d", &n);

  while (n != 1) {  // 只要不等于1就一直循环计算
    if (n % 2) {
      n = (3 * n + 1) / 2;
    } else {
      n /= 2;
    }
    ans++;  // 记录步数
  }

  printf("%d", ans);

  return 0;
}
```



## 1002 写出这个数

读入一个正整数 *n*，计算其各位数字之和，用汉语拼音写出和的每一位数字。

### 输入格式：

每个测试输入包含 1 个测试用例，即给出自然数 *n* 的值。这里保证 *n* 小于 $100^{100}$。

### 输出格式：

在一行内输出 *n* 的各位数字之和的每一位，拼音数字间有 1 空格，但一行中最后一个拼音数字后没有空格。

### 输入样例：

```in
1234567890987654321123456789
```

### 输出样例：

```out
yi san wu
```

### 题目分析

首先n是一个大整数，所以需要用字符串来存储，然后计算各位数字之和sum。

将sum转成字符数组，可以实现高位先输出，这里使用`sprintf`函数。也可以利用栈来实现。

### 参考代码

```cpp
#include <string.h>

#include <iostream>
#include <string>

using namespace std;

string n = "";  // 输入的自然数
int sum = 0;    // n的各位数字之和
char ch[10];    // 临时字符数组
string strArr[10] = {"ling", "yi",  "er", "san", "si",
                     "wu",   "liu", "qi", "ba",  "jiu"};

int main() {
  cin >> n;

  // 求n各位数字之和
  for (int i = 0; i < n.length(); i++) {
    sum += n[i] - '0';
  }

  // 将sum转换成拼音输出
  sprintf(ch, "%d", sum);  // 将整型转换成字符数组
  for (int i = 0; i < strlen(ch); i++) {
    if (i < strlen(ch) - 1) {
      cout << strArr[ch[i] - '0'] << " ";
    } else {
      cout << strArr[ch[i] - '0'];
    }
  }

  return 0;
}
```



## 1003 我要通过！

“**答案正确**”是自动判题系统给出的最令人欢喜的回复。本题属于 PAT 的“**答案正确**”大派送 —— 只要读入的字符串满足下列条件，系统就输出“**答案正确**”，否则输出“**答案错误**”。

得到“**答案正确**”的条件是：

1. 字符串中必须仅有 `P`、 `A`、 `T`这三种字符，不可以包含其它字符；
2. 任意形如 `xPATx` 的字符串都可以获得“**答案正确**”，其中 `x` 或者是空字符串，或者是仅由字母 `A` 组成的字符串；
3. 如果 `aPbTc` 是正确的，那么 `aPbATca` 也是正确的，其中 `a`、 `b`、 `c` 均或者是空字符串，或者是仅由字母 `A` 组成的字符串。

现在就请你为 PAT 写一个自动裁判程序，判定哪些字符串是可以获得“**答案正确**”的。

### 输入格式：

每个测试输入包含 1 个测试用例。第 1 行给出一个正整数 *n* (<10)，是需要检测的字符串个数。接下来每个字符串占一行，字符串长度不超过 100，且不包含空格。

### 输出格式：

每个字符串的检测结果占一行，如果该字符串可以获得“**答案正确**”，则输出 `YES`，否则输出 `NO`。

### 输入样例：

```in
8
PAT
PAAT
AAPATAA
AAPAATAAAA
xPATx
PT
Whatever
APAAATAA
```

### 输出样例：

```out
YES
YES
YES
YES
NO
NO
NO
NO
```

### 题目分析

用户输入的字符串大概可以归纳一个前提两种情况：

**前提**

字符串不能包含`P` `A` `T`以外的字符

**情况1：**

 中间是字符串`PAT` ，两边要么没有A，要么A的个数相同，即形如：`PAT`、`APATA`、`AAPATAA` ......

**情况2：**

若`aPbTc`成立，则`aPbATca`成立，`P`和`T`之间至少有一个`A`

  由于PAT成立，故PAAT成立，故PAAAT成立，....，故PA...T成立

  由于APATA成立，故APAATAA成立，故APAAATAAA成立，...，故AP[n个A]T[n个A]成立

  由于AAPATAA成立，故AAPAATAAAA成立，故AAPAAATAAAAAA成立，...，故AAP[n个A]T[n * 2个A]成立

所以形如[n个A]P[m个A]T[n * m个A]是成立的，其中 m > 0。



**备注**

我们只需要根据原字符串中`P`和`T`的位置，构造一个满足要求的新的字符串，看看是否和原字符串相等，即可判断是否符合要求

### 参考代码

```cpp
#include <iostream>
#include <string>

using namespace std;

// 生成全是A的字符串，长度为n
string getStrA(int n) {
  string str = "";
  while (n--) {
    str += "A";
  }
  return str;
}

bool judge(string str) {
  int len = str.length();
  int pos1 = str.find("P");  // P首次出现的位置
  int pos2 = str.find("T");  // T首次出现位置
  // 前提：不能包含除P A T以外的其他字符
  for (int i = 0; i < len; i++) {
    if (!(str[i] == 'P' || str[i] == 'A' || str[i] == 'T')) {
      return false;
    }
  }

  // 情况1：中间是字符串PAT ，两边要么没有A，要么A的个数相同
  if (pos1 >= 0 && str == getStrA(pos1) + "PAT" + getStrA(pos1)) {
    return true;
  }

  // 情况2：形如[n个A]P[m个A]T[n * m个A]是成立的, 其中m > 0
  if ((pos2 - pos1 - 1) > 0 && str == getStrA(pos1) + "P" +
                                          getStrA(pos2 - pos1 - 1) + "T" +
                                          getStrA(pos1 * (pos2 - pos1 - 1))) {
    return true;
  }

  return false;  // 如果都不满足，返回false
}

int main() {
  int n;
  string str;
  cin >> n;
  while (n--) {
    cin >> str;
    if (judge(str)) {
      printf("YES\n");
    } else {
      printf("NO\n");
    }
  }

  return 0;
}

```



## 1004 成绩排名

读入 *n*（>0）名学生的姓名、学号、成绩，分别输出成绩最高和成绩最低学生的姓名和学号。

### 输入格式：

每个测试输入包含 1 个测试用例，格式为

```
第 1 行：正整数 n
第 2 行：第 1 个学生的姓名 学号 成绩
第 3 行：第 2 个学生的姓名 学号 成绩
  ... ... ...
第 n+1 行：第 n 个学生的姓名 学号 成绩
```

其中`姓名`和`学号`均为不超过 10 个字符的字符串，成绩为 0 到 100 之间的一个整数，这里保证在一组测试用例中没有两个学生的成绩是相同的。

### 输出格式：

对每个测试用例输出 2 行，第 1 行是成绩最高学生的姓名和学号，第 2 行是成绩最低学生的姓名和学号，字符串间有 1 空格。

### 输入样例：

```in
3
Joe Math990112 89
Mike CS991301 100
Mary EE990830 95
```

### 输出样例：

```out
Mike CS991301
Joe Math990112
```

### 题目分析

一个学生有许多属性，所以最好定义一个结构体。

本题只要求找最大和最小，并没有要求排序。所以在读入数据的时候就可以进行相关判断，一个循环就搞定。

### 参考代码

```cpp
#include <stdio.h>
#include <string.h>

struct Student {        // 定义学生结构体
  char name[12];
  char id[12];
  int score;
} maxS, minS, tempS;

int main() {
  int n;
  scanf("%d", &n);
  maxS.score = 0;       // 初始化
  minS.score = 1000;

  while (n--) {
    scanf("%s %s %d", tempS.name, tempS.id, &tempS.score);
    if (tempS.score > maxS.score) {
      strcpy(maxS.name, tempS.name);
      strcpy(maxS.id, tempS.id);
      maxS.score = tempS.score;
    }
    if (tempS.score < minS.score) {
      strcpy(minS.name, tempS.name);
      strcpy(minS.id, tempS.id);
      minS.score = tempS.score;
    }
  }
  
  printf("%s %s\n", maxS.name, maxS.id);
  printf("%s %s", minS.name, minS.id);

  return 0;
}
```



## 1005 继续(3n+1)猜想 

卡拉兹(Callatz)猜想已经在1001中给出了描述。在这个题目里，情况稍微有些复杂。

当我们验证卡拉兹猜想的时候，为了避免重复计算，可以记录下递推过程中遇到的每一个数。例如对 *n*=3 进行验证的时候，我们需要计算 3、5、8、4、2、1，则当我们对 *n*=5、8、4、2 进行验证的时候，就可以直接判定卡拉兹猜想的真伪，而不需要重复计算，因为这 4 个数已经在验证3的时候遇到过了，我们称 5、8、4、2 是被 3“覆盖”的数。我们称一个数列中的某个数 *n* 为“关键数”，如果 *n* 不能被数列中的其他数字所覆盖。

现在给定一系列待验证的数字，我们只需要验证其中的几个关键数，就可以不必再重复验证余下的数字。你的任务就是找出这些关键数字，并按从大到小的顺序输出它们。

### 输入格式：

每个测试输入包含 1 个测试用例，第 1 行给出一个正整数 *K* (<100)，第 2 行给出 *K* 个互不相同的待验证的正整数 *n* (1<*n*≤100)的值，数字间用空格隔开。

### 输出格式：

每个测试用例的输出占一行，按从大到小的顺序输出关键数字。数字间用 1 个空格隔开，但一行中最后一个数字后没有空格。

### 输入样例：

```in
6
3 5 6 7 8 11
```

### 输出样例：

```out
7 6
```

### 题目分析

因为正整数的个数小于100，且正整数n(1<*n*≤100)。所以：

1. 利用数组下标来记录用户输入的正整数，并将下标所对应的值设为1
2. 进行卡拉兹运算，每进行一步，就将数组对应位置的值设为0，
3. 最后，数组中值为1的下标就是没有被数列中的其他数字所覆盖的"关键数"

### 参考代码

```cpp
#include <algorithm>
#include <iostream>

using namespace std;

int main() {
  int n;
  int flag = 1;
  int a[110] = {0};

  scanf("%d", &n);

  // 读入待验证的数字，将数组对应位置的值设为1
  for (int i = 0; i < n; i++) {
    int t;
    scanf("%d", &t);
    a[t] = 1;
  }

  // 进行卡拉兹运算，每进行一步，就将数组对应位置的值设为0
  for (int i = 2; i < 101; i++) {
    if (a[i]) {
      int t = i;
      while (t != 1) {
        if (t % 2) {
          t = (3 * t + 1) / 2;
        } else {
          t /= 2;
        }
        if (t <= 100) {		// 用户输入的是100以内的数，所以我们只考虑100以内的数字
          a[t] = 0;
        }
      }
    }
  }

  // 最后，数组中值为1的下标就是没有被数列中的其他数字所覆盖的”关键数“
  for (int i = 100; i > 1; i--) {
    if (a[i]) {
      if (flag) {
        printf("%d", i);
        flag = 0;
      } else {
        printf(" %d", i);
      }
    }
  }

  return 0;
}
```



## 1006 换个格式输出整数

让我们用字母 `B` 来表示“百”、字母 `S` 表示“十”，用 `12...n` 来表示不为零的个位数字 `n`（<10），换个格式来输出任一个不超过 3 位的正整数。例如 `234` 应该被输出为 `BBSSS1234`，因为它有 2 个“百”、3 个“十”、以及个位的 4。

### 输入格式：

每个测试输入包含 1 个测试用例，给出正整数 *n*（<1000）。

### 输出格式：

每个测试用例的输出占一行，用规定的格式输出 *n*。

### 输入样例 1：

```in
234
```

### 输出样例 1：

```out
BBSSS1234
```

### 输入样例 2：

```in
23
```

### 输出样例 2：

```out
SS123
```

### 题目分析

因为规定正整数n<1000，所以将n分离出百位，十位和个位的值，然后根据格式循环打印即可。

### 参考代码

```cpp
#include <stdio.h>

int main() {
    int n;
    scanf("%d", &n);

    int b = n / 100 % 10;
    int s = n / 10 % 10;
    int a = n % 10;

    for (int i = 0; i < b; i++) {
        printf("%c", 'B');
    }
    for (int i = 0; i < s; i++) {
        printf("%c", 'S');
    }
    for (int i = 0; i < a; i++) {
        printf("%d", i + 1);
    }
    printf("\n");

    return 0;
}
```



## 1007 素数对猜想

让我们定义：
$$
d_n = p_{n+1} - p_n
$$
其中 $p_i$ 是第*i*个素数。显然有 $d_1$ = 1，且对于*n*>1有 $d_n$ 是偶数。“素数对猜想”认为“存在无穷多对相邻且差为2的素数”。

现给定任意正整数`N`( < $10^5$ )，请计算不超过`N`的满足猜想的素数对的个数。

### 输入格式:

输入在一行给出正整数`N`。

### 输出格式:

在一行中输出不超过`N`的满足猜想的素数对的个数。

### 输入样例:

```in
20
```

### 输出样例:

```out
4
```

### 题目分析

本题主要考察如何判断素数。参考代码判断素数时间复杂度为O(sqrt(n))，较为可接受。

素数猜想的判断要从第二个素数a开始，判断a和a+2是否为素数，如果是计数器加1; 每轮循环a都自增2，因为自增1明显不可能是素数，可以减少计算次数。

### 参考代码

```cpp
#include <math.h>
#include <stdio.h>

bool isPrime(int n) {  // 判断一个数是否为素数
  if (n <= 1) return false;
  int sqr = (int)sqrt(n * 1.0);
  for (int i = 2; i <= sqr; i++) {
    if (n % i == 0) return false;
  }
  return true;
}

int main() {
  int n;
  int ans = 0;

  scanf("%d", &n);
  for (int i = 3; i + 2 <= n; i += 2) {  // 从第二个素数开始判断，每次自增2
    if (isPrime(i) && isPrime(i + 2)) ans++;
  }
  printf("%d", ans);
  return 0;
}
```



## 1008 数组元素循环右移问题 

一个数组*A*中存有*N*（> 0）个整数，在不允许使用另外数组的前提下，将每个整数循环向右移M（≥0）个位置，即将*A*中的数据由$(A_0A_1......A_{N-1})$变换为$(A_{N-M}...A_{N-1}A_0A_1...A_{N-M-1})$ （ 最后*M*个数循环移至最前面的*M*个位置）。如果需要考虑程序移动数据的次数尽量少，要如何设计移动的方法？

### 输入格式:

每个输入包含一个测试用例，第1行输入*N*（1≤*N*≤100）和*M*（≥0）；第2行输入*N*个整数，之间用空格分隔。

### 输出格式:

在一行中输出循环右移*M*位以后的整数序列，之间用空格分隔，序列结尾不能有多余空格。

### 输入样例:

```in
6 2
1 2 3 4 5 6
```

### 输出样例:

```out
5 6 1 2 3 4
```

### 题目分析

C++的`algorithm`头文件有数组反转函数`reverse()`，参考代码中为了训练，重新实现了这个功能，其实调用`reverse()`更加高效、简单。

参考本目录下Notes笔记的相关章节：`数组元素循环右移问题及解决方法`

或者参考链接：https://www.jb51.net/article/181740.htm

### 参考代码

```cpp
#include <iostream>

using namespace std;

// 开始是首元素的地址，结束是尾元素地址的下一个地址
void reverseArr(int* start, int* end) {
  int n = (end - start) / 2;
  for (int i = 0; i < n; i++) {
    int t = *(start + i);
    *(start + i) = *(end - i - 1);
    *(end - i - 1) = t;
  }
}

int main() {
  int n, m;
  int arr[110];
  scanf("%d %d", &n, &m);

  m %= n;  // m的值可能大于数组长度n, 所以需要取模
  for (int i = 0; i < n; i++) {
    scanf("%d", &arr[i]);
  }

  reverseArr(arr, arr + n);  // 先将整个数组[0，N)逆置一遍；
  reverseArr(arr, arr + m);  // 再将数组的前部分区间[0，M)进行逆置；
  reverseArr(arr + m, arr + n);  // 最后将数组的后部分区间[M，N)进行逆置；

  printf("%d", arr[0]);
  for (int i = 1; i < n; i++) {
    printf(" %d", arr[i]);
  }

  return 0;
}
```


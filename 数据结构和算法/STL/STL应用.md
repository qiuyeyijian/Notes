# STL应用

## 1. 存放主数据

算法设计的重要步骤是设计数据的存储结构，除非特别指定，程序员可以采用STL中的容器存放主要数据，选择何种容器不仅要考虑数据的类型，还要考虑数据的处理过程。

> 有一段英文由若干个单词组成，单词之间用一个空格分隔。编写程序提取其中的所有单词。
>
> 输入：The following code computes the intersection of two arrays
>
> 输出：
>
> The
>
> following
>
> code
>
> ...

```cpp
#include<vector>
#include<string>
#include<iostream>
//有的编译器不能使用printf，所以需要包含stdio.h头文件
#include<stdio.h>
using namespace std;

void splite(string str, vector<string>& words) {
	// 初始化一个空字符串
	string word = "";
	// 单词开始的位置
	unsigned int wordStart = 0;
	// 单词结束的位置， 如果没有找到空格就返回-1
	unsigned int wordEnd = str.find(" ");

	while (wordEnd != -1) {
		// 提取当前单词
		word = str.substr(wordStart, wordEnd - wordStart);
		// 保存这个单词
		words.push_back(word);
		// 继续寻找下一个单词开始和结束位置
		wordStart = wordEnd + 1;
		wordEnd = str.find(" ", wordStart);
	}

	// 最后一个单词单独提取
	if (wordStart < str.length() - 1) {
		words.push_back(str.substr(wordStart));
	}

}

int main() {

	string str = "The following code computes the intersection of two arrays";
	vector<string> words;
	// 调用单词分割函数
	splite(str, words);
	for (vector<string>::iterator it = words.begin(); it != words.end(); it++) {
		//cout << *it << endl;
		printf("%s\n", (*it).c_str());
	}


	return 0;
}
```



## 存放临时数据

在算法设计中有时需要存放一些临时数据，通常情况是，如果后存入的元素先处理，可以使用stack（栈）容器；如果先存入的元素先处理，可以使用queue（队）容器；如果元素的处理顺序按某个优先级进行，则可以使用priority_queue（优先队列）容器。

> 设计一个算法，判断一个含有`(), [], {}`3种类型括号的表达式中所有括号是否匹配。
>
> 示例1：
>
> 输入：`(a + [b-c] +d)`
>
> 输出：`(a + [b-c] +d)`中括号匹配
>
> 示例2：
>
> 输入：`(a + [b-c} +d)`
>
> 输出：`(a + [b-c} +d)`中括号不匹配

```cpp
#include<stack>
#include<string>
#include<iostream>
//有的编译器不能使用printf，所以需要包含stdio.h头文件
#include<stdio.h>
using namespace std;

bool isMatch(string str) {
	stack<char> st;
	for (int i = 0; i < str.length(); i++) {
		if (str[i] == '(' || str[i] == '[' || str[i] == '{') {
			st.push(str[i]);
		}
		else if (str[i] == ')') {
			// 若与栈顶元素相匹配，则弹出栈顶元素，否则返回false
			if (st.top() == '(') {
				st.pop();
			}
			else {
				return false;
			}
		}
		else if (str[i] == ']') {
			// 若与栈顶元素相匹配，则弹出栈顶元素，否则返回false
			if (st.top() == '[') {
				st.pop();
			}
			else {
				return false;
			}
		}
		else if (str[i] == '}') {
			// 若与栈顶元素相匹配，则弹出栈顶元素，否则返回false
			if (st.top() == '{') {
				st.pop();
			}
			else {
				return false;
			}
		}
		
	}
	// 如果之前都没有返回false, 则说明都匹配，返回true
	return true;
}

int main() {
	string str1 = "(a + [ b - c] + d)";
	string str2 = "(a + [ b - c} + d)";

	isMatch(str1) ? printf("%s 中括号匹配\n", str1.c_str()) : printf("%s 中括号不匹配\n", str1.c_str());
	isMatch(str2) ? printf("%s 中括号匹配\n", str2.c_str()) : printf("%s 中括号不匹配\n", str2.c_str());
	return 0;
}
```



## 检测数据元素的唯一性

> 设计一个算法判断字符串str中的每个字符是否唯一。例如，“abc”的每个字符是唯一的，算法返回true，而“accb”中的字符‘c’不是唯一的，算法返回false。

```cpp
bool isUnique(string& str) {
	map<char, int> myMap;
	for (int i = 0; i < str.length(); i++) {
        // 以str[i]作为key,第一次自增之后value为1,
        // 之后如果再凭借key找到这个元素，使其自增，则说明重复了
		myMap[str[i]]++;
		if (myMap[str[i]] > 1) {
			return false;
		}
	}
	return true;
}
```


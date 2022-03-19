# Python

## PIP



```python
# 安装包
pip install 包名 -i https://mirrors.aliyun.com/pypi/simple/
# 查看所有已安装的包
pip list
# 查看某个包的详细信息
pip show 包名

pip freeze > requirements.txt
```



## 标准数据类型

Python3 中有六个标准的数据类型：

- Number（数字）
- String（字符串）
- List（列表）
- Tuple（元组）
- Set（集合）
- Dictionary（字典）

Python3 的六个标准数据类型中：

- **不可变数据（3 个）：**Number（数字）、String（字符串）、Tuple（元组）；
- **可变数据（3 个）：**List（列表）、Dictionary（字典）、Set（集合）

对于Number（数字），Python3 支持 **int、float、bool、complex（复数）**。



## 导入模块

首先你要了解 **import** 与 **from…import** 的区别。

- **import 模块**：导入一个模块；注：相当于导入的是一个文件夹，是个相对路径。
- **from…import**：导入了一个模块中的一个函数；注：相当于导入的是一个文件夹中的文件，是个绝对路径。

所以使用上的的区别是当引用文件时是:

```
import   //模块.函数

from…import  // 直接使用函数名使用就可以了
```

所以

**from…import \***：是把一个模块中所有函数都导入进来; 注：相当于：相当于导入的是一个文件夹中所有文件，所有函数都是绝对路径。

结论：

from…import *语句与import区别在于：

> **import** 导入模块，每次使用模块中的函数都要是定是哪个模块。
>
> **from…import \*** 导入模块，每次使用模块中的函数，直接使用函数就可以了；注因为已经知道该函数是那个模块中的了。

## 编程实验

**将两个列表合并成字典**

```python
a = ['name', 'age', 'sex']
b = ['Dong', 38, 'Male']

# 将两个列表创建字典
print(dict(zip(a, b)))
```

**生成0~100的20个随机数，前十个升序排列，后十个降序排列**

```python
import random

list_01 = []
list_02 = []
list_03 = []

# 随机产生20个0-100整数
for i in range(20):
    list_01.append(random.randint(0, 100))

# 分片
list_02 = list_01[0:10]
list_03 = list_01[10:20]

# 升序
list_02.sort()
# 降序
list_03.sort(reverse=True)
list_01 = list_02 + list_03

print(list_01)
```

**s = “ajldjlajfdljfddd”，编程实现去重并从小到大排序输出 “adfjl“。（去掉重复）**

```python
s = "ajldjlajfdljfddd"
# 利用集合去掉重复元素
a = set(s)

# 利用列表排序
b = list(a)
b.sort()

# 列表转字符串
print("".join(b))
```

**生成1000个0~100之间的随机整数，并统计每个元素出现的次数。（按照出现次数排序）**

```python
import random

list01 = []

# 生成一个字典，键是从0到1000，值初始化为0
d = {i: 0 for i in range(1000)}

for i in range(1000):
    list01.append(random.randint(0, 100))
    # 将生成的数对应于字典的键的值加1
    d[list01[i]] = d[list01[i]] + 1

# 将生成的字典按照值排序
d_order = sorted(d.items(), key=lambda x: x[1], reverse=True)

print(d_order)
```

**随机密码生成。编写程序，在26个英文字母大小写和9个数字组成的列表中随机生成8个6位密码。**

```python
import random

# 生成包含a-z, A-Z, 0-9的列表
listPool = [chr(i) for i in range(65, 91)] + [chr(i) for i in range(97, 123)] + [chr(i) for i in range(48, 58)]
password = ""

# 从列表中随机选择八个字母
for i in range(8):
    password += (listPool[random.randint(0, 61)])

print(password)

```

**编写程序,用户输入一个列表和2个整数作为下标,然后使用切片获取并输出列表中介于2个下标之间的元素组成的子列表。例如用户输入[1, 2, 3, 4, 5, 6]和2,5,程序输出[3, 4, 5, 6]。**

```python
s = input("请输入列表: ")

a, b = (input("请输入两个整数, 以空格分开: ").split(" "))

a = int(a)
b = int(b)

# 将列表字符串转化为列表
s = eval(s)

print(s[a:b])

```




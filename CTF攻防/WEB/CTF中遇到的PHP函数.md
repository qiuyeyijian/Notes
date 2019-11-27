## PHP 与 CTF

### 0x01. 利用 mb_substr() 函数 分割中文字符

 mb_substr() 函数返回字符串的一部分，之前我们学过 substr() 函数，它只针对英文字符，如果要分割的中文文字则需要使用 mb_substr()。 

``` php
<?php
echo mb_substr("菜鸟教程", 0, 2);
// 输出：菜鸟
?>
```

### 0x02. mb_strpos()查找字符串首次出现位置

> mb_strpos — 查找字符串在另一个字符串中首次出现的位置
其中，mb_strpos 按字处理，strpos 按字符处理


#### 语法

```php
int mb_strpos ( 
  string $haystack , 
  string $needle [, 
  int $offset = 0 [, 
  string $encoding = mb_internal_encoding() ]] 
  )
```

| 参数          | 描述                                                         |
| :------------ | :----------------------------------------------------------- |
| haystack      | 要被检查的 string。                                          |
| needle        | 在 haystack 中查找这个字符串。 和 strpos() 不同的是，数字的值不会被当做字符的顺序值。 |
| offset        | 搜索位置的偏移。如果没有提供该参数，将会使用 0。负数的 offset 会从字符串尾部开始统计。 |
| encoding      | encoding 参数为字符编码。如果省略，则使用内部字符编码。      |
| Return Values | 返回 string 的 haystack 中 needle 首次出现位置的数值。 如果没有找到 needle，它将返回 FALSE。 |

*参考：* 

 https://www.php.net/manual/en/function.mb-strpos.php 

https://www.jb51.net/article/134373.htm 

### 0x03. PHP chdir() 函数

#### 实例

改变当前的目录：

```
<?php
// 获取当前目录
echo getcwd() . "<br>";

// 改变目录
chdir("images");

// 获得当前目录
echo getcwd();
?>
```

结果：

```
/home/php
/home/php/images
```

### 0x04. preg_replace() 函数

preg_replace 函数执行一个正则表达式的搜索和替换。

#### 语法

```
mixed preg_replace ( mixed $pattern , mixed $replacement , mixed $subject [, int $limit = -1 [, int &$count ]] )
```

搜索 subject 中匹配 pattern 的部分， 以 replacement 进行替换。

参数说明：

- $pattern: 要搜索的模式，可以是字符串或一个字符串数组。
- $replacement: 用于替换的字符串或字符串数组。
- $subject: 要搜索替换的目标字符串或字符串数组。
- $limit: 可选，对于每个模式用于每个 subject 字符串的最大可替换次数。 默认是-1（无限制）。
- $count: 可选，为替换执行的次数。

> *mixed* 说明一个参数可以接受多种不同的（但不一定是所有的）类型。
>
> 例如 [gettype()](https://www.php.net/manual/zh/function.gettype.php) 可以接受所有的 PHP 类型，[str_replace()](https://www.php.net/manual/zh/function.str-replace.php) 可以接受字符串和数组。

#### 返回值

如果 subject 是一个数组， preg_replace() 返回一个数组， 其他情况下返回一个字符串。

如果匹配被查找到，替换后的 subject 被返回，其他情况下 返回没有改变的 subject。如果发生错误，返回 NULL。

#### Notice

> - $pattern 存在 /e 模式修正符，允许代码执行
> - /e 模式修正符，是 **preg_replace() ** 将 $replacement 当做php代码来执行

### 0x05. trim()函数

 trim() 函数移除字符串两侧的空白字符或其他预定义字符。 

#### 语法

```
trim(string,charlist)
```

| 参数       | 描述                                                         |
| :--------- | :----------------------------------------------------------- |
| *string*   | 必需。规定要检查的字符串。                                   |
| *charlist* | 可选。规定从字符串中删除哪些字符。如果被省略，则移除以下所有字符："\0" - NULL"\t" - 制表符"\n" - 换行"\x0B" - 垂直制表符"\r" - 回车" " - 空格 |

### 0x06. strcmp函数

strcmp ( string str1,stringstr1,stringstr2 )
str1是第一个字符串，str2是第二个字符串。如果 str1 小于 str2 返回 < 0； 如果 str1 大于 str2 返回 > 0；如果两者相等，返回 0。
但是如果我们传入非字符串类型的数据的时候，这个函数将发生错误，在5.3之前的php中，显示了报错的警告信息后，将return 0 ! 也就是虽然报了错，但却判定其相等了。
因此，如果传入一个非字符串类型的变量即可，一般情况下，我们我们传数组，就可以构造payload，例如：

```php
?a[]=123
```

### 0x07. eregi()函数

语法
```
int eregi(string pattern, string string, [array regs]);
```
定义和用法
eregi()函数在一个字符串搜索指定的模式的字符串。`搜索不区分大小写`。Eregi()可以特别有用的检查有效性字符串,如密码。

可选的输入参数规则包含一个数组的所有匹配表达式,他们被正则表达式的括号分组。

返回值
如果匹配成功返回true,否则,则返回false

> eregi()函数与ereg()函数基本相同，区别是ereg()搜索字符时候大小写敏感

### 0x08.MD5()函数

 有两种方法绕过：

1，md5()函数无法处理数组，如果传入的为数组，会返回NULL，所以两个数组经过加密后得到的都是NULL,也就是相等的。

2，利用==比较漏洞

如果两个字符经MD5加密后的值为 0exxxxx形式，就会被认为是科学计数法，且表示的是0*10的xxxx次方，还是零，都是相等的。

下列的字符串的MD5值都是0e开头的：
```
QNKCDZO

240610708

s878926199a

s155964671a

s214587387a

s214587387a
```
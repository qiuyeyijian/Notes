## PHP 与 CTF

### ~~1. 利用file读取文件代码~~

``` php
xxx.com/index.php?file=index.php
```

### 2. 利用 mb_substr() 函数 分割中文字符

 mb_substr() 函数返回字符串的一部分，之前我们学过 substr() 函数，它只针对英文字符，如果要分割的中文文字则需要使用 mb_substr()。 

``` php
<?php
echo mb_substr("菜鸟教程", 0, 2);
// 输出：菜鸟
?>
```

### 3. mb_strpos()查找字符串首次出现位置

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

### 6. PHP chdir() 函数

## 实例

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


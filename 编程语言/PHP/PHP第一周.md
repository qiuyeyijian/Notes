## Learn

### 1. PHP 并置运算符

在 PHP 中，只有一个字符串运算符。

并置运算符 (.) 用于把两个字符串值连接起来。

下面的实例演示了如何将两个字符串变量连接在一起：

``` php
<?php
$txt1="Hello world!";
$txt2="What a nice day!";
echo $txt1.$txt2;
?>
```

### 2. PHP strlen() 函数

有时知道字符串值的长度是很有用的。

strlen() 函数返回字符串的长度（字符数）。

下面的实例返回字符串 "Hello world!" 的长度：

``` php
<?php
echo strlen("Hello world!");
?>
```

###  3. PHP strpos() 函数

strpos() 函数用于在字符串内查找一个字符或一段指定的文本。

如果在字符串中找到匹配，该函数会返回第一个匹配的字符位置。如果未找到匹配，则返回 FALSE。

下面的实例在字符串 "Hello world!" 中查找文本 "world"：

``` php
<?php
echo strpos("Hello world!","world");
?>
```

 **提示：**在上面的实例中，字符串 "world" 的位置是 6。之所以是 6 而不是 7 的原因是，字符串中第一个字符的位置是 0，而不是 1。 

### 4. 组合比较符(PHP7+)

PHP7+ 支持组合比较符（combined comparison operator）也称之为太空船操作符，符号为 **<=>**。组合比较运算符可以轻松实现两个变量的比较，当然不仅限于数值类数据的比较。

语法格式如下：

```php
$c = $a <=> $b;
```

解析如下：

- 如果 **$a > $b**, 则 **$c** 的值为 **1**。
- 如果 **$a == $b**, 则 **$c** 的值为 **0**。
- 如果 **$a < $b**, 则 **$c** 的值为 **-1**。

### 5. PHP 关联数组

关联数组是使用您分配给数组的指定的键的数组。

这里有两种创建关联数组的方法：

``` php
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
```

or: 

``` php
$age['Peter']="35";
$age['Ben']="37";
$age['Joe']="43";
```

### 6. PHP - 数组排序函数

在本章中，我们将一一介绍下列 PHP 数组排序函数：

- sort() - 对数组进行升序排列
- rsort() - 对数组进行降序排列
- asort() - 根据关联数组的值，对数组进行升序排列
- ksort() - 根据关联数组的键，对数组进行升序排列
- arsort() - 根据关联数组的值，对数组进行降序排列
- krsort() - 根据关联数组的键，对数组进行降序排列

### 7. PHP 超级全局变量

PHP中预定义了几个超级全局变量（superglobals） ，这意味着它们在一个脚本的全部作用域中都可用。 你不需要特别说明，就可以在函数及类中使用。

PHP 超级全局变量列表:

- $GLOBALS
- $_SERVER
- $_REQUEST
- $_POST
- $_GET
- $_FILES
- $_ENV
- $_COOKIE
- $_SESSION

### 8. 冒泡排序

``` php
<?php
$arr = [10, 3, 5, 6, 2, 8, 23, 18];
for ($i = 0; $i < count($arr)-1; $i++) {
    for ($j = 0; $j < count($arr) - $i - 1;$j++) {
        if ($arr[$j]>$arr[$j+1]) {
            $temp = $arr[$j];
            $arr[$j] = $arr[$j+1];
            $arr[$j+1] = $temp;
        }
    }
}
echo "<br>";
print_r($arr);
```

### 9. PHP魔术常量

PHP 向它运行的任何脚本提供了大量的预定义常量。

不过很多常量都是由不同的扩展库定义的，只有在加载了这些扩展库时才会出现，或者动态加载后，或者在编译时已经包括进去了。

有八个魔术常量它们的值随着它们在代码中的位置改变而改变。

例如 __LINE__ 的值就依赖于它在脚本中所处的行来决定。这些特殊的常量不区分大小写，如下：

#### __LINE__

文件中的当前行号。

#### __FILE__

文件的完整路径和文件名。如果用在被包含文件中，则返回被包含的文件名。

自 PHP 4.0.2 起，__FILE__ 总是包含一个绝对路径（如果是符号连接，则是解析后的绝对路径），而在此之前的版本有时会包含一个相对路径。

#### __DIR__

文件所在的目录。如果用在被包括文件中，则返回被包括的文件所在的目录。

它等价于 dirname(__FILE__)。除非是根目录，否则目录中名不包括末尾的斜杠。（PHP 5.3.0中新增）

#### __FUNCTION__

函数名称（PHP 4.3.0 新加）。自 PHP 5 起本常量返回该函数被定义时的名字（区分大小写）。在 PHP 4 中该值总是小写字母的。

####  __CLASS__

类的名称（PHP 4.3.0 新加）。自 PHP 5 起本常量返回该类被定义时的名字（区分大小写）。

在 PHP 4 中该值总是小写字母的。类名包括其被声明的作用区域（例如 Foo\Bar）。注意自 PHP 5.4 起 __CLASS__ 对 trait 也起作用。当用在 trait 方法中时，__CLASS__ 是调用 trait 方法的类的名字。

#### __TRAIT__

Trait 的名字（PHP 5.4.0 新加）。自 PHP 5.4.0 起，PHP 实现了代码复用的一个方法，称为 traits。

Trait 名包括其被声明的作用区域（例如 Foo\Bar）。

从基类继承的成员被插入的 SayWorld Trait 中的 MyHelloWorld 方法所覆盖。其行为 MyHelloWorld 类中定义的方法一致。优先顺序是当前类中的方法会覆盖 trait 方法，而 trait 方法又覆盖了基类中的方法。

#### __METHOD__

类的方法名（PHP 5.0.0 新加）。返回该方法被定义时的名字（区分大小写）。

#### __NAMESPACE__

当前命名空间的名称（区分大小写）。此常量是在编译时定义的（PHP 5.3.0 新增）。

> ` 魔术常量的两边，每一边有两个下划线的。 `

### 10. PHP命名空间

 https://www.runoob.com/w3cnote/php-namespace-intro.html 

### 11. PHP self  parent 和  static

来自: https://www.jb51.net/article/163726.htm 

PHP群里有人询问self关键字的用法，答案是比较明显的：静态成员函数内不能用this调用非成员函数，但可以用self调用静态成员函数/变量/常量；其他成员函数可以用self调用静态成员函数以及非静态成员函数。随着讨论的深入，发现self并没有那么简单。鉴于此，本文先对几个关键字做对比和区分，再总结self的用法。

**与parent、static以及this的区别**

要想将彻底搞懂self，要与parent、static以及this区分开。以下分别做对比。

**parent**

self与parent的区分比较容易：parent引用父类/基类被隐盖的方法（或变量），self则引用自身方法（或变量）。例如构造函数中调用父类构造函数：

``` php
class Base {
 public function __construct() {
  echo "Base contructor!", PHP_EOL;
 }
}
 
class Child {
 public function __construct() {
  parent::__construct();
  echo "Child contructor!", PHP_EOL;
 }
}
 
new Child;
// 输出：
// Base contructor!
// Child contructor!
```

**static**

static常规用途是修饰函数或变量使其成为类函数和类变量，也可以修饰函数内变量延长其生命周期至整个应用程序的生命周期。但是其与self关联上是PHP 5.3以来引入的新用途：静态延迟绑定。

有了static的静态延迟绑定功能，可以在运行时动态确定归属的类。例如：

[?](https://www.jb51.net/article/163726.htm#)

```php
class Base {
 public function __construct() {
  echo "Base constructor!", PHP_EOL;
 }
 
 public static function getSelf() {
  return new self();
 }
 
 public static function getInstance() {
  return new static();
 }
 
 public function selfFoo() {
  return self::foo();
 }
 
 public function staticFoo() {
  return static::foo();
 }
 
 public function thisFoo() {
  return $this->foo();
 }
 
 public function foo() {
  echo "Base Foo!", PHP_EOL;
 }
}
 
class Child extends Base {
 public function __construct() {
  echo "Child constructor!", PHP_EOL;
 }
 
 public function foo() {
  echo "Child Foo!", PHP_EOL;
 }
}
 
$base = Child::getSelf();
$child = Child::getInstance();
 
$child->selfFoo();
$child->staticFoo();
$child->thisFoo();
```

程序输出结果如下：

> Base constructor!
> Child constructor!
> Base Foo!
> Child Foo!
> Child Foo!

在函数引用上，self与static的区别是：对于静态成员函数，self指向代码当前类，static指向调用类；对于非静态成员函数，self抑制多态，指向当前类的成员函数，static等同于this，动态指向调用类的函数。

parent、self、static三个关键字联合在一起看挺有意思，分别指向父类、当前类、子类，有点“过去、现在、未来”的味道。

**this**

self与this是被讨论最多，也是最容易引起误用的组合。两者的主要区别如下：

> 1. this不能用在静态成员函数中，self可以；
> 2. 对静态成员函数/变量的访问，建议 用self，不要用$this::或$this->的形式；
> 3. 对非静态成员变量的访问，不能用self，只能用this;
> 4. this要在对象已经实例化的情况下使用，self没有此限制；
> 5. 在非静态成员函数内使用，self抑制多态行为，引用当前类的函数；而this引用调用类的重写(override)函数（如果有的话）

**self的用途**

看完与上述三个关键字的区别，self的用途是不是呼之即出？一句话总结，那就是：self总是指向“当前类（及类实例）”。详细说则是：

1. 替代类名，引用当前类的静态成员变量和静态函数；
2. 抑制多态行为，引用当前类的函数而非子类中覆盖的实现；

**槽点**

1. 这几个关键字中，只有this要加$符号且必须加，强迫症表示很难受；
2. 静态成员函数中不能通过$this->调用非静态成员函数，但是可以通过self::调用，且在调用函数中未使用$this->的情况下还能顺畅运行。此行为貌似在不同PHP版本中表现不同，在当前的7.3中ok；
3. 在静态函数和非静态函数中输出self，猜猜结果是什么？都是string(4) "self"，迷之输出；
4. return $this instanceof static::class;会有语法错误，但是以下两种写法就正常：

```php
$class = static::class;
return $this instanceof $class;
// 或者这样：
return $this instanceof static;
```

所以这是为什么啊？！

**参考**

[When to use self over $this?](https://stackoverflow.com/questions/151969/when-to-use-self-over-this)

### 12. $_GET、$_POST 和 $_REQUEST 的区别？

**$_GET** 变量接受所有以 **get** 方式发送的请求，及浏览器地址栏中的 **?** 之后的内容。

**$_POS**T 变量接受所有以 post 方式发送的请求，例如，一个 form 以 **method=post** 提交，提交后 php 会处理 post 过来的全部变量。

**$_REQUEST** 支持两种方式发送过来的请求，即 **post** 和 **get** 它都可以接受，显示不显示要看传递方法，get 会显示在 url 中（有字符数限制），post 不会在 url 中显示，可以传递任意多的数据（只要服务器支持）

### 13. 什么是 htmlspecialchars()方法?

htmlspecialchars() 函数把一些预定义的字符转换为 HTML 实体。

- & （和号） 成为 &amp;
- " （双引号） 成为 &quot;
- ' （单引号） 成为 &#039;
- < （小于） 成为 &lt;
- \> （大于） 成为 &gt;

### 14. PHP表单验证

当用户提交表单时，我们将做以下两件事情：

1. 使用 PHP trim() 函数去除用户输入数据中不必要的字符 (如：空格，tab，换行)。
2. 使用PHP stripslashes()函数去除用户输入数据中的反斜杠 (\)

接下来让我们将这些过滤的函数写在一个我们自己定义的函数中，这样可以大大提高代码的复用性。

将函数命名为 test_input()。

现在，我们可以通过test_input()函数来检测 $_POST 中的所有变量, 脚本代码如下所示：

``` php
<?php
// 定义变量并默认设置为空值
$name = $email = $gender = $comment = $website = "";
 
if ($_SERVER["REQUEST_METHOD"] == "POST")
{
  $name = test_input($_POST["name"]);
  $email = test_input($_POST["email"]);
  $website = test_input($_POST["website"]);
  $comment = test_input($_POST["comment"]);
  $gender = test_input($_POST["gender"]);
}
 
function test_input($data)
{
  $data = trim($data);
  $data = stripslashes($data);
  $data = htmlspecialchars($data);
  return $data;
}
?>
```



### 15. PHP require 和 include 的区别

> - require 一般放在 PHP 文件的最前面，程序在执行前就会先导入要引用的文件；
> - include 一般放在程序的流程控制中，当程序执行时碰到才会引用，简化程序的执行流程。
>
> - require 引入的文件有错误时，执行会中断，并返回一个致命错误；
> - include 引入的文件有错误时，会继续执行，并返回一个警告。
>
>  https://www.runoob.com/w3cnote/php-different-include-and-require.html 

### 16. PHP 上传文件

通过使用 PHP 的全局数组 $_FILES，你可以从客户计算机向远程服务器上传文件。

第一个参数是表单的 input name，第二个下标可以是 "name"、"type"、"size"、"tmp_name" 或 "error"。如下所示：

> - $_FILES["file"]["name"] - 上传文件的名称
> - $_FILES["file"]["type"] - 上传文件的类型
> - $_FILES["file"]["size"] - 上传文件的大小，以字节计
> - $_FILES["file"]["tmp_name"] - 存储在服务器的文件的临时副本的名称
> - $_FILES["file"]["error"] - 由文件上传导致的错误代码

### 17. 函数和过滤器

如需过滤变量，请使用下面的过滤器函数之一：

> - filter_var() - 通过一个指定的过滤器来过滤单一的变量
> - filter_var_array() - 通过相同的或不同的过滤器来过滤多个变量
> - filter_input - 获取一个输入变量，并对它进行过滤
> - filter_input_array - 获取多个输入变量，并通过相同的或不同的过滤器对它们进行过滤
>
>  https://www.runoob.com/php/php-ref-filter.html 

``` php
<?php
$filters = array
(
    "name" => array
    (
        "filter"=>FILTER_SANITIZE_STRING
    ),
    "age" => array
    (
        "filter"=>FILTER_VALIDATE_INT,
        "options"=>array
        (
            "min_range"=>1,
            "max_range"=>120
        )
    ),
    "email"=> FILTER_VALIDATE_EMAIL
);
 
$result = filter_input_array(INPUT_GET, $filters);
 
if (!$result["age"])
{
    echo("年龄必须在 1 到 120 之间。<br>");
}
elseif(!$result["email"])
{
    echo("E-Mail 不合法<br>");
}
else
{
    echo("输入正确");
}
?>
```

### 18.可变数量的参数列表

PHP 在用户自定义函数中支持可变数量的参数列表。在 PHP 5.6 及以上的版本中，由 *...* 语法实现；在 PHP 5.5 及更早版本中，使用函数[func_num_args()](http://cn.php.net/manual/zh/function.func-num-args.php)，[func_get_arg()](http://cn.php.net/manual/zh/function.func-get-arg.php)，和 [func_get_args()](http://cn.php.net/manual/zh/function.func-get-args.php) 。

在PHP 5.6及更高版本中，参数列表可能包含...标记，表示该函数接受可变数量的参数。参数将作为数组传递给给定变量

```php
<?php
//声明时使用
function sum(...$numbers) {
    $acc = 0;
    foreach ($numbers as $n) {
        $acc += $n;
    }
    return $acc;
}
//10
echo sum(1, 2, 3, 4);
?>
```

``` php
<?php
//调用时使用
function add($a, $b) {
    return $a + $b;
}
//3
echo add(...[1, 2])."\n";
//3
$a = [1, 2];
echo add(...$a);
?>
```


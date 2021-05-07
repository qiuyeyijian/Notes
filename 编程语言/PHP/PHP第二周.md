## 第二周

### 1. PHP 标量类型声明

默认情况下，所有的PHP文件都处于弱类型校验模式。

PHP 7 增加了标量类型声明的特性，标量类型声明有两种模式:

> - 强制模式 (默认)
> - 严格模式

标量类型声明语法格式：

```
declare(strict_types=1); 
```

代码中通过指定 strict_types的值（1或者0），1表示严格类型校验模式，作用于函数调用和返回语句；0表示弱类型校验模式。

可以使用的类型参数有：

> - int
> - float
> - bool
> - string
> - interfaces
> - array
> - callable

**对于标量类型声明：在严格模式下，有一种例外的情况是：当函数参数为float时，传入int型变量不会跑出typeerror，而是正常执行，在返回类型声明中，也是同样的:**

```php
<?php
declare(strict_types = 1);
function test (float $inter) {
    return $inter;
}

echo test(2); // 结果为2

function test1(int $inte) : float{
    return $inte;
}
echo test1(1); // 结果为1
?>
```

### 2. PHP 连接mysql 数据库

``` php
//创建数据库（面向对象）
<?php
$servername = "localhost";
$username = "username";
$password = "password";
 
// 创建连接
$conn = new mysqli($servername, $username, $password);
// 检测连接
if ($conn->connect_error) {
    die("连接失败: " . $conn->connect_error);
} 
 
// 创建数据库
$sql = "CREATE DATABASE myDB";
if ($conn->query($sql) === TRUE) {
    echo "数据库创建成功";
} else {
    echo "Error creating database: " . $conn->error;
}
 
$conn->close();
?>
```

### 3. PHP 创建数据表

我们将创建一个名为 "MyGuests" 的表，有 5 个列： "id", "firstname", "lastname", "email" 和 "reg_date":

``` sql
CREATE TABLE MyGuests (
    id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    firstname VARCHAR(30) NOT NULL,
    lastname VARCHAR(30) NOT NULL,
    email VARCHAR(50),
    reg_date TIMESTAMP
)
```

**上表中的注意事项:**

数据类型指定列可以存储什么类型的数据。完整的数据类型请参考我们的 [数据类型参考手册](https://www.runoob.com/sql/sql-datatypes.html)。

在设置了数据类型后，你可以为每个列指定其他选项的属性：

> - NOT NULL - 每一行都必须含有值（不能为空），null 值是不允许的。
> - DEFAULT value - 设置默认值
> - UNSIGNED - 使用无符号数值类型，0 及正数
> - AUTO INCREMENT - 设置 MySQL 字段的值在新增记录时每次自动增长 1
> - PRIMARY KEY - 设置数据表中每条记录的唯一标识。 通常列的 PRIMARY KEY 设置为 ID 数值，与 AUTO_INCREMENT 一起使用。

每个表都应该有一个主键(本列为 "id" 列)，主键必须包含唯一的值。


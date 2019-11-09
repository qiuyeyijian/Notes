## CTF 中PHP为协议总结

### PHP中支持的伪协议

```php
file:// — 访问本地文件系统
http:// — 访问 HTTP(s) 网址
ftp:// — 访问 FTP(s) URLs
php:// — 访问各个输入/输出流（I/O streams）
zlib:// — 压缩流
data:// — 数据（RFC 2397）
glob:// — 查找匹配的文件路径模式
phar:// — PHP 归档
ssh2:// — Secure Shell 2
rar:// — RAR
ogg:// — 音频流
expect:// — 处理交互式的流
```



### php://filter

php://filter 是一种元封装器， 设计用于数据流打开时的[筛选过滤](https://www.php.net/manual/zh/filters.php)应用。 这对于一体式（all-in-one）的文件函数非常有用，类似 [readfile()](https://www.php.net/manual/zh/function.readfile.php)、 [file()](https://www.php.net/manual/zh/function.file.php) 和 [file_get_contents()](https://www.php.net/manual/zh/function.file-get-contents.php)， 在数据流内容读取之前没有机会应用其他过滤器。

php://filter 目标使用以下的参数作为它路径的一部分。 复合过滤链能够在一个路径上指定。详细使用这些参数可以参考具体范例。

| 名称                        | 描述                                                         |
| :-------------------------- | :----------------------------------------------------------- |
| *resource=<要过滤的数据流>* | 这个参数是必须的。它指定了你要筛选过滤的数据流。             |
| *read=<读链的筛选列表>*     | 该参数可选。可以设定一个或多个过滤器名称，以管道符（*\|*）分隔。 |
| *write=<写链的筛选列表>*    | 该参数可选。可以设定一个或多个过滤器名称，以管道符（*\|*）分隔。 |
| *<；两个链的筛选列表>*      | 任何没有以 *read=* 或 *write=* 作前缀 的筛选器列表会视情况应用于读或写链。 |

在CTF 比赛中 php://filter 常用于读取一个页面的源码

``` php
http://XXX/index.php?file=php://filter/read=convert.base64-encode/resource=index.php
```

也可以去掉 `read`绕过一些限制函数

``` php
index.php?file=php://filter/convert.base64-encode/resource=index.php
```

### php://input

 php://input 是个可以访问请求的原始数据的只读流，可以读取到来自POST的原始数据。但当 enctype=”multipart/form-data” 的时候 php://input 是无效的。 

利用条件：

1. allow_url_include = On。
2. 对allow_url_fopen不做要求。

```php
http:/xxx/index.php?file=php://input
```

![image-20191109215758429](PHPfilter.assets/image-20191109215758429.png)

   ### phar://

利用条件

1. phar文件要能够上传到服务器端。如`file_exists()`，`fopen()`，`file_get_contents()`，`file()`等文件操作函数

2. 要有可用的魔术方法作为“跳板”。

3. 文件操作函数的参数可控，且`:`、`/`、`phar`等特殊字符没有被过滤。

   ![image-20191109224717795](PHP%E4%BC%AA%E5%8D%8F%E8%AE%AE.assets/image-20191109224717795.png)

文件是被放到upload文件夹下，构造payload：

``` php
http://xxx/index.php?file=phar://upload/phar.gif
```


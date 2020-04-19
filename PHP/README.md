## PHP



### PHP bing壁纸



```php
<?php
$str = file_get_contents('http://cn.bing.com/HPImageArchive.aspx?idx=0&n=1');   // 从bing获取数据
 
if(preg_match('/<url>([^<]+)<\/url>/isU', $str, $matches)) { // 正则匹配抓取图片url
    $imgurl = 'http://cn.bing.com'.$matches[1];
} else {  // 如果由于某些原因，没抓取到图片地址
    $imgurl = 'http://img.infinitynewtab.com/InfinityWallpaper/2_14.jpg'; // 使用默认的图像(默认图像链接可修改为自己的)
}
 
header("Location: {$imgurl}");    // 跳转至目标图像
```



### PHP MySql 数据库

#### 连接数据库封装函数

```php
<?php
function getDbConnection() {
    $username = $_POST['username'];
    $password = $_POST['password'];

    $hostname = "localhost";
    $dbName = "cloth_design";
    $dbUsername = "cloth_design";
    $dbPassword = "cxx982121";
    $dbConnection = new mysqli($hostname, $dbUsername, $dbPassword, $dbName);
//Check connection
    if ($dbConnection->connect_error) {
        die("连接失败： ".$dbConnection->connect_error);
    }
    else {
//        echo "连接成功";
        return $dbConnection;
    }
}
```



#### 登录

```php
<?php
include_once("function/database.php");
// $userName = $_POST['userName'];
// $password = $_POST['password'];
$username = $_POST['username'];
$password =$_POST['password'];

$dbConnection = getDbConnection();

$loginSQL = "select * from users where username='$username' and password='$password'";
$result = mysqli_query($dbConnection, $loginSQL);
//while ($row=mysqli_fetch_array($result, MYSQLI_ASSOC)) {
//    echo $row['username'].$row['password'];
//    echo "hhh";
//}
if(mysqli_num_rows($result)) {
    echo "ok";
}
else {
    echo "error";
}

$dbConnection->close();

```



#### 注册

```php
<?php
include_once ("function/database.php");
$username = $_POST['username'];
$password = $_POST['password'];

$dbConnection = getDbConnection();

$sql = "INSERT INTO users(username, password) values ('$username', '$password')";

if ($dbConnection->query($sql) === TRUE) {
    echo "插入成功";
}
else {
    echo $sql.$dbConnection->error;
}

$dbConnection->close();
```



### php cookie

#### 语法

```php
setcookie(name, value, expire, path, domain);
```



```php
<?php
$expire=time()+60*60*24*30;
setcookie("user", "runoob", $expire);
?>

<html>
.....
```

在上面的实例中，过期时间被设置为一个月（*60 秒 \* 60 分 \* 24 小时 \* 30 天*）。



### php curl_func.php

curl 封装函数

```php
<?php

/**
 * 使用：
 * echo curlOpen('http://www.baidu.com');
 *
 * POST数据
 * $post = array('aa'=>'ddd','ee'=>'d')
 * 或
 * $post = 'aa=ddd&ee=d';
 * echo curlOpen('http://www.baidu.com',array('post'=>$post));
 * @param string $url
 * @param array $config
 * @return bool|string
 */
function curlOpen($url, $config = array())
{
    $arr = array('post' => false,'referer' => $url,'cookie' => '', 'useragent' => 'Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.0; Trident/4.0; SLCC1; .NET CLR 2.0.50727; .NET CLR 3.0.04506; customie8)', 'timeout' => 20, 'return' => true, 'proxy' => '', 'userpwd' => '', 'nobody' => false,'header'=>array(),'gzip'=>true,'ssl'=>false,'isupfile'=>false);
    $arr = array_merge($arr, $config);
    $ch = curl_init();

    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, $arr['return']);
    curl_setopt($ch, CURLOPT_NOBODY, $arr['nobody']);
    curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
    curl_setopt($ch, CURLOPT_USERAGENT, $arr['useragent']);
    curl_setopt($ch, CURLOPT_REFERER, $arr['referer']);
    curl_setopt($ch, CURLOPT_TIMEOUT, $arr['timeout']);
    //curl_setopt($ch, CURLOPT_HEADER, true);//获取header
    if($arr['gzip']) curl_setopt($ch, CURLOPT_ENCODING, 'gzip,deflate');
    if($arr['ssl'])
    {
        curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
        curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, false);
    }
    if(!empty($arr['cookie']))
    {
        curl_setopt($ch, CURLOPT_COOKIEJAR, $arr['cookie']);
        curl_setopt($ch, CURLOPT_COOKIEFILE, $arr['cookie']);
    }

    if(!empty($arr['proxy']))
    {
        //curl_setopt($ch, CURLOPT_PROXYTYPE, CURLPROXY_HTTP);
        curl_setopt ($ch, CURLOPT_PROXY, $arr['proxy']);
        if(!empty($arr['userpwd']))
        {
            curl_setopt($ch,CURLOPT_PROXYUSERPWD,$arr['userpwd']);
        }
    }

    //ip比较特殊，用键值表示
    if(!empty($arr['header']['ip']))
    {
        array_push($arr['header'],'X-FORWARDED-FOR:'.$arr['header']['ip'],'CLIENT-IP:'.$arr['header']['ip']);
        unset($arr['header']['ip']);
    }
    $arr['header'] = array_filter($arr['header']);

    if(!empty($arr['header']))
    {
        curl_setopt($ch, CURLOPT_HTTPHEADER, $arr['header']);
    }

    if ($arr['post'] != false)
    {
        curl_setopt($ch, CURLOPT_POST, true);
        if(is_array($arr['post']) && $arr['isupfile'] === false)
        {
            $post = http_build_query($arr['post']);
        }
        else
        {
            $post = $arr['post'];
        }
        curl_setopt($ch, CURLOPT_POSTFIELDS, $post);
    }
    $result = curl_exec($ch);
    //var_dump(curl_getinfo($ch));
    curl_close($ch);

    return $result;
}
```


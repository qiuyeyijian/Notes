## Redis



### php连接redis测试



```php
 <?php

 //连接本地的 Redis 服务
 
 $redis = new Redis();
 
 $redis->connect('127.0.0.1', 6379);
 
    //连接redis密码认证，成功返回true 失败返回false

 $ret = $redis->auth('TSDC@tdfybim102424');
 
 //echo "Connection to server sucessfully";
 
 //查看服务是否运行
 
 echo "Server is running: " . $redis->ping();
```


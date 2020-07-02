## IDEA 快捷键



### 1. 搜索和替换

```
Ctrl+Shift+R.
```



### idea 连接MySQL报错

``` mysql
Server returns invalid timezone. Go to 'Advanced' tab and set 'serverTimezone' property manually. 
```



#### 首选方法

只需要在连接语句后面加上`?serverTimezone=GMT`

```mysql
jdbc:mysql://localhost:3306?serverTimezone=GMT
```

#### 方法二

时区错误，MySQL默认的时区是UTC时区，比北京时间晚8个小时。
所以要修改mysql的时长
使用cmd
找到mysql安装目录并进入bin文件夹
输入
mysql -u root -p
然后输入密码，进入mysql命令模式
输入
set global time_zone=’+8:00’;
再次连接成功

#### 方法三


> 有可能是数据库版本问题，换个数据库驱动文件试试
>


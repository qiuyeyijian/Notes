# README

## Chrome

### 1. 快捷键

```
//撤销关闭的标签页
Ctrl + Shift + T

//历史记录
Ctrl + h
```

### 2. 截图

```
1. 打开控制台
2. Ctrl + Shift + P 
3. 然后输入 screen 
4. 选择capture full size screenshot
```



## VMWare

### 开机自动挂载共享文件夹

手动挂载（每次开机都需要重新挂载）

```bash
vmhgfs-fuse .host:/VMShare /root/workspace
```

开机自动挂载

```bash
vim /etc/fstab
# /VMShare是共享文件夹名称，挂载到/root/workspace
.host:/VMShare /root/workspace fuse.vmhgfs-fuse allow_other 0 0	
```



## Jupyter 安装与启动

```bash
pip install jupyter

# 进入到你的工作目录，然后 
jupyter notebook
```







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



### 一直在reading pom.xml文件并且卡住

今天安装了IDEA ultimate版本，安装之后新建web项目时一直在reading pom.xml文件并且卡住不动了，在导入该项目时也一直在reading pom.xml。问题不清晰，只能百度，查了很多方式并且试了之后都没用。最后找到了以下方法：

win+ R、打开cmd、输入

```shell
netsh winsock reset
```

然后重启电脑

执行完之后就解决问题了。

据说是网络配置问题。导入maven项目时，第一次是要从远程仓库下载pom.xml里定义的，然而一直在reading，确定是网络配置问题！





## Visual Studio

### ctrl+鼠标左键点击后，返回原来位置的方法

我们常常用到**Ctrl+鼠标左键**的方式来查看类或变量名的定义声明，看完之后我们想回到程序原来的位置，此时可以通过**Alt + ←（方向左键）**来返回到原来的位置。



## PPT

中文字体：微软雅黑

英文字体：Arial

| 大标题             | 小标题        | 正文 |
| ------------------ | ------------- | ---- |
| 微软雅黑 加粗 24PX | 微软雅黑 20PX | 14PX |

| 大标题             | 小标题        | 正文 |
| ------------------ | ------------- | ---- |
| 微软雅黑 加粗 28PX | 微软雅黑 24PX | 18PX |



## Visual Studio Code

### 添加头文件包含路径

http://www.360doc.com/content/21/0802/19/38894361_989248738.shtml

https://www.cnblogs.com/unrulife/p/14319466.html

https://www.likecs.com/show-203839382.html






































































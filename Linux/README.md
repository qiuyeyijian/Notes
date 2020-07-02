## 遇到和使用的一些命令

### 1. yum -y install与yum install有什么不同

如果使用yum install xxxx，会找到安装包之后，询问你Is this OK[y/d/N]，需要你手动进行选择。但是如果加上参数-y，就会自动选择y，不需要你再手动选择！

``` 
yum -y install 包名（支持*） ：自动选择y，全自动
yum install 包名（支持*） ：手动选择y or n
yum remove 包名（不支持*）
rpm -ivh 包名（支持*）：安装rpm包
rpm -e 包名（不支持*）：卸载rpm包
```

### 2. cp ，mv, rm命令

```bash
cp [-f -i -r] 源文件 目标文件
mv [-f -i] 源文件 目标文件、
rm [-f -i -r]
ls [-l -i -d -a]			//-l：显示详细信息, -d: 显示该目录的信息
mkdir [-p]					//创建多级目录
rmdir [-p]					//删除多级目录
file 文件名				  //显示文件类型
```

> -f : 强制覆盖重名文件，不询问
>
> -i: 询问
>
> -r: 递归拷贝，用于目录
>



### 软件安装

rpm安装：

```bash
rpm -i example.rpm 安装 example.rpm 包；
rpm -iv example.rpm 安装 example.rpm 包并在安装过程中显示正在安装的文件信息；
rpm -ivh example.rpm 安装 example.rpm 包并在安装过程中显示正在安装的文件信息及安装进度
```



常用命令：

```
rpm 常用命令
1.安装一个包 （展示正在安装的文件信息以及安装进度）
# rpm -ivh 
 
2.升级一个包 
# rpm -Uvh 
 
3.卸载一个包 
# rpm -e 
 
4.安装参数 
--force 即使覆盖属于其它包的文件也强迫安装 
--nodeps 如果该RPM包的安装依赖其它包，即使其它包没装，也强迫安装。 
 
5.查询一个包是否被安装 
# rpm -q < rpm package name> 
 
6.得到被安装的包的信息 
# rpm -qi < rpm package name> 
 
7.列出该包中有哪些文件 
# rpm -ql < rpm package name> 
 
8.列出服务器上的一个文件属于哪一个RPM包 
#rpm -qf 
 
9.可综合好几个参数一起用 
# rpm -qil < rpm package name> 
 
10.列出所有被安装的rpm package 
# rpm -qa 
 
11.列出一个未被安装进系统的RPM包文件中包含有哪些文件？ 
# rpm -qilp < rpm package name>
 
12.解压RPM包
 
有时我们需要RPM包中的某个文件，如何解压RPM包呢？
 
RPM包括是使用cpio格式打包的，因此可以先转成cpio然后解压，如下所示：

```







### .查看ip地址

```
ifconfig
```



### 文件处理

> 归档：也称为打包，指的是一个文件或目录的集合，而这个集合被存储在一个文件中。归档文件没有经过压缩，因此，它占用的空间是其中所有文件和目录的总和。
>
> 压缩：压缩文件也是一个文件和目录的集合，且这个集合也被存储在一个文件中，但它们的不同之处在于，**压缩文件采用了不同的存储方式，使其所占用的磁盘空间比集合中所有文件大小的总和要小。**



#### tar.gz(常用)

​    tar.gz这种格式是Linux下使用得最多的压缩格式。它在压缩时不会占用太多CPU的，而且可以得到一个非常理想的压缩率。

##### 格式

```
tar [选项] [源文件或目录]
```

```
-c 将多个文件或目录进行打包。
-A 追加tar文件到归档文件。
-f 包名，指定包的文件名。包的扩展名是用来给管理员识别格式的，所以一定要正确指定扩展名；
-v 显示打包文件过程；
-x 对tar包做解打包操作。
-t 只查看tar包中有哪些文件或目录，不对tar包做解打包操作。
-C 目录指定解打包位置。
-z 压缩和解压缩 ".tar.gz" 格式；
-j 压缩和解压缩 ".tar.bz2"格式。
```

##### 常用命令

```bash
tar -zcvf filename.tar.gz [directory]				//压缩一个目录
tar -zxvf filename.tar.gz							//解压缩到当前文件夹
tar -zxvf filename.tar.gz -C /usr/temp/				//解压缩到指定目录下
```




#### zip：
> 注意：zip压缩命令需要手工指定压缩之后的压缩包名，注意写清楚扩展名，以便解压缩时使用，扩展名为.zip

##### 格式
```bash
zip [选项] 压缩包名 源文件或源目录列表
```

```bash
-r 递归压缩目录，及将制定目录下的所有文件以及子目录全部压缩。
-m 将文件压缩之后，删除原始文件，相当于把文件移到压缩文件中。
-v 显示详细的压缩过程信息。
-q 在压缩的时候不显示命令的执行过程。
-压缩级别 压缩级别是从 1~9 的数字，-1 代表压缩速度更快，-9 代表压缩效果更好。
-u 更新压缩文件，即往压缩文件中添加新文件。
-i 只包含指定文件
-x 排除指定文件压缩
```

##### 常用命令

```bash
压缩：zip filename.zip dirname
```



#### unzip：

##### 格式
```bash
unzip [选项] 压缩包名
```

```bash
-d 目录名，将压缩文件解压到指定目录下。
-n 解压时并不覆盖已经存在的文件。
-o 解压时覆盖已经存在的文件，并且无需用户确认。
-v 查看压缩文件的详细信息，包括压缩文件中包含的文件大小、文件名以及压缩比等，但并不做解压操作。
-t 测试压缩文件有无损坏，但并不解压。
-x 文件列表 解压文件，但不包含文件列表中指定的文件。
```

##### 常用命令

```bash
解压：unzip filename.zip
```



### 终端显示与关闭回显

```bash
屏蔽显示：

stty -echo   #禁止回显
stty echo    #打开回显
```



#### gzip：

> gzip是Linux系统中经常用来对文件进行压缩和解压缩的命令，通过此命令压缩得到的新文件，其扩展名通常标记为“.gz”。
> 注意：gzip命令只能用来压缩文件，不能压缩目录，即便指定了目录，也只能压缩目录内的所有文件

##### 格式
```bash
gzip [选项] 源文件
```

```bash
-c 将压缩数据输出到标准输出中，并保留源文件。#例子gzip -c vc > bb.gz
-d 对压缩文件进行解压缩。
-r 递归压缩指定目录下以及子目录下的所有文件。
-v 对于每个压缩和解压缩的文件，显示相应的文件名和压缩比。
-l 对每一个压缩文件，显示以下字段：压缩文件的大小；未压缩文件的大小；压缩比；未压缩文件的名称。
-数字 用于指定压缩等级，-1 压缩等级最低，压缩比最差；-9 压缩比最高。默认压缩比是 -6
```



#### gunzip：

##### 格式

```bash
gunzip [选项] 文件
```

```bash
-r 递归处理，解压缩指定目录下以及子目录下的所有文件。
-c 把解压缩后的文件输出到标准输出设备。
-f 强制解压缩文件，不理会文件是否已存在等情况。
-l 列出压缩文件内容。
-v 显示命令执行过程。
-t 测试压缩文件是否正常，但不对其做解压缩操作。
```



####  磁盘分区

```bash
ls /dev | grep sd				//查看当前系统挂载的硬盘

fdisk /dev/sda					//对sda硬盘进行操作

mkfs -t ext4 /dev/sdb1			//格式化主分区


```

### 系统管理

#### 添加组

```bash
groupadd <组名>
```

```bash
-g <gid> 	为新组指定GID，其默认值大于500，且大于系统其他组的GID, 与 -r 互斥
-o       	改组的GID可以不唯一
-r			添加一个系统账号组，指定小于499的第一个未使用数值为该系统组的GID，与 -g 互斥、
-f			若正在创建的组已经存在，将不报错而强制添加该组
```

例：在系统添加一个新组 `student` , 并为其指定GID是600

```bash
groupadd -g 600 student
```

#### 删除组

```bash
groupdel <组名>
```

#### 修改组

```bash
groupmod [-g <新的GID> [-o]] [-n <新组名>] <现有组名>
```

例：把组`student`  的GID值修改为 700， 并将组名改为 `teacher`

```bash
groupmod -g 700 -n teacher student
```

#### 组管理

```bash
gpasswd
```

```bash
-a：添加用户到组；
-d：从组删除用户；
-A：指定管理员；
-M：指定组成员和-A的用途差不多；
-r：删除密码；
-R：限制用户登入组，只有组中的成员才可以用newgrp加入该组。
```

例：给test 组创建一个密码

```bash
[root@localhost tmp]# gpasswd test
Changing the password for group test
New Password:
Re-enter new password:
```

添加user1，让user1来管理test组

```bash
[root@localhost tmp]# useradd user1
[root@localhost tmp]# gpasswd -A user1 test
```

创建user2，user3用户，并且让user1添加这两个账户到test组中

```bash
[root@localhost tmp] useradd user2
[root@localhost tmp] useradd user
[root@localhost tmp] su - user1
[user1@localhost ~] gpasswd -a user2 test
Adding user user2 to group test
[user1@localhost ~] gpasswd -a user3 test
Adding user user3 to group test
```

查看/etc/group文件进行验证

```bash
[root@localhost ~] tail -n 10 /etc/group | grep test
test:x:1001:user2,user3
```



#### 添加用户

```bash
useradd [选项] [用户名]
```

```bash
-c 注释信息，通常为用户 passwd 文件的用户名字段指定用户名或其他相关信息
-d 指定新用户的主目录，默认值是 /home/用户名
-e 用户账号失效日期，在此日期后，该账号将失效,时间格式为 yyyy-mm-dd
-p 指定用户密码
-s 指定用户登录的默认Sehll环境，若不指定参数，则有系统为其指定默认Shell程序
-u 指定用户标志号的数值，系统默认值的下限为99，并且要大于系统中现存的其他用户UID,通常0-99的UID		值保留给系统账号
-G 指定新用户的附加组，该参数对应于 /etc/group 文件中的用户列表字段
-g 指定用户所在的组名或登录时初始组标志号
-n 在创建新用户的账户时，使该用户所在组的组名与该用户的登录名相同
-f 指定用户密码过期后到该用户账号被永久查封之前
```

例：创建一个用户账号 `qiuyeyijian` ，主目录为 `/home/qiuyeyijian`， 登录时使用 bash 作为其Shell程序

```bash
useradd -d /home/qiuyeyijian -s /bin/bash qiuyeyijian
```

创建一个用户 `qiuyeyijian`, 初始组为 `student`附加组为 `teacher`, 

```bash
useradd -g student -G teacher qiuyeyijian
```



#### 修改用户

```bash
usermod <参数> 用户名
```

```bash
-d:修改用户的主目录     #usermod  -d  /ljj/user1  user1
-l:修改用户的账号名称  #usermod  -l  user11  user1
-g:更改用户的基本组
-G：更改用户的附加组   #usermod  -G  group1  user1 
-c:修改备注信息
-e:修改账号的有效期限
-s:修改登录时使用的Shell  
-L:锁定用户密码
-U：解除用户密码
```



#### 删除用户

```bash
userdel 用户名
```

```bash
-r 将用户主目录及该目录下的文件删除，也会将该用户在系统中的其他文件删除
```





### 查看端口占用并删除

```bash
[root@onepiece ~]# lsof -i
# 将会显示 命令 + 进程ID + 进程所属用户, 以及监听的协议、状态等信息
COMMAND     PID USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
dhclient    728 root    6u  IPv4   11262      0t0  UDP *:bootpc
ntpd        839  ntp   16u  IPv4   13671      0t0  UDP *:ntp
ntpd        839  ntp   18u  IPv4   13677      0t0  UDP localhost:ntp
```

使用`lsof`查看指定端口占用情况

```bash
lsof -i:8081
```

使用`netstat`查看指定端口占用情况

```bash
netstat -anp | grep 8081
```

杀死某个端口的占用进程

```shell
kill -s 9 9646(进程号)
```



### Springboot 部署项目

```bash
nohup java -jar test.jar >temp.txt &
```


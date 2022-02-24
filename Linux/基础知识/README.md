# Linux

## 入门知识

### systemd 初始化进程

http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html

根据 Linux 惯例，字母`d`是守护进程（daemon）的缩写。

Linux系统的开机过程是这样的，即先从BIOS开始，然后进入Boot Loader，再加载系统内核，然后内核进行初始化，最后启动初始化进程。初始化进程作为Linux系统启动后的第一个正式服务，它需要完成Linux系统中相关的初始化工作，为用户提供合适的工作环境。同学们可以将初始化进程粗犷地理解成从我们按下开机键到看见系统桌面的这个过程。初始化进程完成了一大半工作。

红帽RHEL 7/8系统替换掉了熟悉的初始化进程服务System V init，正式采用全新的systemd初始化进程服务。

Linux系统在启动时要进行大量的初始化工作，比如挂载文件系统和交换分区、启动各类进程服务等，这些都可以看作是一个一个的单元（unit），systemd用目标（target）代替了System V init中运行级别的概念。

服务的启动、重启、停止、重载、查看状态等常用命令

| 老系统命令          | 新系统命令              | 作用                           |
| ------------------- | ----------------------- | ------------------------------ |
| service foo start   | systemctl start httpd   | 启动服务                       |
| service foo restart | systemctl restart httpd | 重启服务                       |
| service foo stop    | systemctl stop httpd    | 停止服务                       |
| service foo reload  | systemctl reload httpd  | 重新加载配置文件（不终止服务） |
| service foo status  | systemctl status httpd  | 查看服务状态                   |

服务开机启动、不启动、查看各级别下服务启动状态等常用命令

| 老系统命令        | 新系统命令                             | 作用                               |
| ----------------- | -------------------------------------- | ---------------------------------- |
| chkconfig foo on  | systemctl enable httpd                 | 开机自动启动                       |
| chkconfig foo off | systemctl disable httpd                | 开机不自动启动                     |
| chkconfig foo     | systemctl is-enabled httpd             | 查看特定服务是否为开机自启动       |
| chkconfig --list  | systemctl list-unit-files --type=httpd | 查看各个级别下服务的启动与禁用情况 |



### 其他

在执行命令时在末尾添加一个&符号，这样命令将进入系统后台来执行。





## 软件管理

### RPM

RPM（红帽软件包管理器），有点像Windows系统中的控制面板，会建立统一的数据库，详细记录软件信息并能够自动分析依赖关系。

### YUM

尽管RPM能够帮助用户查询软件之间的依赖关系，但问题还是要运维人员自己来解决，而有些大型软件可能与数十个程序都有依赖关系，在这种情况下安装软件依然很繁琐。

Yum软件仓库便是为了进一步降低软件安装难度和复杂度而设计的技术。Yum软件仓库可以根据用户的要求分析出所需软件包及其相关的依赖关系，然后自动从服务器下载软件包并安装到系统。

**RPM是为了简化安装复杂度，而YUM软件仓库是为了解决软件包之间的依赖关系。**

### DNF

DNF实际上就是解决了上述问题的Yum软件仓库的提升版，行业内称之为Yum v4版本。

作为Yum软件仓库v3版本的接替者，DNF特别友好地继承了原有的命令格式，且使用习惯上也保持了一致。大家不用担心不会操作。

以前，安装软件用的命令是“yum install软件包名称”，那么现在则是“dnf install软件包名称”（也就是说，将yum替换成dnf即可）。



## 系统工作命令

|        |      |        |          |      |      |
| ------ | ---- | ------ | -------- | ---- | ---- |
| echo   | date | reboot | poweroff | wget | ps   |
| pstree |      |        |          |      |      |
|        |      |        |          |      |      |
|        |      |        |          |      |      |
|        |      |        |          |      |      |

> 在Linux系统中的命令参数有长短格式之分，长格式和长格式之间不能合并，长格式和短格式之间也不能合并，但短格式和短格式之间是可以合并的，合并后仅保留一个减号（-）即可。
>
> 另外ps命令可允许参数不加减号（-），因此可直接写成ps aux的样子。

### wget

wget命令用于在终端命令行中下载网络文件，英文全称为“web get”

| 参数 | 作用                                 |
| ---- | ------------------------------------ |
| -b   | 后台下载模式                         |
| -P   | 下载到指定目录                       |
| -t   | 最大尝试次数                         |
| -c   | 断点续传                             |
| -p   | 下载页面内所有资源，包括图片、视频等 |
| -r   | 递归下载                             |



### ps

ps命令用于查看系统中的进程状态，英文全称为“processes”

| 参数 | 作用                               |
| ---- | ---------------------------------- |
| -a   | 显示所有进程（包括其他用户的进程） |
| -u   | 用户以及其他详细信息               |
| -x   | 显示没有控制终端的进程             |

在Linux系统中有5种常见的进程状态，分别为运行、中断、不可中断、僵死与停止，其各自含义如下所示。

> **R（运行）**：进程正在运行或在运行队列中等待。
>
> **S（中断）**：进程处于休眠中，当某个条件形成后或者接收到信号时，则脱离该  状态。
>
> **D（不可中断）**：进程不响应系统异步信号，即便用kill命令也不能将其中断。
>
> **Z（僵死）**：进程已经终止，但进程描述符依然存在, 直到父进程调用wait4()系统函数后将进程释放。
>
> **T（停止）**：进程收到停止信号后停止运行。

**除了上面5种常见的进程状态，还有可能是高优先级（<）、低优先级（N）、被锁进内存（L）、包含子进程（s）以及多线程（l）这5种补充形式。**

| USER         | PID      | %CPU         | %MEM       | VSZ                      | RSS                        | TTY      | STAT     | START        | TIME              | COMMAND        |
| ------------ | -------- | ------------ | ---------- | ------------------------ | -------------------------- | -------- | -------- | ------------ | ----------------- | -------------- |
| 进程的所有者 | 进程ID号 | 运算器占用率 | 内存占用率 | 虚拟内存使用量(单位是KB) | 占用的固定内存量(单位是KB) | 所在终端 | 进程状态 | 被启动的时间 | 实际使用CPU的时间 | 命令名称与参数 |



### pstree

pstree命令用于以树状图的形式展示进程之间的关系，英文全称为“process tree”

### top

top命令用于动态地监视进程活动及系统负载等信息，输入该命令后按回车键执行即可。

前面介绍的命令都是静态地查看系统状态，不能实时滚动最新数据，而top命令能够动态地查看系统状态，因此完全可以将它看作是Linux中“强化版的Windows任务管理器”。



top命令执行结果的前5行为系统整体的统计信息，其所代表的含义如下。

> 第1行：系统时间、运行时间、登录终端数、系统负载（3个数值分别为1分钟、5分钟、15分钟内的平均值，数值越小意味着负载越低）。
>
> 第2行：进程总数、运行中的进程数、睡眠中的进程数、停止的进程数、僵死的进程数。
>
> 第3行：用户占用资源百分比、系统内核占用资源百分比、改变过优先级的进程资源百分比、空闲的资源百分比等。其中数据均为CPU数据并以百分比格式显示，例如“99.9 id”意味着有99.9%的CPU处理器资源处于空闲。
>
> 第4行：物理内存总量、内存空闲量、内存使用量、作为内核缓存的内存量。
>
> 第5行：虚拟内存总量、虚拟内存空闲量、虚拟内存使用量、已被提前加载的内存量。

> PID — 进程id
> USER — 进程所有者
> PR — 进程优先级
> NI — nice值。负值表示高优先级，正值表示低优先级
> VIRT — 进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES
> RES — 进程使用的、未被换出的物理内存大小，单位kb。RES=CODE+DATA
> SHR — 共享内存大小，单位kb
> S —进程状态。D=不可中断的睡眠状态 R=运行 S=睡眠 T=跟踪/停止 Z=僵尸进程
> %CPU — 上次更新到现在的CPU时间占用百分比
> %MEM — 进程使用的物理内存百分比
> TIME+ — 进程使用的CPU时间总计，单位1/100秒
> COMMAND — 进程名称（命令名/命令行）



### nice

nice命令用于调整进程的优先级，语法格式为“nice优先级数字 服务名称”。

在top命令输出的结果中，PR和NI值代表的是进程的优先级，数字越低（取值范围是-20～19），优先级越高。在日常的生产工作中，可以将一些不重要进程的优先级调低，让紧迫的服务更多地利用CPU和内存资源，以达到合理分配系统资源的目的。例如将bash服务的优先级调整到最高：

```bash
nice -n -20 bash
```

### pidof

pidof命令用于查询某个指定服务进程的PID号码值，语法格式为“pidof [参数] 服务名称”。

每个进程的进程号码值（PID）是唯一的，可以用于区分不同的进程。

```bash
pidof sshd
```

### kill

kill命令用于终止某个指定PID值的服务进程，语法格式为“kill [参数] 进程的PID”。

接下来，使用kill命令把上面用pidof命令查询到的PID所代表的进程终止掉，其命令如下所示。这种操作的效果等同于强制停止sshd服务。

```
kill 2156
```

但有时系统会提示进程无法被终止，此时可以加参数-9，表示最高级别地强制杀死进程：

```
kill -9 2156
```

### killall

killall命令用于终止某个指定名称的服务所对应的全部进程，语法格式为“killall [参数] 服务名称”。

通常来讲，复杂软件的服务程序会有多个进程协同为用户提供服务，如果用kill命令逐个去结束这些进程会比较麻烦，此时可以使用killall命令来批量结束某个服务程序带有的全部进程。

```bash
killall sshd
```



## 系统状态监测命令

### ifconfig

ifconfig命令用于获取网卡配置与网络状态等信息，英文全称为“interface config”，语法格式为“ifconfig [参数] [网络设备]”。

使用ifconfig命令来查看本机当前的网卡配置与网络状态等信息时，其实主要查看的就是网卡名称、inet参数后面的IP地址、ether参数后面的网卡物理地址（又称为MAC地址），以及RX、TX的接收数据包与发送数据包的个数及累计流量（即下面加粗的信息内容）：



### uname

uname命令用于查看系统内核版本与系统架构等信息，英文全称为“unix name”，语法格式为“uname [-a]”。

在使用uname命令时，一般要固定搭配上-a参数来完整地查看当前系统的内核名称、主机名、内核发行版本、节点名、压制时间、硬件名称、硬件平台、处理器类型以及操作系统名称等信息：

```bash
uname -a

cat /etc/redhat-release		# 查看当前系统版本的详细信息
```



### uptime

uptime命令用于查看系统的负载信息，输入该命令后按回车键执行即可。

uptime命令真的很棒，它可以显示当前系统时间、系统已运行时间、启用终端数量以及平均负载值等信息。平均负载值指的是系统在最近1分钟、5分钟、15分钟内的压力情况（下面加粗的信息部分），负载值越低越好：

> “负载值越低越好”是对运维人员来讲的，越低表示越安全省心。但是公司购置的硬件设备如果长期处于空闲状态，则明显是种资源浪费，老板也不会开心。所以建议负载值保持在1左右，在生产环境中不要超过5就好。



### free

free命令用于显示当前系统中内存的使用量信息，语法格式为“free [-h]”。

为了保证Linux系统不会因资源耗尽而突然宕机，运维人员需要时刻关注内存的使用量。在使用free命令时，可以结合使用-h参数以更人性化的方式输出当前内存的实时使用量信息。

| 内存总量 | 已用量 | 空闲量 | 进程共享的内存量 | 磁盘缓存的内存量 | 缓存的内存量 | 可用量    |
| -------- | ------ | ------ | ---------------- | ---------------- | ------------ | --------- |
| total    | used   | free   | shared           | buffers          | buff/cache   | available |

如果不使用-h（易读模式）查看内存使用量情况，则默认以KB为单位。



### who

who命令用于查看当前登入主机的用户终端信息，这3个简单的字母可以快速显示出所有正在登录本机的用户名称以及他们正在开启的终端信息；如果有远程用户，还会显示出来访者的IP地址。

| 登陆的用户名 | 终端设备 | 登陆到系统的时间        |
| ------------ | -------- | ----------------------- |
| root         | tty2     | 2020-07-24 06:26 (tty2) |

### last

last命令用于调取主机的被访记录。Linux系统会将每次的登录信息都记录到日志文件中

### ping

执行ping命令时，系统会使用ICMP向远端主机发出要求回应的信息，若连接远端主机的网络没有问题，远端主机会回应该信息。

```bash
ping -c 4 qiuyeyijian.com		# -c 表示请求次数
```

### tracepath

tracepath命令用于显示数据包到达目的主机时途中经过的所有路由信息，语法格式为“tracepath [参数] 域名”。

当两台主机之间无法正常ping通时，要考虑两台主机之间是否有错误的路由信息，导致数据被某一台设备错误地丢弃。这时便可以使用tracepath命令追踪数据包到达目的主机时途中的所有路由信息，以分析是哪台设备出了问题。

### netstat

netstat命令用于显示如网络连接、路由表、接口状态等的网络相关信息，英文全称为“network status”，语法格式为“netstat [参数]”。

| -a   | 显示所有连接中的Socket   |
| ---- | ------------------------ |
| -p   | 显示正在使用的Socket信息 |
| -t   | 显示TCP协议的连接状态    |
| -u   | 显示UDP协议的连接状态    |
| -n   | 使用IP地址，不使用域名   |
| -l   | 仅列出正在监听的服务状态 |
| -i   | 显示网卡列表信息         |
| -r   | 显示路由表信息           |





















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



## 文件管理

> 归档：也称为打包，指的是一个文件或目录的集合，而这个集合被存储在一个文件中。归档文件没有经过压缩，因此，它占用的空间是其中所有文件和目录的总和。
>
> 压缩：压缩文件也是一个文件和目录的集合，且这个集合也被存储在一个文件中，但它们的不同之处在于，**压缩文件采用了不同的存储方式，使其所占用的磁盘空间比集合中所有文件大小的总和要小。**



### tar压缩解压

tar.gz这种格式是Linux下使用得最多的压缩格式。它在压缩时不会占用太多CPU的，而且可以得到一个非常理想的压缩率。

```
tar [选项] [源文件或目录]
```

```shell
-z 压缩和解压缩 ".tar.gz" 格式；
-x 对tar包做解打包操作。
-c 将多个文件或目录进行打包。
-v 显示打包文件过程；
-f 包名，指定包的文件名。包的扩展名是用来给管理员识别格式的，所以一定要正确指定扩展名；

-A 追加tar文件到归档文件。
-t 只查看tar包中有哪些文件或目录，不对tar包做解打包操作。
-C 目录指定解打包位置。
-j 压缩和解压缩 ".tar.bz2"格式。
```

```bash
tar -zcvf filename.tar.gz [directory]				//压缩一个目录
tar -zxvf filename.tar.gz							//解压缩到当前文件夹
tar -zxvf filename.tar.gz -C /usr/temp/				//解压缩到指定目录下
```



### 查找文件

```shell
find ./ -name *.txt -exec mv {} ./ \;
```

```shell
-exec 后面跟着命令，以分号结束。分号前面加上反斜杠\，防止终端将分号解释成其他意思。
{} 表示find查找的结果，中间不能有空格

```








### 终端显示与关闭回显

```bash
屏蔽显示：

stty -echo   #禁止回显
stty echo    #打开回显
```



## 磁盘管理



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


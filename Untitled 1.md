原创

# linux常用命令（详解）

2018-08-29 09:45:41  更多



版权声明：本文为博主原创文章，遵循[ CC 4.0 BY-SA ](http://creativecommons.org/licenses/by-sa/4.0/)版权协议，转载请附上原文出处链接和本声明。本文链接：https://blog.csdn.net/qq_36802111/article/details/82177844

# 一、日常使用命令/常用快捷键命令

## 开关机命令

​        **1、shutdown –h now**：立刻进行关机

​        **2、shutdown –r now：**现在重新启动计算机

​        **3、reboot：**现在重新启动计算机

​        **4、su -：**切换用户；**passwd：**修改用户密码

​        **5、logout：**用户注销

## 常用快捷命令

​        **1、tab** = 补全

​        2、**ctrl + l -**：清屏，类似**clear**命令

​        3、**ctrl + r** -：查找历史命令（**history**）；**ctrl+c** = 终止

​        4、**ctrl+k** = 删除此处至末尾所有内容

​        5、**ctrl+u** = 删除此处至开始所有内容

## 常用工具命令

### man:帮助命令     wc:文本统计统计         wordcount          3      5         29         a.txt          行数    单词数    字符数    文件名         常见参数：             -l：只查看行数             -w: 只查看单词数             -c：只查看字符数     du:文件大小统计         格式：du [选项参数] dir_path         常见参数：                 -s:只统计该文件目录的大小，不递归                 -h:人性化的显示单位     find:文件检索命令

```
语法 find   path   -option   [   -print ]   [ -exec   -ok   command ]   {} \; 参数说明 : find 根据下列规则判断 path 和 expression，在命令列上第一个 - ( ) , ! 之前的部份为 path，之后的是 expression。如果 path 是空字串则使用目前路径，如果 expression 是空字串则使用 -print 为预设 expression。 expression 中可使用的选项有二三十个之多，在此只介绍最常用的部份。 -mount, -xdev : 只检查和指定目录在同一个文件系统下的文件，避免列出其它文件系统中的文件 -amin n : 在过去 n 分钟内被读取过 -anewer file : 比文件 file 更晚被读取过的文件 -atime n : 在过去n天内被读取过的文件 -cmin n : 在过去 n 分钟内被修改过 -cnewer file :比文件 file 更新的文件 -ctime n : 在过去n天内被修改过的文件 -empty : 空的文件-gid n or -group name : gid 是 n 或是 group 名称是 name -ipath p, -path p : 路径名称符合 p 的文件，ipath 会忽略大小写 -name name, -iname name : 文件名称符合 name 的文件。iname 会忽略大小写 -size n : 文件大小 是 n 单位，b 代表 512 位元组的区块，c 表示字元数，k 表示 kilo bytes，w 是二个位元组。-type c : 文件类型是 c 的文件。 d: 目录 c: 字型装置文件 b: 区块装置文件 p: 具名贮列 f: 一般文件 l: 符号连结 s: socket -pid n : process id 是 n 的文件 你可以使用 ( ) 将运算式分隔，并使用下列运算。 exp1 -and exp2 ! expr -not expr exp1 -or exp2 exp1, exp2实例 将目前目录及其子目录下所有延伸档名是 c 的文件列出来。 # find . -name "*.c" 将目前目录其其下子目录中所有一般文件列出 # find . -type f 将目前目录及其子目录下所有最近 20 天内更新过的文件列出 # find . -ctime -20 查找/var/log目录中更改时间在7日以前的普通文件，并在删除之前询问它们： # find /var/log -type f -mtime +7 -ok rm {} \; 查找前目录中文件属主具有读、写权限，并且文件所属组的用户和其他用户具有读权限的文件： # find . -type f -perm 644 -exec ls -l {} \; 为了查找系统中所有文件长度为0的普通文件，并列出它们的完整路径： # find / -type f -size 0 -exec ls -l {} \;
```

# 二、常用目录/文件操作命令

## 1.展示目录列表命令ls（list）

  ls             展示当前目录下的可见文件
  ls -a         展示当前目录下所有的文件（包括隐藏的文件）
  ls -l(ll)      展示当前目录下文件的详细信息
  ll -a          展示当前目录下所有文件的详细信息
  ll -h          友好的显示当前目录下文件的详细信息（其实就是文件的大小可读性更强了）

  pwd：显示目前的目录

## 2.切换目录命令cd（change directory）

  cd test         切换到test目录下
  cd .. 切换到上一级目录
  cd / 切换到系统根目录下
  cd ~ 切换到当前用户的根目录下
  cd - 切换到上一级所在的目录

## 3.目录的创建（mkdir）和删除（rmdir）命令

  mkdir test 在当前目录下创建一个test目录
  mkdir -p test/a/b 在test目录下的a目录下创建一个b目录，如果上一级目录不存在，则连它的父目录一起创建
  rmdir test 删除当前目录下的test目录（注意：该命令只能够删除空目录）

## 4.文件的创建（touch）和删除（rm）命令

  touch test.txt         在当前目录下创建一个test.txt的文件
  rm test.txt 删除test.txt的文件（带询问的删除，需输入y才能删除）
  rm -f test.txt 直接删除text.txt文件
  rm -r test 递归删除，即删除test目录以及其目录下的子目录（带询问的删除）
  rm -rf test 直接删除test目录以及其目录下的子目录

## 5.文件打包或解压命令tar

1. 1. 1. 1. 打包并压缩文件

Linux中的打包文件一般是以.tar结尾的，压缩的命令一般是以.gz结尾的。

而一般情况下打包和压缩是一起进行的，打包并压缩后的文件的后缀名一般.tar.gz。

命令：tar -zcvf 打包压缩后的文件名 要打包压缩的文件

其中：z：调用gzip压缩命令进行压缩

  c：打包文件

  v：显示运行过程

  f：指定文件名

示例：打包并压缩/test下的所有文件 压缩后的压缩包指定名称为xxx.tar.gz

tar -zcvf xxx.tar.gz aaa.txt bbb.txt ccc.txt

或：tar -zcvf xxx.tar.gz /test/*

 

|      |                                                   |
| ---- | ------------------------------------------------- |
|      | ![img](Untitled%201.assets/20180829092441219.png) |





1. 1. 1. 1. 解压压缩包**（重点）**

命令：tar [-xvf] 压缩文件

其中：x：代表解压

示例：将/test下的xxx.tar.gz解压到当前目录下

 

|      |                                                   |
| ---- | ------------------------------------------------- |
|      | ![img](Untitled%201.assets/20180829092441350.png) |


tar -xvf xxx.tar.gz

示例：将/test下的xxx.tar.gz解压到根目录/usr下

**tar -xvf xxx.tar.gz -C /usr------C代表指定解压的位置**

 

|      |                                                   |
| ---- | ------------------------------------------------- |
|      | ![img](Untitled%201.assets/20180829092441336.png) |











 

 

1. 1. 1. Linux的权限命令

权限是Linux中的重要概念，每个文件/目录等都具有权限，通过ls -l命令我们可以 查看某个目录下的文件或目录的权限

 

文件的类型：

d：代表目录

-：代表文件

l：代表链接（可以认为是window中的快捷方式）

后面的9位分为3组，每3位置一组，分别代表属主的权限，与当前用户同组的     用户的权限，其他用户的权限

r：代表权限是可读，r也可以用数字4表示

w：代表权限是可写，w也可以用数字2表示

x：代表权限是可执行，x也可以用数字1表示

 

 

| **属主（****user****）** | **属组（****group****）** | **其他用户** |      |      |      |      |      |      |
| ------------------------ | ------------------------- | ------------ | ---- | ---- | ---- | ---- | ---- | ---- |
| r                        | w                         | x            | r    | w    | x    | r    | w    | x    |
| 4                        | 2                         | 1            | 4    | 2    | 1    | 4    | 2    | 1    |

**linux中用户的分类        小李     小李对象    老王        所有者u    同组用户g    其他人o    linux中文件权限        读r        写w        执行x    没有权限-            文件详情信息：        -rw-r--r--. 1 root root       5 Aug 28 02:27 a.txt            d rwx r-x r-x. 2 root root    4096 Aug 27 08:52 test        第一位：d:目录，-：文件        rw-                r--                r--        所有者           同组用户        其他人        只有读写          只有读            只有读            1：该文件的链接数    root：文件所属者    root：文件所属组     5 Aug 28 02:27：最后的修改时间**

**修改文件/目录的权限的命令：chmod**

**示例：修改/test下的aaa.txt的权限为属主有全部权限，属主所在的组有读写权限，**

**其他用户只有读的权限**

**chmod u=rwx,g=rw,o=r aaa.txt**

**上述示例还可以使用数字表示：**

**chmod 764 aaa.txt**

 

**修改文件的所属用户和所属组 chown        chown username:groupName aa.txt        chown username: aa.txt        chown :groupName aa.txt            -R：递归子目录修改所属者和所属组**

## 三、文件/文件夹的cp rm及文件的查看

### cp (复制文件或目录)

cp 即拷贝文件和目录。

语法:

```
[root@www ~]# cp [-adfilprsu] 来源档(source) 目标档(destination)
[root@www ~]# cp [options] source1 source2 source3 .... directory
```

选项与参数：

- **-a：**相当於 -pdr 的意思，至於 pdr 请参考下列说明；(常用)
- **-d：**若来源档为连结档的属性(link file)，则复制连结档属性而非文件本身；
- **-f：**为强制(force)的意思，若目标文件已经存在且无法开启，则移除后再尝试一次；
- **-i：**若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用)
- **-l：**进行硬式连结(hard link)的连结档创建，而非复制文件本身；
- **-p：**连同文件的属性一起复制过去，而非使用默认属性(备份常用)；
- **-r：**递归持续复制，用於目录的复制行为；(常用)
- **-s：**复制成为符号连结档 (symbolic link)，亦即『捷径』文件；
- **-u：**若 destination 比 source 旧才升级 destination ！

用 root 身份，将 root 目录下的 .bashrc 复制到 /tmp 下，并命名为 bashrc

```
[root@www ~]# cp ~/.bashrc /tmp/bashrc
[root@www ~]# cp -i ~/.bashrc /tmp/bashrc
cp: overwrite `/tmp/bashrc'? n  <==n不覆盖，y为覆盖
```

### rm (移除文件或目录)

语法：

```
 rm [-fir] 文件或目录
```

选项与参数：

- -f ：就是 force 的意思，忽略不存在的文件，不会出现警告信息；
- -i ：互动模式，在删除前会询问使用者是否动作
- -r ：递归删除啊！最常用在目录的删除了！这是非常危险的选项！！！
-  

将刚刚在 cp 的实例中创建的 bashrc 删除掉！

```
[root@www tmp]# rm -i bashrc
rm: remove regular file `bashrc'? y
```

如果加上 -i 的选项就会主动询问喔，避免你删除到错误的档名！

### mv (移动文件与目录，或修改名称)

语法：

```
[root@www ~]# mv [-fiu] source destination
[root@www ~]# mv [options] source1 source2 source3 .... directory
```

选项与参数：

- -f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；
- -i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！
- -u ：若目标文件已经存在，且 source 比较新，才会升级 (update)

复制一文件，创建一目录，将文件移动到目录中

```
[root@www ~]# cd /tmp
[root@www tmp]# cp ~/.bashrc bashrc
[root@www tmp]# mkdir mvtest
[root@www tmp]# mv bashrc mvtest
```

将某个文件移动到某个目录去，就是这样做！

将刚刚的目录名称更名为 mvtest2

```
[root@www tmp]# mv mvtest mvtest2
```

------

## Linux 文件内容查看

Linux系统中使用以下命令来查看文件的内容：

- cat  由第一行开始显示文件内容
- tac  从最后一行开始显示，可以看出 tac 是 cat 的倒著写！
- nl   显示的时候，顺道输出行号！
- more 一页一页的显示文件内容
- less 与 more 类似，但是比 more 更好的是，他可以往前翻页！
- head 只看头几行
- tail 只看尾巴几行

你可以使用 *man [命令]*来查看各个命令的使用文档，如 ：man cp。

### cat

由第一行开始显示文件内容

语法：

```
cat [-AbEnTv]
```

选项与参数：

- -A ：相当於 -vET 的整合选项，可列出一些特殊字符而不是空白而已；
- -b ：列出行号，仅针对非空白行做行号显示，空白行不标行号！
- -E ：将结尾的断行字节 $ 显示出来；
- -n ：列印出行号，连同空白行也会有行号，与 -b 的选项不同；
- -T ：将 [tab] 按键以 ^I 显示出来；
- -v ：列出一些看不出来的特殊字符

检看 /etc/issue 这个文件的内容：

```
[root@www ~]# cat /etc/issue
CentOS release 6.4 (Final)
Kernel \r on an \m
```

### tac

tac与cat命令刚好相反，文件内容从最后一行开始显示，可以看出 tac 是 cat 的倒着写！如：

```
[root@www ~]# tac /etc/issue

Kernel \r on an \m
CentOS release 6.4 (Final)
```

### nl

显示行号

语法：

```
nl [-bnw] 文件
```

选项与参数：

- -b ：指定行号指定的方式，主要有两种：
  -b a ：表示不论是否为空行，也同样列出行号(类似 cat -n)；
  -b t ：如果有空行，空的那一行不要列出行号(默认值)；
- -n ：列出行号表示的方法，主要有三种：
  -n ln ：行号在荧幕的最左方显示；
  -n rn ：行号在自己栏位的最右方显示，且不加 0 ；
  -n rz ：行号在自己栏位的最右方显示，且加 0 ；
- -w ：行号栏位的占用的位数。

实例一：用 nl 列出 /etc/issue 的内容

```
[root@www ~]# nl /etc/issue
     1  CentOS release 6.4 (Final)
     2  Kernel \r on an \m
```

### more

一页一页翻动

```
[root@www ~]# more /etc/man.config
#
# Generated automatically from man.conf.in by the
# configure script.
#
# man.conf from man-1.6d
....(中间省略)....
--More--(28%)  <== 重点在这一行喔！你的光标也会在这里等待你的命令
```

在 more 这个程序的运行过程中，你有几个按键可以按的：

- 空白键 (space)：代表向下翻一页；
- Enter         ：代表向下翻『一行』；
- /字串         ：代表在这个显示的内容当中，向下搜寻『字串』这个关键字；
- :f            ：立刻显示出档名以及目前显示的行数；
- q             ：代表立刻离开 more ，不再显示该文件内容。
- b 或 [ctrl]-b ：代表往回翻页，不过这动作只对文件有用，对管线无用。

### less

一页一页翻动，以下实例输出/etc/man.config文件的内容：

```
[root@www ~]# less /etc/man.config
#
# Generated automatically from man.conf.in by the
# configure script.
#
# man.conf from man-1.6d
....(中间省略)....
:   <== 这里可以等待你输入命令！
```

less运行时可以输入的命令有：

- 空白键    ：向下翻动一页；
- [pagedown]：向下翻动一页；
- [pageup]  ：向上翻动一页；
- /字串     ：向下搜寻『字串』的功能；
- ?字串     ：向上搜寻『字串』的功能；
- n         ：重复前一个搜寻 (与 / 或 ? 有关！)
- N         ：反向的重复前一个搜寻 (与 / 或 ? 有关！)
- q         ：离开 less 这个程序；

### head

取出文件前面几行

语法：

```
head [-n number] 文件 
```

选项与参数：

- -n ：后面接数字，代表显示几行的意思

```
[root@www ~]# head /etc/man.config
```

默认的情况中，显示前面 10 行！若要显示前 20 行，就得要这样：

```
[root@www ~]# head -n 20 /etc/man.config
```

### tail

取出文件后面几行

语法：

```
tail [-n number] 文件 
```

选项与参数：

- -n ：后面接数字，代表显示几行的意思
- -f ：表示持续侦测后面所接的档名，要等到按下[ctrl]-c才会结束tail的侦测

```
[root@www ~]# tail /etc/man.config
# 默认的情况中，显示最后的十行！若要显示最后的 20 行，就得要这样：
[root@www ~]# tail -n 20 /etc/man.config
```

------

# 系统常用操作命令

### visudo:编辑sudo命令的配置         编辑第98行         ## Allow root to run any commands anywhere             root    ALL=(ALL)                               ALL             用户名  登录的主机=（以什么样的身份运行）  可以执行什么命令         如果想让huadian用户也居于root相关权限。。             huadian  ALL=(root)  NOPASSWD:service iptables status             huadian  ALL=(root)  NOPASSWD:service iptables start         推荐用法                 huadian  ALL=(root)  NOPASSWD:ALL                      使用权限：sudo     service iptables status  ----（检查防火墙状态）

### 网络管理：ping、ifconfig     服务管理命令：         service:必须掌握             格式：                 service s_name start|stop|status|restart             linux系统所有自带服务名称：/etc/init.d/                 常用：                     关闭防火墙服务                     service iptables stop                     重启网络服务：                     service network restart                     mysql数据库服务的名称：                         mysql版本低于5.5  mysqld                         mysql版本高于5.5  mysql                      chkconfig:设置是否开机启动           :必须掌握             判定是否开机启动                 chkconfig iptables --list                 2.3.4.5是on表示开机启动             设置                 chkconfig iptables on|off                          进程管理：ps         ps:查当前进程             查看java的进程             ps -ef | grep java         jps:==(ps -ef | grep java) 只有在linux中安装了JDK才能用         kill :杀死某个进程             kill -9 pid                  端口管理         nststat:查看端口开放情况             -a:表示列举所有的连接、服务器监听             -t:列出所有tcp协议的服务             -u:列出所有udp协议的服务             -n:使用端口号来显示             -l:列出所有的监听             -p:列出所有服务的进程id（pid）             常用：netstat -atunlp              redhat的selinux安全机制         关闭selinux安全机制             vim /etc/selinux/config                 SELINUX=disabled             重启机器生效

 

# vim/vi命令看下一篇帖子

# vim/vi命令看下一篇帖子

# vim/vi命令看下一篇帖子

有 0 个人打赏

文章最后发布于: 2018-08-29 09:45:41

[微软人工智能教育与学习共建社区](https://github.com/microsoft/ai-edu?utm_source=csdnblog)

[我们将在此提供人工智能应用开发的真实案例，与广大师生、开发者一起学习、一起贡献！](https://github.com/microsoft/ai-edu?utm_source=csdnblog)



[*Linux**常用命令*（面试题）06-13 阅读数 1万+](https://blog.csdn.net/qq_40910541/article/details/80686362)

[Linux常用命令因为热爱，所以拼搏。–RuiDer常用指令ls　　显示文件或目录-l列出文件详细信息l(list)-a列出当前目录下所有文件及目录，包括隐藏的a(all)mkdir创建目录-p创建目... ](https://blog.csdn.net/qq_40910541/article/details/80686362)博文 [来自： RuiDer的博客](https://blog.csdn.net/qq_40910541)

[*linux*命令*详解*，史上最全！！！01-18 阅读数 1万+](https://blog.csdn.net/bearcatfly/article/details/54602359)

[小编整理一些linux经常用到的命令，由于命令太多，分几期更新，如此辛苦，快快送赞吧！哈哈哈哈... ](https://blog.csdn.net/bearcatfly/article/details/54602359)博文 [来自： bearcatfly的博客](https://blog.csdn.net/bearcatfly)

[*Linux*20个*常用命令*11-24 阅读数 10万+](https://blog.csdn.net/xufei512/article/details/53321980)

[玩过Linux的人都会知道，Linux中的命令的确是非常多，但是玩过Linux的人也从来不会因为Linux的命令如此之多而烦恼，因为我们只需要掌握我们最常用的命令就可以了。当然你也可以在使用时去找一下... ](https://blog.csdn.net/xufei512/article/details/53321980)博文 [来自： 奋斗码农的博客](https://blog.csdn.net/xufei512)

[面试中*linux*常见的20个命令01-19 阅读数 2万+](https://blog.csdn.net/weixin_38429587/article/details/79110588)

[1.查找文件find/-namefilename.txt根据名称查找/目录下的filename.txt文件。2.查看一个程序是否运行ps–ef|greptomcat查看所有有关tomcat的进程3.终... ](https://blog.csdn.net/weixin_38429587/article/details/79110588)博文 [来自： 小明的博客](https://blog.csdn.net/weixin_38429587)
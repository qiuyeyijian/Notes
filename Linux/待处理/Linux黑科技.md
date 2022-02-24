## Linux 常用命令总结

### 1. kali离线安装软件

**下载 Google Chrome**

首先，使用 wget 命令来下载最新版本的 Google Chrome 的 debian 安装包。

```
# wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

**安装 Google Chrome**

` 在 Kali Linux 安装 Google Chrome 最容易的方法就是使用 gdebi ，它会自动帮你下载所有的依赖包。 `

```
# gdebi google-chrome-stable_current_amd64.deb
```

**启动 Google Chrome**

开启一个终端，执行 google-chrome 命令来启动 Google Chrome 浏览器。

```
$ google-chrome
```

**非法指令**

当以 root 用户特权来运行 google-chrome 命令是，会出现非法指令 错误信息。因为通常情况下，Kali Linux 默认情况下的默认用户是 root 用户，我们需要创建一个虚的非特权用户，比如 linuxconfig ，然后使用这个用户来启动 Google Chrome 浏览器。如下：

```
# useradd -m -d /home/linuxconfig linuxconfig
# su linuxconfig -c google-chrome
```

**libappindicator1 包未安装**

```
dpkg: dependency problems prevent configuration of google-chrome-stable:
 google-chrome-stable depends on libappindicator1; however:
  Package libappindicator1 is not installed.
```

使用 gdebi 命令来安装 Google Chrome 的 debian 包可以解决依赖问题。参阅上文。

``` 
其他离线安装命令： sudo dpkg -i 软件名
```

### 2. 火狐中文安装包

``` 
https://addons.mozilla.org/zh-CN/firefox/addon/chinese-simplified-zh-cn-la/
```

### 3. kali ，CentOS，Ubantu镜像源

#### 3.1 Kali

``` 
 vim /etc/apt/sources.list 
```

``` 
推荐配置：

#kali官方源
deb http://http.kali.org/kali kali-rolling main non-free contrib
#中科大的源
deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb http://mirrors.ustc.edu.cn/kali kali-rolling main contrib non-free
deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main contrib non-free
deb http://mirrors.ustc.edu.cn/kali-security kali-current/updates main contrib non-free
deb-src http://mirrors.ustc.edu.cn/kali-security kali-current/updates main contrib non-free
#阿里云源
deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
deb http://mirrors.aliyun.com/kali-security/ kali-rolling main contrib non-free
deb-src http://mirrors.aliyun.com/kali-security/ kali-rolling main contrib non-free
```

> 3、更新源
>
> > apt-get update
>
> 4、更新软件包
>
> > apt-get upgrade
>
> 5、更新系统
>
> > apt-get dist-upgrade
>
> 6、自动删除不需要的软件包
>
> > apt-get autoremove
>
> 7、自动清除软件包
>
> > apt-get autoclean
>
> 8、重启系统
>
> > reboot



#### 3.2 CentOS



### 4. Linux cp命令

Linux cp命令主要用于复制文件或目录。

#### 语法

```
cp [options] source dest
```

或

```
cp [options] source... directory
```

**参数说明**：

- -a：此选项通常在复制目录时使用，它保留链接、文件属性，并复制目录下的所有内容。其作用等于dpR参数组合。
- -d：复制时保留链接。这里所说的链接相当于Windows系统中的快捷方式。
- -f：覆盖已经存在的目标文件而不给出提示。
- -i：与-f选项相反，在覆盖目标文件之前给出提示，要求用户确认是否覆盖，回答"y"时目标文件将被覆盖。
- -p：除复制文件的内容外，还把修改时间和访问权限也复制到新文件中。
- -r：若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。
- -l：不复制文件，只是生成链接文件。

####  实例

使用指令"cp"将当前目录"test/"下的所有文件复制到新目录"newtest"下，输入如下命令：

```
$ cp –r test/ newtest          
```

注意：用户使用该指令复制目录时，必须使用参数"-r"或者"-R"。

### 5. SSH服务

安装ssh

```bash
sudo apt-get install ssh
```

设置开机自启动

```bash
sudo systemctl enable ssh
```

### 6. CentOS查看系统版本

1、查看版本文件名称

```
ll /etc/*centos*
```

2、显示系统版本号

```
cat /etc/centos-release
```

3. 完整信息

```
uname -a
```



### 7. Sed命令 - 修改文本内容

```bash
sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.repo
```



```
sed -i 's/要替换的字符串/替换后的字符串/g' yum.log
或者 sed -i 's#hhh#HHHH#g' h.txt
              s==search  查找并替换
              g==global  全部替换
              -i: implace
              
//如果要替换的字符串里有‘/'等可以用“\"转义
sed -i 's/http:\/\/qiuyeyijian.com/https:\/\/qiuyeyijian.com/g' /root/a.txt
```

### 9. Ubuntu 开启指定端口

一般情况下，ubuntu安装好的时候，iptables会被安装上，如果没有的话先安装

```
sudo apt-get install iptables
```

添加开放端口

```
sudo iptables -I INPUT -p tcp --dport [端口号] -j ACCEPT
```

```
sudo iptables -I INPUT -p tcp --dport 80 -j ACCEPT
```

临时保存配置，重启后失效

```
sudo iptables-save
```



安装 iptables-persistent工具，持久化开放端口配置

```
sudo apt-get install iptables-persistent

sudo netfilter-persistent save

sudo netfilter-persistent reload
```

### 10. cat 命令

```bash
cat > hello.txt				//新建文件
cat hello.txt hi.txt > wei.txt		//文件合并
cat hello.txt >> hi.txt				//将hello.txt的内容追加到hi.txt的末尾
cat >1.txt 							//进入编辑状态，ctr + d 退出


```

### 11. 磁盘问题

* 查看磁盘使用情况（已经挂载的磁盘）

```
df -h 
```

* 查看所有磁盘，包括 df -h 看不到的为挂载的磁盘 

```
fdisk -lu
```

* 使用 LVM 调整磁盘分区

```bash
# ext2/ext3/ext4文件系统的调整命令是resize2fs（增大和减小都支持）

lvextend -L 120G /dev/mapper/ubuntu--vg-ubuntu--lv     //增大至120G
lvextend -L +20G /dev/mapper/ubuntu--vg-ubuntu--lv     //增加20G
lvreduce -L 50G /dev/mapper/ubuntu--vg-ubuntu--lv      //减小至50G
lvreduce -L -8G /dev/mapper/ubuntu--vg-ubuntu--lv      //减小8G
lvresize -L  30G /dev/mapper/ubuntu--vg-ubuntu--lv     //调整为30G
```

执行调整

```
resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv            //执行调整
```





### 12. Linux软链接(快捷方式)

```
ln -s 源文件路径 链接文件路径
```



实例 `ln -s /home/gamestat /gamestat`



linux下的软链接类似于windows下的快捷方式

`ln -s a b` 中的 a 就是源文件，b是链接文件名,其作用是当进入b目录，实际上是链接进入了a目录

如上面的示例，当我们执行命令 cd /gamestat/的时候 实际上是进入了 /home/gamestat/

值得注意的是执行命令的时候,应该是a目录已经建立，目录b没有建立。我最开始操作的是也把b目录给建立了，结果就不对了

删除软链接：

```
rm -rf b				
```

### 13. 查看程序路径

```
which java			//查看java 程序执行路径
```



### 14. 返回上一次所在目录

```bash
cd -
```



### 15. chmod 命令



#### 语法

```
chmod [-cfvR] [--help] [--version] mode file...
```



- u 表示该文件的拥有者，g 表示与该文件的拥有者属于同一个群体(group)者，o 表示其他以外的人，a 表示这三者皆是。
- \+ 表示增加权限、- 表示取消权限、= 表示唯一设定权限。
- r 表示可读取，w 表示可写入，x 表示可执行，X 表示只有当该文件是个子目录或者该文件已经被设定过为可执行。

其他参数说明：

- -c : 若该文件权限确实已经更改，才显示其更改动作
- -f : 若该文件权限无法被更改也不要显示错误讯息
- -v : 显示权限变更的详细资料
- -R : 对目前目录下的所有文件与子目录进行相同的权限变更(即以递回的方式逐个变更)
- --help : 显示辅助说明
- --version : 显示版本



用数字表示权限，语法为：

```
chmod abc file
```

其中a,b,c各为一个数字，分别表示User、Group、及Other的权限。

#### r=4，w=2，x=1

- 若要rwx属性则4+2+1=7；
- 若要rw-属性则4+2=6；
- 若要r-x属性则4+1=5。





### 16. 查看和杀死进程

#### 查看进程

```bash
ps -ef |grep redis
```

* ps:将某个进程显示出来
  -A 　显示所有程序。 
  -e 　此参数的效果和指定"A"参数相同。
  -f 　显示[UID](https://www.baidu.com/s?wd=UID&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1dBm1uBmvF9uADYuAfzPHTY0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EPW6sPjmLPWR1njDsrjRsn1Tz),PPIP,C与S[TIME](https://www.baidu.com/s?wd=TIME&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1dBm1uBmvF9uADYuAfzPHTY0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EPW6sPjmLPWR1njDsrjRsn1Tz)栏位。 
  grep命令是查找
  中间的|是管道命令 是指ps命令与grep同时执行

这条命令的意思是显示有关redis有关的进程

* kill 参数 进程号

```bash
  kill -9 4394
```

kill就是给某个进程id发送了一个信号。默认发送的信号是SIGTERM，而kill -9发送的信号是SIGKILL，即exit。exit信号不会被系统阻塞，所以kill -9能顺利杀掉进程。当然你也可以使用kill发送其他信号给进程。



### 17. Linux系统下永久修改自己喜欢的系统主机名



```bash
hostnamectl set-hostname 主机名
```



下面是修改终端显示的名字

> 三行代码实现想要效果

* 打开root目录下的隐藏文件` .bashrc` 

``` 
vi ~/.bashrc
```

- 添加如下代码

```bash
PS1="[\u@localhost \w]\$" #修改中间的localhost为自己想要修改的主机名
```

- 立即生效 

```bash
source ~/.bashrc
```



### 18. Linux命令中Ctrl+z、Ctrl+c和Ctrl+d

Ctrl+c是强制中断程序的执行。

Ctrl+z的是将任务中断,但是此任务并没有结束,他仍然在进程中他只是维持挂起的状态

Ctrl+d 不是发送信号，而是表示一个特殊的二进制值，表示 EOF。



### 19. 查看所有系统内核

```
sudo awk -F\' '$1=="menuentry " {print i++ " : " $2}' /etc/grub2.cfg
```


# [CentOS7 无法使用yum命令，无法更新解决方法](https://www.cnblogs.com/crowsong/p/9371216.html)



- **前言**
- **设置网卡开机自动启动**
- **设置国内dns服务器系统**
- **修改CentOS-Base.repo中的地址**
- **所参考的文章地址**

------

**前言**

刚安装完的CentOS7的系统，发现无法使用yum命令进行更新，在更新的时候会出现下面这种内容，为此问题有以下这些解决方案可以尝试。

```
 One of the configured repositories failed (Unknown),
 and yum doesn't have enough cached data to continue. At this point the only
 safe thing yum can do is fail. There are a few ways to work "fix" this:

     1. Contact the upstream for the repository and get them to fix the problem.

     2. Reconfigure the baseurl/etc. for the repository, to point to a working
        upstream. This is most often useful if you are using a newer
        distribution release than is supported by the repository (and the
        packages for the previous distribution release still work).

     3. Disable the repository, so yum won't use it by default. Yum will then
        just ignore the repository until you permanently enable it again or use
        --enablerepo for temporary usage:

            yum-config-manager --disable <repoid>

     4. Configure the failing repository to be skipped, if it is unavailable.
        Note that yum will try to contact the repo. when it runs most commands,
        so will have to try and fail each time (and thus. yum will be be much
        slower). If it is a very temporary problem though, this is often a nice
        compromise:

            yum-config-manager --save --setopt=<repoid>.skip_if_unavailable=true
```







24





 





1

```
 One of the configured repositories failed (Unknown),
```

2

```
 and yum doesn't have enough cached data to continue. At this point the only
```

3

```
 safe thing yum can do is fail. There are a few ways to work "fix" this:
```

4

```

```

5

```
     1. Contact the upstream for the repository and get them to fix the problem.
```

6

```

```

7

```
     2. Reconfigure the baseurl/etc. for the repository, to point to a working
```

8

```
        upstream. This is most often useful if you are using a newer
```

9

```
        distribution release than is supported by the repository (and the
```

10

```
        packages for the previous distribution release still work).
```

11

```

```

12

```
     3. Disable the repository, so yum won't use it by default. Yum will then
```

13

```
        just ignore the repository until you permanently enable it again or use
```

14

```
        --enablerepo for temporary usage:
```

15

```

```

16

```
            yum-config-manager --disable <repoid>
```

17

```

```

18

```
     4. Configure the failing repository to be skipped, if it is unavailable.
```

19

```
        Note that yum will try to contact the repo. when it runs most commands,
```

20

```
        so will have to try and fail each time (and thus. yum will be be much
```

21

```
        slower). If it is a very temporary problem though, this is often a nice
```

22

```
        compromise:
```

23

```

```

24

```
            yum-config-manager --save --setopt=<repoid>.skip_if_unavailable=true
```





![img](https://images2018.cnblogs.com/blog/1100571/201807/1100571-20180726131831183-1935177873.png)





------

**设置网卡开机自动启动**

针对这个问题首先要确认网卡是否已经启动了，CentOS7最开始安装完的时候网卡可能会是关闭的，需要自己自行开启。

确保自己使用的是root账号，若不是，请自行更换。

1、进入/etc/sysconfig/network-scripts 目录。即输入命令 "cd /etc/sysconfig/network-scripts" ，使用命令 "ls -a" 可以查看该目录下的所有文件。

![img](https://images2018.cnblogs.com/blog/1100571/201807/1100571-20180726131834139-133957286.png)

2、修改ifcfg-ens33的网卡配置文件（CentOS7修改了网卡命名规则，不再是eth0了，而是ifcfg-eno+数字）。输入命令 "vi ifcfg-ens33" 进入vi编辑器，按下"i"或者"insert"键进入编辑模式。

![img](https://images2018.cnblogs.com/blog/1100571/201807/1100571-20180726131836345-329282733.png)

3、将 "ONBOOT" 的值修改为 "yes" ，之后按esc退出编辑模式，输入 ":wq" 保存退出

![img](https://images2018.cnblogs.com/blog/1100571/201807/1100571-20180726131840245-1145779365.png)

4、重启系统或者重启网卡，输入命令 "reboot" 或 "service network restart"。



------

**设置国内dns服务器**

若已经开启了网卡还是存在该问题可以尝试配置下国内的dns。

1、输入命令 "vi /etc/resolv.conf" 

2、添加 "nameserver 114.114.114.114" 

![img](https://images2018.cnblogs.com/blog/1100571/201807/1100571-20180726131845182-1313682202.png)

3、保存后，重启系统或者重启网卡，输入命令 "reboot" 或 "service network restart"。



------

**修改CentOS-Base.repo中的地址**

若上述方法还是无效可以尝试修改CentOS-Base.repo中的地址

1、进入 "/etc/yum.repos.d" 。

2、编辑 "vi CentOS-Base.repo" 。

3、将所有的 "mirrorlist" 注释掉，将所有的 "baseurl" 取消注释。

![img](https://images2018.cnblogs.com/blog/1100571/201807/1100571-20180726131847194-2089409851.png)

4、保存后，重启系统，输入命令 "reboot" 。
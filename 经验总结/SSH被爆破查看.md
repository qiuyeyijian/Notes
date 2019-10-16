### 查看SSH爆破记录及防范

作者 [SMG](http://www.howru.cc/articles/author/exadmin) 在[小小的技巧](http://www.howru.cc/articles/category/tricks) 标签 [Linux](http://www.howru.cc/articles/tag/linux)

> 新入手了一台HostUS的VPS，刚重做好系统，发现每次用Putty登录都提示距离上次成功登录有十几次失败的登录。什么鬼啊！难道有人尝试登录我的VPS？



马上用lastb命令查看一下登录失败的日志记录。一堆记录，全部都是尝试用root登录。再用wc数一下数量。word天啊！不到一个小时就有122条记录。显然有人在暴力破解登录密码了。

```
$ lastb | grep root | wc -l
122
```

手上两台BandwagonHost的VPS从来没有遇到过这样的事。想到HostUS建好的系统使用默认的SSH端口22，而BandwagonHost是随机分配SSH端口，估计这就是被人盯上的主要原因。于是马上去改SSH端口。一般都是在`/etc/ssh/sshd_config`里面改SSH的监听端口，不过HostUS的控制面板也提供了改SSH端口的功能。改好重启后，世界马上就安静了。

最后看一下是哪些IP爆破我的VPS，顺手在iptables里面加入拦截的名单。

```
$ lastb | grep root | awk '{print $3}' | sort | uniq
58.218.204.181
61.234.247.236
```

竟然都是国内的IP，并且应该是一些数据中心的IP。估计都是国内有人通过服务器不停地找肉鸡。

```
$ iptables -I INPUT -s 58.218.204.181 -j DROP
$ iptables -I INPUT -s 61.234.247.236 -j DROP
$ iptables --list
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
DROP       all  --  61.234.247.236       anywhere
DROP       all  --  58.218.204.181       anywhere

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```

现在过了一天，暂时再也没有新的爆破记录了。如果还有的话，就要关闭root的密码登录改用密钥登录了。
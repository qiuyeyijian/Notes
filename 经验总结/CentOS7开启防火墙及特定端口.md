# CentOS7开启防火墙及特定端口

2018年07月25日 19:21:00  更多



 版权声明：欢迎评论和「带出处」转载 https://blog.csdn.net/zll_0405/article/details/81208606



以前为了方便，把防火墙都关闭了，因为现在项目都比较重要，害怕受到攻击，所以为了安全性，现在需要将防火墙开启，接下来介绍一下步骤。
1，	首先查看防火墙状态：

```
firewall-cmd --state
```

下图所示为关闭防火墙，接下来需要开启
![这里写图片描述](https://img-blog.csdn.net/20180725191617581?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3psbF8wNDA1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
2，	开启防火墙，
启动firewall：

```
systemctl start firewalld.service
```

设置开机自启：

```
systemctl enable firewalld.service
```

3，	重启防火墙：

```
systemctl restart firewalld.service
```

4，	检查防火墙状态是否打开：

```
firewall-cmd --state
```

如图显示已经打开

![这里写图片描述](https://img-blog.csdn.net/20180725192024865?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3psbF8wNDA1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
5，	查看防火墙设置开机自启是否成功：

```
systemctl is-enabled firewalld.service;echo $?
```

如图所示，即为成功
![这里写图片描述](https://img-blog.csdn.net/20180725191737997?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3psbF8wNDA1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
以上就是开启防火墙相关步骤



在开启防火墙之后，我们有些服务就会访问不到，是因为服务的相关端口没有打开。
在此以打开80端口为例
命令：

```
开端口命令：firewall-cmd --zone=public --add-port=80/tcp --permanent
重启防火墙：systemctl restart firewalld.service
 
命令含义：
 
--zone #作用域
 
--add-port=80/tcp  #添加端口，格式为：端口/通讯协议
 
--permanent   #永久生效，没有此参数重启后失效
12345678910
```

如图，可看到开启端口成功：
![这里写图片描述](https://img-blog.csdn.net/20180726195427665?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3psbF8wNDA1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
如果不放心，可以通过命令：

```
netstat -ntlp
或：firewall-cmd --list-ports
```

查看开启的所有端口，具体如图
![这里写图片描述](https://img-blog.csdn.net/20180726195754549?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3psbF8wNDA1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
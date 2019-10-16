# Kali Linux 用户利用SSH远程登录





> ### Kali Linux 用户利用SSH远程登录

一、修改SSH参数

修改sshd_config文件

vi /etc/ssh/sshd_config

将#PasswordAuthentication no的注释去掉,并且将NO改为YES //kali中默认是yes



![img](https://ask.qcloudimg.com/http-save/developer-news/09s6ch9thi.jpeg?imageView2/2/w/1620)

将PermitRootLogin without-password改为

PermitRootLogin yes



![img](https://ask.qcloudimg.com/http-save/developer-news/3vw9lpc7dq.jpeg?imageView2/2/w/1620)

二、启动SSH服务

/etc/init.d/ssh start

查看SSH服务状态

/etc/init.d/ssh status

输入命令“service ssh restart”

重启ssh服务

输入命令“ifconfig”查看IP



![img](https://ask.qcloudimg.com/http-save/developer-news/0is3tbmge0.jpeg?imageView2/2/w/1620)

三、使用SSH登录工具(Putty/SecureCRT/XShell)登录kali
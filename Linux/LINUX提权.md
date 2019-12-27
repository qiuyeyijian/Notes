## Linux提权
1.信息收集

2.脏牛漏洞提权

3.内核漏洞exp提权

4.SUID提权

0x00 基础信息收集
（1）：内核，操作系统和设备信息

```
uname -a    打印所有可用的系统信息
uname -r    内核版本
uname -n    系统主机名。
uname -m    查看系统内核架构（64位/32位）
hostname    系统主机名
cat /proc/version    内核信息
cat /etc/*-release   分发信息
cat /etc/issue       分发信息
cat /proc/cpuinfo    CPU信息
```


（2）用户和群组

```
cat /etc/passwd     列出系统上的所有用户
cat /etc/group      列出系统上的所有组
grep -v -E "^#" /etc/passwd | awk -F: '$3 == 0 { print $1}'      列出所有的超级用户账户
whoami              查看当前用户
w                   谁目前已登录，他们正在做什么
last                最后登录用户的列表
lastlog             所有用户上次登录的信息
lastlog –u %username%  有关指定用户上次登录的信息
lastlog |grep -v "Never"  以前登录用户的完
```


（3）用户和权限信息：

```
whoami        当前用户名
id            当前用户信息
cat /etc/sudoers  谁被允许以root身份执行
sudo -l       当前用户可以以root身份执行操作
```

（4）环境信息
 

```
env        显示环境变量
set        现实环境变量
echo %PATH 路径信息
history    显示当前用户的历史命令记录
pwd        输出工作目录
cat /etc/profile   显示默认系统变量
cat /etc/shells    显示可用的shell
```





linux操作有很多命令，但是线下赛的防护工作中常用的也就那么一些，我们平时可以留意并总结起来，便于我们比赛使用。



```
ssh <-p 端口> 用户名@IP　　
scp 文件路径  用户名@IP:存放路径　　　　
tar -zcvf web.tar.gz /var/www/html/　　
w 　　　　
pkill -kill -t <用户tty>　　 　　
ps aux | grep pid或者进程名　　　　
#查看已建立的网络连接及进程
netstat -antulp | grep EST
#查看指定端口被哪个进程占用
lsof -i:端口号 或者 netstat -tunlp|grep 端口号
#结束进程命令
kill PID
killall <进程名>　　
kill - <PID>　　
#封杀某个IP或者ip段，如：.　　
iptables -I INPUT -s . -j DROP
iptables -I INPUT -s ./ -j DROP
#禁止从某个主机ssh远程访问登陆到本机，如123..　　
iptable -t filter -A INPUT -s . -p tcp --dport  -j DROP　　
#备份mysql数据库
mysqldump -u 用户名 -p 密码 数据库名 > back.sql　　　　
mysqldump --all-databases > bak.sql　　　　　　
#还原mysql数据库
mysql -u 用户名 -p 密码 数据库名 < bak.sql　　
find / *.php -perm  　　 　　
awk -F:  /etc/passwd　　　　
crontab -l　　　　
#检测所有的tcp连接数量及状态
netstat -ant|awk  |grep |sed -e  -e |sort|uniq -c|sort -rn
#查看页面访问排名前十的IP
cat /var/log/apache2/access.log | cut -f1 -d   | sort | uniq -c | sort -k  -r | head -　　
#查看页面访问排名前十的URL
cat /var/log/apache2/access.log | cut -f4 -d   | sort | uniq -c | sort -k  -r | head -
```


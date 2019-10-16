原

# linux服务器历险之linux安全设置

2007年09月26日 00:28:00 [chinalinuxzend](https://me.csdn.net/chinalinuxzend) 阅读数 1641



1，删除所有的特殊账户 
你应该删除所有不用的缺省用户和组账户（比如lp,　sync,　shutdown,　halt,　news,　uucp,　operator,　games,　gopher等）。 
删除用户： 
[root@kapil　/]#　userdel　LP　 
删除组： 
[root@kapil　/]#　groupdel　LP　 
删除系统臃肿多余的账号：
userdel adm
userdel lp
userdel sync
userdel shutdown
userdel halt
userdel news
userdel uucp
userdel operator
userdel games
userdel gopher
userdel ftp 如果你不允许匿名FTP，就删掉这个用户帐号
groupdel adm
groupdel lp
groupdel news
groupdel uucp
groupdel games
groupdel dip
groupdel pppusers

第 二步是删除/etc/passwd文件中许多缺省的系统帐号。Linux提供这些帐号主要是用于许多其实极少需要的系统操作。如果不需要这些帐号，删除它 们。帐号越多，系统被入侵的可能性就越大。例如"news"帐号，如果不运行nntp新闻组服务器，就不需要该帐号（注意要更新 /etc/cron.hourly文件，因为脚本中涉及到了"news"用户）。另外，一定要删除"ftp"帐号，因为该帐号仅用于匿名FTP访问。 

我 们还要修改/etc/ftpusers文件。任何被列入该文件的帐号将不能ftp到本系统。通常用于限制系统帐号，例如root和bin等，禁止这些帐号 的FTP会话。缺省时Linux已创建了该文件。一定要确保root根用户被包含在该文件中，以禁止root与系统的ftp会话。检查并确认需要FTP到 该防火墙的所有帐号**不**在/etc/ftpusers文件中。 

另外，确保根用户root不能telnet到系统。这强迫用户用 其普通帐号登录到系统，然后再su成为root。/etc/securetty文件列出了root所能连接的tty终端。将tty1、tty2等列入该文 件中，使root用户只能从本地登录到系统中。ttyp1、ttyp2等是pseudo（虚拟）终端，它们允许root远程telnet到系统中。 

最 后，创建/etc/issue文件。该ASCII文本文件用于在所有telnet登录时显示的信息。当试图登录到系统中时，该文件中的警告信息将被显示。 在Linux系统中要修改/etc/rc.d/init.d/S99local脚本文件，以生成固定的/etc/issue文件。 

因为缺省时Linux在每次启动时都生成新的/etc/issue文件。 


2，选择正确的密码 
在选择正确密码之前还应作以下修改： 
修改密码长度：在你安装linux时默认的密码长度是5个字节。但这并不够，要把它设为8。修改最短密码长度需要编辑login.defs文件（vi　/etc/login.defs），把下面这行 
PASS_MIN_LEN　　　　5　 
改为 
PASS_MIN_LEN　　　　8 
login.defs文件是login程序的配置文件。

3、root账户自动注销 
在unix 系统中root账户是具有最高特权的。如果系统管理员在离开系统之前忘记注销root账户，系统会自动注销。通过修改账户中"TMOUT"参数，可以实现 此功能。TMOUT按秒计算。编辑你的profile文件（vi　/etc/profile）,在"HISTFILESIZE="后面加入下面这行： 
TMOUT=3600 
3600，表示60*60=3600秒，也就是1小时。这样，如果系统中登陆的用户在一个小时内都没有动作，那么系统会自动注销这个账户。你可以在个别用户的".bashrc"文件中添加该值，以便系统对该用户实行特殊的自动注销时间。 
改变这项设置后，必须先注销用户，再用该用户登陆才能激活这个功能。

4、取消普通用户的控制台访问权限 
你应该取消普通用户的控制台访问权限，比如shutdown、reboot、halt等命令。 
[root@kapil　/]#　rm　-f　/etc/security/console.apps/　 
是你要注销的程序名。

5、禁止系统信息暴露 
当有人远程登陆时，禁止显示系统欢迎信息。你可以通过修改"/etc/inetd.conf"文件来达到这个目的。 
把/etc/inetd.conf文件下面这行： 
telnet　　stream　　tcp　　　　　nowait　root　　　　/usr/sbin/tcpd　　in.telnetd 
修改为: 
telnet　　stream　　tcp　　　　　nowait　　root　　　　/usr/sbin/tcpd　　in.telnetd　-h 
在最后加"-h"可以使当有人登陆时只显示一个login:提示，而不显示系统欢迎信息。 

6、修改"/etc/host.conf"文件 
"/etc/host.conf"说明了如何解析地址。编辑"/etc/host.conf"文件（vi　/etc/host.conf），加入下面这行： 

Lookup　names　via　DNS　first　then　fall　back　to　/etc/hosts.　 

order　bind,hosts　 

We　have　machines　with　multiple　IP　addresses.　 

multi　on　 

Check　for　IP　address　spoofing.　 

nospoof　on　 
第一项设置首先通过DNS解析IP地址，然后通过hosts文件解析。第二项设置检测是否"/etc/hosts"文件中的主机是否拥有多个IP地址（比如有多个以太口网卡）。第三项设置说明要注意对本机未经许可的电子欺骗。

7、使"/etc/services"文件免疫 
使"/etc/services"文件免疫，防止未经许可的删除或添加服务： 
[root@kapil　/]#　chattr　+i　/etc/services 

8、不允许从不同的控制台进行root登陆 
"/etc/securetty"文件允许你定义root用户可以从那个TTY设备登陆。你可以编辑"/etc/securetty"文件，再不需要登陆的TTY设备前添加"#"标志，来禁止从该TTY设备进行root登陆。

9、禁止任何人通过su命令改变为root用户 
su(Substitute　User替代用户)命令允许你成为系统中其他已存在的用户。如果你不希望任何人通过su命令改变为root用户或对某些用户限制使用su命令，你可以在su配置文件（在"/etc/pam.d/"目录下）的开头添加下面两行： 
编辑su文件(vi　/etc/pam.d/su)，在开头添加下面两行： 
auth　sufficient　/lib/security/pam_rootok.so　debug　 
auth　required　/lib/security/Pam_wheel.so　group=wheel　 
这表明只有"wheel"组的成员可以使用su命令成为root用户。你可以把用户添加到"wheel"组，以使它可以使用su命令成为root用户。 

10、Shell　logging 
Bash 　shell在"~/.bash_history"（"~/"表示用户目录）文件中保存了500条使用过的命令，这样可以使你输入使用过的长命令变得容 易。每个在系统中拥有账号的用户在他的目录下都有一个".bash_history"文件。bash　shell应该保存少量的命令，并且在每次用户注销 时都把这些历史命令删除。 
第一步： 
"/etc/profile"文件中的"HISTFILESIZE"和"HISTSIZE"行确 定所有用户的".bash_history"文件中可以保存的旧命令条数。强烈建议把把"/etc/profile"文件中的 "HISTFILESIZE"和"HISTSIZE"行的值设为一个较小的数，比如 30。编辑profile文件（vi　/etc/profile），把下面这行改为： 
HISTFILESIZE=30　 
HISTSIZE=30　 
这表示每个用户的".bash_history"文件只可以保存30条旧命令。 
第二步： 
网管还应该在"/etc/skel/.bash_logout"　文件中添加下面这行"rm　-f　　$HOME/.bash_history"　。这样，当用户每次注销时，".bash_history"文件都会被删除。 
编辑.bash_logout文件(vi　/etc/skel/.bash_logout)　，添加下面这行： 
rm　-f　$HOME/.bash_history 

11、禁止Control-Alt-Delete　键盘关闭命令 
在"/etc/inittab"　文件中注释掉下面这行（使用#）： 
ca::ctrlaltdel:/sbin/shutdown　-t3　-r　now　 
改为： 
#ca::ctrlaltdel:/sbin/shutdown　-t3　-r　now　 
为了使这项改动起作用，输入下面这个命令： 
[root@kapil　/]#　/sbin/init　q 

12、给"/etc/rc.d/init.d"　下script文件设置权限 
给执行或关闭启动时执行的程序的script文件设置权限。 
[root@kapil/]#　chmod　-R　700　/etc/rc.d/init.d/*　 
这表示只有root才允许读、写、执行该目录下的script文件。 

13、隐藏系统信息 
在缺省情况下，当你登陆到linux系统，它会告诉你该linux发行版的名称、版本、内核版本、服务器的名称。对于黑客来说这些信息足够它入侵你的系统了。你应该只给它显示一个"login:"提示符。 
第一步： 
编辑"/etc/rc.d/rc.local"　文件，在下面显示的这些行前加一个"#"，把输出信息的命令注释掉。 

This　will　overwrite　/etc/issue　at　every　boot.　　So,　make　any　changes　you　 

want　to　make　to　/etc/issue　here　or　you　will　lose　them　when　you　reboot.　 

#echo　""　>　/etc/issue　 
#echo　"$R"　>>　/etc/issue　 
#echo　"Kernel　$(uname　-r)　on　$a　$(uname　-m)"　>>　/etc/issue　 

#　 
#cp　-f　/etc/issue　/etc/issue.net　 
#echo　>>　/etc/issue 
第二步： 
删除"/etc"目录下的"isue.net"和"issue"文件： 
[root@kapil　/]#　rm　-f　/etc/issue　 
[root@kapil　/]#　rm　-f　/etc/issue.net

14、更改下列文件权限，使任何人没有更改账户权限：
chattr +i /etc/passwd
chattr +i /etc/shadow
chattr +i /etc/group
chattr +i /etc/gshadow
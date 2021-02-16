## CentOS 安装完成后必做的几件事

### 0x01. 更换阿里云镜像源

#### 1.备份

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

　　

#### 2.下载新的CentOS-Base.repo 到 /etc/yum.repos.d/ 

CentOS 7:
```
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
或者
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```


CentOS 8: 
```
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-8.repo
或者
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-8.repo

```

#### 3、之后先清除缓存再生成缓存

```
# 清除缓存
yum clean all
# 生存缓存
yum makecache
```



### 4. 一键安装脚本

```bash
#! /bin/bash

#========================================
#Autor: qiuyeyijian
#Date: 2020.4.2
#========================================

#获取当前系统的发行版本
VERSION=$(cat /etc/centos-release)

#提取当前系统的版本号
V_NUM=${VERSION:21:1}

BASE_REPO="/etc/yum.repos.d/CentOS-Base.repo"
ALI_REPO="http://mirrors.aliyun.com/repo/Centos-${V_NUM}.repo"

echo "备份当前软件源..."
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
echo "备份完成: /etc/yum.repos.d/CentOS-Base.repo.backup"

echo "下载阿里云镜像源..."
wget -O ${BASE_REPO} ${ALI_REPO} || curl -o ${BASE_REPO} ${ALI_REPO}

#补丁程序, 防止出现 Couldn't resolve host 'mirrors.cloud.aliyuncs.com' 信息
sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.repo

echo "清除缓存..."
yum clean all
echo "缓存清除成功,OK"

echo "生成缓存..."
yum makecache
echo "生成缓存成功, OK"

echo "更新软件..."
yum update -y
echo "软件更新完毕, OK"

echo "是否需要自动删除不需要的安装包"
read -p "Enter your choice: y/n(默认y):" CHOICE
case "${CHOICE}" in
	[yY] | [yY][eE][sS])
		yum autoremove -y
		;;
	[nN] | [nN][oO])
		echo "现在你可以飞快地安装软件了:)"
		;;
	*)
		yum autoremove -y
		;;
esac

```





### 0x02. 开启SSH服务

#### 1. 安装openssh-server

```
yum install -y openssl openssh-server
```

#### 2. 修改配置文件

```
vim /etc/ssh/sshd_config
```

修改以下几个配置，这几个配置不再一起，需要找一下
```
#Port 22						//默认ssh端口22，可以修改，但要将前面的“#”去掉

PermitRootLogin yes				//允许root 用户登录

RSSAuthentication yes

PubkeyAuthenticatiion yes	

```

#### 3. 注册ssh服务，开机启动

```
systemctl start sshd.service

systemctl enable sshd.service
```

#### 4. 创建秘钥对

```
cd /root
ssh-keygen		//接下来一路回车即可, 可以指定文件类型为rsa: ssh-keygen -t rsa 
```



ps:如果给其他用户开启ssh, 则需要注意.ssh权限问题

```
chmod 700 .ssh
chmod 600 .ssh/*
```

#### 5.保存秘钥

```
cd .ssh
touch authorized_keys
ls
```

会发现几个文件

```
id_rsa			//私钥
id_rsa.pub		//公钥
authorized_keys //存放公钥的文件，以后如果还有其他人需要连接，则将他们生成的公钥保存到里面
```

将公钥保存到 authorized_keys 中

```
cat id_rsa.pub >> authorized_keys
```

私钥请保存到自己的电脑中，比如Windows 可以保存到用户目录的 `.ssh` 目录下，然后删除

如果不方便下载，cat 命令可以查看内容，然后复制保存



```bash
ssh-keygen可用的参数选项有：
 
     -a trials
             在使用 -T 对 DH-GEX 候选素数进行安全筛选时需要执行的基本测试数量。
 
     -B      显示指定的公钥/私钥文件的 bubblebabble 摘要。
 
     -b bits
             指定密钥长度。对于RSA密钥，最小要求768位，默认是2048位。DSA密钥必须恰好是1024位(FIPS 186-2 标准的要求)。
 
     -C comment
             提供一个新注释
 
     -c      要求修改私钥和公钥文件中的注释。本选项只支持 RSA1 密钥。
             程序将提示输入私钥文件名、密语(如果存在)、新注释。
 
     -D reader
             下载存储在智能卡 reader 里的 RSA 公钥。
 
     -e      读取OpenSSH的私钥或公钥文件，并以 RFC 4716 SSH 公钥文件格式在 stdout 上显示出来。
             该选项能够为多种商业版本的 SSH 输出密钥。
 
     -F hostname
             在 known_hosts 文件中搜索指定的 hostname ，并列出所有的匹配项。
             这个选项主要用于查找散列过的主机名/ip地址，还可以和 -H 选项联用打印找到的公钥的散列值。
 
     -f filename
             指定密钥文件名。
 
     -G output_file
             为 DH-GEX 产生候选素数。这些素数必须在使用之前使用 -T 选项进行安全筛选。
 
     -g      在使用 -r 打印指纹资源记录的时候使用通用的 DNS 格式。
 
     -H      对 known_hosts 文件进行散列计算。这将把文件中的所有主机名/ip地址替换为相应的散列值。
             原来文件的内容将会添加一个".old"后缀后保存。这些散列值只能被 ssh 和 sshd 使用。
             这个选项不会修改已经经过散列的主机名/ip地址，因此可以在部分公钥已经散列过的文件上安全使用。
 
     -i      读取未加密的SSH-2兼容的私钥/公钥文件，然后在 stdout 显示OpenSSH兼容的私钥/公钥。
             该选项主要用于从多种商业版本的SSH中导入密钥。
 
     -l      显示公钥文件的指纹数据。它也支持 RSA1 的私钥。
             对于RSA和DSA密钥，将会寻找对应的公钥文件，然后显示其指纹数据。
 
     -M memory
             指定在生成 DH-GEXS 候选素数的时候最大内存用量(MB)。
 
     -N new_passphrase
             提供一个新的密语。
 
     -P passphrase
             提供(旧)密语。
 
     -p      要求改变某私钥文件的密语而不重建私钥。程序将提示输入私钥文件名、原来的密语、以及两次输入新密语。
 
     -q      安静模式。用于在 /etc/rc 中创建新密钥的时候。
 
     -R hostname
             从 known_hosts 文件中删除所有属于 hostname 的密钥。
             这个选项主要用于删除经过散列的主机(参见 -H 选项)的密钥。
 
     -r hostname
             打印名为 hostname 的公钥文件的 SSHFP 指纹资源记录。
 
     -S start
             指定在生成 DH-GEX 候选模数时的起始点(16进制)。
 
     -T output_file
             测试 Diffie-Hellman group exchange 候选素数(由 -G 选项生成)的安全性。
 
     -t type
             指定要创建的密钥类型。可以使用："rsa1"(SSH-1) "rsa"(SSH-2) "dsa"(SSH-2)
 
     -U reader
             把现存的RSA私钥上传到智能卡 reader
 
     -v      详细模式。ssh-keygen 将会输出处理过程的详细调试信息。常用于调试模数的产生过程。
             重复使用多个 -v 选项将会增加信息的详细程度(最大3次)。
 
     -W generator
             指定在为 DH-GEX 测试候选模数时想要使用的 generator
 
     -y      读取OpenSSH专有格式的公钥文件，并将OpenSSH公钥显示在 stdout 上。
```



### 8. CentOS 防火墙，开启指定端口

1，	首先查看防火墙状态：

```
firewall-cmd --state
```

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

5. 开启特定端口

```
firewall-cmd --zone=public --add-port=80/tcp --permanent
```

```
firewall-cmd --zone=public --remove-port=80/tcp --permanent
```

重启防火墙

```
systemctl restart firewalld.service
```

```
命令含义：
 
--zone #作用域
 
--add-port=80/tcp  #添加端口，格式为：端口/通讯协议
 
--permanent   #永久生效，没有此参数重启后失效
12345678910
```

6. 查看

```
firewall-cmd --list-ports
```


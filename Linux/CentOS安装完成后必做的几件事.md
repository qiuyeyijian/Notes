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
ssh-keygen		//接下来一路回车即可
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


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










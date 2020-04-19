## CentOS 7 升级 CentOS8

### 1. 安装epel源

```
yum -y install epel-release
```

### 2. 安装rpmconf和yum-utils

```
yum -y install rpmconf yum-utils
```

### 3. 解决配置问题

如果出现一些提示，请输入Y和回车继续，如果没提示继续第四步操作

```
rpmconf -a
```

清理一些不需要的包
```
package-cleanup --leaves
```

```
package-cleanup --orphans
```



### 4.安装dnf

```
yum -y install dnf
```

### 5. 移除yum和yum-metadata-parser

```
dnf -y remove yum yum-metadata-parser
```

```
rm -rf /etc/yum
```

### 7.安装Centos8的源和升级epel源

```
dnf -y upgrade
```

```
dnf -y upgrade http://mirrors.163.com/centos/8.0.1905/BaseOS/x86_64/os/Packages/centos-release-8.0-0.1905.0.9.el8.x86_64.rpm
```

```
dnf -y upgrade https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
```

```
dnf clean all
```



### 8.卸载centos7的内核

```
rpm -e --nodeps `rpm -qa|grep -i kernel`
```

```
rpm -e 'rpm -q kernel'
```

```
rpm -e --nodeps sysvinit-tools
```



### 9.升级到centos8，这一步一般会报错，如果没有报错请进行第10步操作

```
dnf -y --releasever=8 --allowerasing --setopt=deltarpm=false distro-sync
```



发现报错之后先卸载from package后面的包名

```
rpm -e --nodeps sysvinit-tools-2.88-14.dsf.el7.x86_64

rpm -e --nodeps python-inotify-0.9.4-4.el7.noarch

rpm -e --nodeps adwaita-qt5-1.0-1.el7.x86_64

rpm -e --nodeps pycairo-1.8.10-8.el7.x86_64
```



卸载完后再次执行升级

```
dnf -y --releasever=8 --allowerasing --setopt=deltarpm=false distro-sync
```

### 10.执行rpmconf，会出现如下界面，一直输入Y和回车即可

```
rpmconf -a
```



### 确保内核被安装了

```
rpm -e kernel-core
```

```
dnf -y install kernel-core
```



### 确保grup 升级了

```
ROOTDEV='ls /dev/*da|head -1';
echo “Detected root as $ROOTDEV…”
grub2-install $ROOTDEV
```



### 最小化安装

```
dnf -y groupupdate “Core” “Minimal Install”
```



### 11.重启机器

```
reboot
```

http://rpmfind.net/linux/fedora/linux/development/rawhide/Everything/x86_64/os/Packages/y/yum-4.2.21-1.fc33.noarch.rpm

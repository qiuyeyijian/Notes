## 安装好Ubuntu 18.04.4 后要做的几件事



### 1. 替换阿里云镜像源

```
mv /etc/apt/source.list /etc/apt/source.list.backup

wget -O /etc/apt/source.list https://yijian.me/repo/ubuntu/source.list

```



### 2. 安装谷歌浏览器

1. 官网下载最新版本
2. 

```
sudo dpkg -i google.deb

or

sudo gdebi google.deb				//没有gdebi 命令使用 sudo apt install gdebi-core 安装
```



### 3. 安装搜狗输入法Linux版

1. 官网下载最新版本

2. gdebi 安装

```
gdebi sougou.deb
```





### 4. 跳过 grub开机菜单

```
sudo vim /etc/default/grub
```

将  `GRUB_TIMEOUT=0` 修改为  `GRUB_TIMEOUT=-1`



更新grub

```
sudo update-grub
```


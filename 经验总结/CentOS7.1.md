

# [CentOS7.1修改默认yum源为国内的阿里云yum源](https://www.cnblogs.com/comexchan/p/5815869.html)



官方的yum源在国内访问效果不佳。

需要改为国内比较好的阿里云或者网易的yum源

修改方式：

```
下载wget
yum install wget -y
echo 备份当前的yum源
mv /etc/yum.repos.d /etc/yum.repos.d.backup4comex
echo 新建空的yum源设置目录
mkdir /etc/yum.repos.d
echo 下载阿里云的yum源配置
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```

然后重建缓存：

```
yum clean all
yum makecache
```
## Centos7下Nginx的安装和配置 



 

# 安装



## 安装编译工具及库文件

```
yum -y install make  gcc-c++ libtool  
```



## 安装 PCRE

PCRE 作用是让 Nginx 支持 Rewrite 功能。

```
$ cd /usr/local
$ wget http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz
$ tar zxvf pcre-8.35.tar.gz
$ cd pcre-8.35
$ ./configure
$ make && make install
$ pcre-config --version
```



## 安装zlib库

Nginx的gzip模块需要 zlib 库

```
$ cd /usr/local/ 
$ wget http://zlib.net/zlib-1.2.11.tar.gz
$ tar -zxvf zlib-1.2.11.tar.gz
$ cd zlib-1.2.11
$ ./configure
$ make
$ make install
```



## 安装ssl

Nginx的ssl 功能需要openssl库

```
$ cd /usr/local/
$ wget http://www.openssl.org/source/openssl-1.0.1j.tar.gz
$ tar -zxvf openssl-1.0.1j.tar.gz
$ ./config
$ make
$ make install
```



## 安装 Nginx

```
$ cd /usr/local/
$ wget http://nginx.org/download/nginx-1.8.0.tar.gz
$ tar -zxvf nginx-1.8.0.tar.gz
$ cd nginx-1.8.0  
$ ./configure --prefix=/usr/local/webserver/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/usr/local/pcre-8.35 --with-openssl=/usr/local/openssl-1.0.1j --with-zlib=/usr/local/zlib-1.2.11 
$ make
$ make install
#查看版本
$ /usr/local/webserver/nginx/sbin/nginx -v 
```



#  启动

```
$ /usr/local/nginx/sbin/nginx
```

检查是否启动成功：

打开浏览器访问此机器的 IP，如果浏览器出现 Welcome to nginx! 则表示 Nginx 已经安装并运行成功。

重新载入配置文件：
$ /usr/local/webserver/nginx/sbin/nginx –s reload

重启：
$ /usr/local/webserver/nginx/sbin/nginx –s reopen

停止：
$ /usr/local/webserver/nginx/sbin/nginx –s stop

测试配置文件是否正常：
$ /usr/local/webserver/nginx/sbin/nginx –t

强制关闭：
$ pkill nginx

nginx -h #帮助  

nginx -v #显示版本  

nginx -V #显示版本和配置信息  

nginx -t #测试配置  

nginx -q #测试配置时，只输出错误信息  

nginx -s stop #停止服务器  

nginx -s reload #重新加载配置  

 



# 新方法：使用yum安装Nginx

CentOS中设置yum repository , 创建含有以下内容的文件 `/etc/yum.repos.d/nginx.repo` :

[nginx]

name=nginx repo
baseurl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1
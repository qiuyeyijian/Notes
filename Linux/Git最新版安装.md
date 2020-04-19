##### 查看git版本，卸载旧版本（如果没有安装git请直接到下一步）



```csharp
git --version
yum remove git
```

##### 安装依赖软件



```undefined
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel asciidoc
yum install  gcc perl-ExtUtils-MakeMaker
```

##### 编译安装最新的git版本



```bash
cd /usr/local/src/
wget -O git.zip https://github.com/git/git/archive/master.zip
unzip git.zip
cd git-master
make prefix=/usr/local/git all
make prefix=/usr/local/git install
```

##### 添加到环境变量



```bash
echo "export PATH=$PATH:/usr/local/git/bin" >> ~/.bashrc
source ~/.bashrc
git --version
```

好了最新的git就装好了。



作者：KEEPINUP
链接：https://www.jianshu.com/p/450feb5e8f81
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
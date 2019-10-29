## Linux 常用命令总结

### 1. kali离线安装软件

**下载 Google Chrome**

首先，使用 wget 命令来下载最新版本的 Google Chrome 的 debian 安装包。

```
# wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

**安装 Google Chrome**

` 在 Kali Linux 安装 Google Chrome 最容易的方法就是使用 gdebi ，它会自动帮你下载所有的依赖包。 `

```
# gdebi google-chrome-stable_current_amd64.deb
```

**启动 Google Chrome**

开启一个终端，执行 google-chrome 命令来启动 Google Chrome 浏览器。

```
$ google-chrome
```

**非法指令**

当以 root 用户特权来运行 google-chrome 命令是，会出现非法指令 错误信息。因为通常情况下，Kali Linux 默认情况下的默认用户是 root 用户，我们需要创建一个虚的非特权用户，比如 linuxconfig ，然后使用这个用户来启动 Google Chrome 浏览器。如下：

```
# useradd -m -d /home/linuxconfig linuxconfig
# su linuxconfig -c google-chrome
```

**libappindicator1 包未安装**

```
dpkg: dependency problems prevent configuration of google-chrome-stable:
 google-chrome-stable depends on libappindicator1; however:
  Package libappindicator1 is not installed.
```

使用 gdebi 命令来安装 Google Chrome 的 debian 包可以解决依赖问题。参阅上文。

``` 
其他离线安装命令： sudo dpkg -i 软件名
```

### 2. 火狐中文安装包

``` 
https://addons.mozilla.org/zh-CN/firefox/addon/chinese-simplified-zh-cn-la/
```

### 3. kali 镜像源

``` 
 vim /etc/apt/sources.list 
```

``` 
推荐配置：

#kali官方源
deb http://http.kali.org/kali kali-rolling main non-free contrib
#中科大的源
deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb http://mirrors.ustc.edu.cn/kali kali-rolling main contrib non-free
deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main contrib non-free
deb http://mirrors.ustc.edu.cn/kali-security kali-current/updates main contrib non-free
deb-src http://mirrors.ustc.edu.cn/kali-security kali-current/updates main contrib non-free
#阿里云源
deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
deb http://mirrors.aliyun.com/kali-security/ kali-rolling main contrib non-free
deb-src http://mirrors.aliyun.com/kali-security/ kali-rolling main contrib non-free
```

> 3、更新源
>
> > apt-get update
>
> 4、更新软件包
>
> > apt-get upgrade
>
> 5、更新系统
>
> > apt-get dist-upgrade
>
> 6、自动删除不需要的软件包
>
> > apt-get autoremove
>
> 7、自动清除软件包
>
> > apt-get autoclean
>
> 8、重启系统
>
> > reboot

### Linux cp命令

Linux cp命令主要用于复制文件或目录。

#### 语法

```
cp [options] source dest
```

或

```
cp [options] source... directory
```

**参数说明**：

- -a：此选项通常在复制目录时使用，它保留链接、文件属性，并复制目录下的所有内容。其作用等于dpR参数组合。
- -d：复制时保留链接。这里所说的链接相当于Windows系统中的快捷方式。
- -f：覆盖已经存在的目标文件而不给出提示。
- -i：与-f选项相反，在覆盖目标文件之前给出提示，要求用户确认是否覆盖，回答"y"时目标文件将被覆盖。
- -p：除复制文件的内容外，还把修改时间和访问权限也复制到新文件中。
- -r：若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。
- -l：不复制文件，只是生成链接文件。

####  实例

使用指令"cp"将当前目录"test/"下的所有文件复制到新目录"newtest"下，输入如下命令：

```
$ cp –r test/ newtest          
```

注意：用户使用该指令复制目录时，必须使用参数"-r"或者"-R"。

### 
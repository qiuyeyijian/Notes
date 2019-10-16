# [ubuntu安装openssh-server 报依赖错误的解决过程](https://www.cnblogs.com/mliudong/p/4094519.html)



ubuntu自带的有openssh-client,所以可以通过

```
`ssh` `username@host`
```

来远程连接linux

可是要想通过ssh被连接,ubuntu系统需要有openssh-server,可以通过

```
`ps` `-e | ``grep` `ssh`
```

来查看,如果没有显示sshd则说明没有安装openssh-server

可通过

```
`sudo` `apt-get ``install` `openssh-server`
```

来安装openssh-server,如果顺利的话会安装成功,如果遇到

``` 
  $ ``sudo` `apt-get ``installopenssh-server
```



> ``正在读取软件包列表... 完成``正在分析软件包的依赖关系树       ``正在读取状态信息... 完成       ``有一些软件包无法被安装。如果您用的是 unstable 发行版，这也许是``因为系统无法达到您要求的状态造成的。该版本中可能会有一些您需要的软件``包尚未被创建或是它们已被从新到(Incoming)目录移出。``下列信息可能会对解决问题有所帮助：` `下列软件包有未满足的依赖关系：`` ``openssh-server : 依赖: openssh-client (= 1:6.6p1-2ubuntu1)``E: 无法修正错误，因为您要求某些软件包保持现状，就是它们破坏了软件包间的依赖关系。

这是因为,openssh-server是依赖于openssh-clien的,那ubuntu不是自带了openssh-client吗?原由是自带的openssh-clien与所要安装的openssh-server所依赖的版本不同,这里所依赖的版本是

```
`1:6.6p1-2ubuntu1`
```

所以要安装对应版本的openssh-clien,来覆盖掉ubuntu自带的

```
$ sudo apt-get install openssh-client=1:6.6p1-2ubuntu1
```





> ``正在读取软件包列表... 完成``正在分析软件包的依赖关系树       ``正在读取状态信息... 完成       ``建议安装的软件包：libpam-``ssh` `keychain monkeysphere``下列软件包将被【降级】：openssh-client``升级了 0 个软件包，新安装了 0 个软件包，降级了 1 个软件包，要卸载 0 个软件包，有 0 个软件包未被升级。``需要下载 566 kB 的软件包。``解压缩后会消耗掉 0 B 的额外空间。``您希望继续执行吗？ [Y``/n``] y``获取：1 http:``//cn``.archive.ubuntu.com``/ubuntu/trusty``/main` `openssh-client amd64 1:6.6p1-2ubuntu1 [566 kB]``下载 566 kB，耗时 2秒 (212 kB``/s``)        ``dpkg：警告：downgrading openssh-client from 1:6.6p1-2ubuntu2 to 1:6.6p1-2ubuntu1``(正在读取数据库 ... 系统当前共安装有 200015 个文件和目录。)``Preparing to unpack ...``/openssh-client_1``%3a6.6p1-2ubuntu1_amd64.deb ...``Unpacking openssh-client (1:6.6p1-2ubuntu1) over (1:6.6p1-2ubuntu2) ...``Processing triggers ``forman``-db (2.6.7.1-1) ...``正在设置 openssh-client (1:6.6p1-2ubuntu1) ...

可以看到,提示了系统中openssh-client被降级,这样再安装openssh-server就可以成功了!
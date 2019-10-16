## Linux系统下永久修改自己喜欢的系统主机名

> 三行代码实现想要效果

* 打开root目录下的隐藏文件` .bashrc` 

``` 
vi ~/.bashrc
```

- 添加如下代码

```bash
PS1="[\u@localhost \w]\$" #修改中间的localhost为自己想要修改的主机名
```

- 立即生效 

```bash
source ~/.bashrc
```


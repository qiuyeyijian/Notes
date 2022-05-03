## Ubuntu安装搜狗输入法



卸载ibus。



```shell
sudo apt-get remove ibus
```

清除ibus配置。



```shell
sudo apt-get purge ibus
```

卸载顶部面板任务栏上的键盘指示。



```shell
sudo  apt-get remove indicator-keyboard
```

安装fcitx输入法框架



```shell
sudo apt install fcitx-table-wbpy fcitx-config-gtk
```

切换为 Fcitx输入法



```shell
im-config -n fcitx
```

im-config 配置需要重启系统才能生效



```shell
sudo shutdown -r now
```

[下载搜狗输入法](https://links.jianshu.com/go?to=https%3A%2F%2Fpinyin.sogou.com%2Flinux%2F%3Fr%3Dpinyin)



```shell
wget http://cdn2.ime.sogou.com/dl/index/1524572264/sogoupinyin_2.2.0.0108_amd64.deb?st=ryCwKkvb-0zXvtBlhw5q4Q&e=1529739124&fn=sogoupinyin_2.2.0.0108_amd64.deb
```

安装搜狗输入法



```shell
sudo dpkg -i sogoupinyin_2.2.0.0108_amd64.deb
```

修复损坏缺少的包



```shell
 sudo apt-get install -f
```

打开 Fcitx 输入法配置

```shell
fcitx-config-gtk3
```

问题:输入法皮肤透明



```undefined
fcitx设置 >>附加组件>>勾选高级 >>取消经典界面

Configure>>  Addon  >>Advanced>>Classic
```



 ### 更换皮肤

去搜狗输入法官网下载相应皮肤，双击安装就行，和Windows一样


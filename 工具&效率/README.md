# README



## Chrome

### 1. 快捷键

```
//撤销关闭的标签页
Ctrl + Shift + T

//历史记录
Ctrl + h
```

### 2. 截图

```
1. 打开控制台
2. Ctrl + Shift + P 
3. 然后输入 screen 
4. 选择capture full size screenshot
```



## VMWare

### 开机自动挂载共享文件夹

手动挂载（每次开机都需要重新挂载）

```bash
vmhgfs-fuse .host:/VMShare /root/workspace
```

开机自动挂载

```bash
vim /etc/fstab
# /VMShare是共享文件夹名称，挂载到/root/workspace
.host:/VMShare /root/workspace fuse.vmhgfs-fuse allow_other 0 0	
```






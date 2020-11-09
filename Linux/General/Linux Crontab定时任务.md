linux内置的cron进程能帮我们实现这些需求，cron搭配shell脚本，非常复杂的指令也没有问题。

### cron介绍

我们经常使用的是crontab命令是cron table的简写，它是cron的配置文件，也可以叫它作业列表，我们可以在以下文件夹内找到相关配置文件。

- /var/spool/cron/ 目录下存放的是每个用户包括root的crontab任务，每个任务以创建者的名字命名
- /etc/crontab 这个文件负责调度各种管理和维护任务。
- /etc/cron.d/ 这个目录用来存放任何要执行的crontab文件或脚本。
- 我们还可以把脚本放在/etc/cron.hourly、/etc/cron.daily、/etc/cron.weekly、/etc/cron.monthly目录中，让它每小时/天/星期、月执行一次。

### crontab的使用

我们常用的命令如下：

```bash
crontab [-u username]　　　　//省略用户表表示操作当前用户的crontab
    -e      (编辑工作表)
    -l      (列出工作表里的命令)
    -r      (删除工作作)
```

我们用**crontab -e**进入当前用户的工作表编辑，是常见的vim界面。每行是一条命令。

crontab的命令构成为 时间+动作，其时间有**分、时、日、月、周**五种，操作符有

- ***** 取值范围内的所有数字
- **/** 每过多少个数字
- **-** 从X到Z
- **，**散列数字



### 实例1：每1分钟执行一次myCommand

```
* * * * * myCommand
```

### 实例2：每小时的第3和第15分钟执行

```
3,15 * * * * myCommand
```

### 实例3：在上午8点到11点的第3和第15分钟执行

```
3,15 8-11 * * * myCommand
```

### 实例4：每隔两天的上午8点到11点的第3和第15分钟执行

```
3,15 8-11 */2  *  * myCommand
```

### 实例5：每周一上午8点到11点的第3和第15分钟执行

```
3,15 8-11 * * 1 myCommand
```

### 实例6：每晚的21:30重启smb

```
30 21 * * * /etc/init.d/smb restart
```

### 实例7：每月1、10、22日的4 : 45重启smb

```
45 4 1,10,22 * * /etc/init.d/smb restart
```

### 实例8：每周六、周日的1 : 10重启smb

```
10 1 * * 6,0 /etc/init.d/smb restart
```

### 实例9：每天18 : 00至23 : 00之间每隔30分钟重启smb

```
0,30 18-23 * * * /etc/init.d/smb restart
```

### 实例10：每星期六的晚上11 : 00 pm重启smb

```
0 23 * * 6 /etc/init.d/smb restart
```

### 实例11：每一小时重启smb

```
* */1 * * * /etc/init.d/smb restart
```

### 实例12：晚上11点到早上7点之间，每隔一小时重启smb

```
* 23-7/1 * * * /etc/init.d/smb restart
```
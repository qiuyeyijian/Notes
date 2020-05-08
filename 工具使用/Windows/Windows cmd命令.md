## cmd命令笔记

### 0x01. 查找，删除任务进程

以 mysql 为例

```
//查找程序进程
tasklist | find "mysql"		

//根据PID删除进程
taskkill -PID 进程PID -F

//开启服务
net start mysql

//关闭服务
net stop mysql

//删除服务
sc delete mysql
```



### 定时关机

```bash
shutdown -s -t 3600

shutdown -a				//取消定时任务
```



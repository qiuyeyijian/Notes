## cmd命令笔记



### 通用

```
shutdown -s -t 3600		// 定时关机，单位秒
shutdown -a				//取消定时任务

win i		// 打开设置
win +/-		// 放大镜
ctrl shift n	// 创建新的文件夹
ctrl win d		// 创建桌面
ctrl win 左右方向键		// 切换桌面

shift alt f		//VSCODE 代码格式化
```



### Win + R

```
services.msc		// 打开服务
regedit				// 打开注册表

psr				//打开步骤记录器
osk				//虚拟键盘
```



### 查找，删除任务进程

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



### 查看端口

```bash
netstat -ano	// 查看所有端口占用情况
netstat -aon|findstr "3306"		// 查看指定端口
taskkill /f /t /im 程序名		// 停止占用端口程序

```
















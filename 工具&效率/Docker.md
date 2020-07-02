## Docker 实用命令快速查询

### 基本操作

```dockerfile
//拉取镜像
docker pull 镜像

//查看正在运行的容器
docker ps

//查看所有容器 
docker ps -a

//根据容器id 进入对应镜像文件夹, 可修改相关配置文件，
docker exec -it 容器id /bin/bash

//如果需要以root 用户的身份进入容器则使用
docker exec -it --user root 容器id /bin/bash

//停止一个容器
docker stop 容器id

//启动一个已经停止的容器
docker start 容器id

//重启一个容器,不管容器是否启动
docker restart 容器id 

//删除镜像
docker rmi -f 镜像id

```



### 批量删除

删除所有容器

```
docker rm -f `docker ps -a -q` 
```

删除所有的镜像

```
docker rmi -f `docker images -q`
```



### docker 开机自启动，容器自启动

1. 开机启动docker

```
systemctl enable docker.service
```

2. 运行容器的时候，加上 `--restart=always` 命令可以令容器自启动

```
docker run -d --restart=always -p 10240:8080 -p 10241:50000 -v /var/jenkins_node:/var/jenkins_home -v /etc/localtime:/etc/localtime --name myjenkins jenkins/jenkins
```

3. 如果启动容器的时候没有加 `--restart=always` 命令，可以使用如下命令，来更新

```
sudo docker update --restart=always 容器id
```

4. 使用on-failure策略时，指定Docker将尝试重新启动容器的最大次数。默认情况下，Docker将尝试永远重新启动容器。

```
sudo docker update --restart=on-failure:10 容器id		//最大重启10次
```



* --restart具体参数值详细信息：
  * `--restat=no` 容器退出时，不重启容器
  * `--restart=no-failure`只有在非0状态退出时才重新启动容器；
  * `--restart=always` 无论退出状态是如何，都重启容器；

 










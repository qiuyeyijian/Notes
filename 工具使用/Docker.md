## Docker 实用命令快速查询

### 基本操作

```dockerfile
//拉取镜像
docker pull 镜像

//查看正在运行的容器
docker ps

//查看所有容器 
docker ps -a

//根据镜像id 进入对应镜像文件夹, 可修改相关配置文件，
docker exec -it 容器id /bin/bash

//如果需要以root 用户的身份进入容器则使用
docker exec -it --user root 容器id /bin/bash

//停止一个容器
docker stop 容器id

//启动一个已经停止的容器
docker start 容器id

//重启一个容器,不管容器是否启动
docker restart 容器id 



```


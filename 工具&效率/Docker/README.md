# Docker

## Docker 基本操作

### 安装Docker

``` bash
# step 1: 安装必要的一些系统工具
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
# Step 2: 添加软件源信息
sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# Step 3: 更新并安装Docker-CE
dnf install https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm

sudo yum makecache
sudo yum -y install docker-ce
# Step 4: 开启Docker服务
sudo systemctl start docker
# Step 5: 设置开机自启动
sudo systemctl enable docker
# Step 6: 测试
docker run hello-world

```

### Docker 的中央仓库

> 1. Docker官方的中央仓库：这个仓库是镜像最全的，但是下载速度较慢
>
>    https://hub.docker.com/
>
> 2. 国内镜像网站：网易蜂巢，daocloud...
>
>    https://c.163yun.com/hub/
>
>    http://hub.daocloud.io		(推荐使用)
>
> 3. 在公司内部会采用私服的方式拉去镜像（添加配置）

### 镜像的操作

下面均使用DaoCloud镜像站来拉去镜像

#### 1. 拉取镜像到本地

```sh
docker pull 镜像名称[:tag]

# 举个例子, 这里使用daocloud镜像站
docker pull daocloud.io/library/tomcat:8.5.15-jre8
```

#### 2. 查看本地全部镜像

```sh
docker images
```

#### 3. 删除本地镜像

```sh
docker rmi 镜像的标识
```

#### 4. 镜像的导入导出（不规范）

```sh
# 将本地的镜像导出
docker save -o 导出路径 镜像id

# 加载本地的镜像文件
docker load -i 镜像文件

# 修改镜像名称
docker tag 镜像id 新镜像名称:版本
```

### 容器的操作

#### 1. 运行容器

```sh
# 简单操作
docker run 镜像标识 | 镜像名称[:tag]
```

```sh
# 常用操作
docker run -d -p 宿主机端口:容器端口 --name 容器名称 镜像名称|镜像标识[:tag]
```

> * -d => 代表后台运行容器
> * -p 宿主机端口:容器端口  =>　当前Linux物理机端口到容器端口的映射
> * --name 容器名称 => 为当前要运行的容器起个名字

#### 2. 查看正在运行的容器

```sh
docker ps [-qa]
```

> * -a  => 查看全部容器，包括没有运行的
> * -q  => 只查看容器得到的标识

#### 3. 查看容器日志

```sh
docker logs -f 容器id
```

#### 4. 进入容器内部

```sh
docker exec -it 容器id /bin/bash
```

#### 5. 删除容器

删除容器前请先停止容器

```sh
# 1. 停止指定容器
docker stop 容器id
# 2. 停止所有容器
docker stop $(docker ps -qa)

# 3. 删除指定容器
docker rm 容器id
# 4. 删除全部容器
docker rm $(docker ps -qa)
```

#### 6. 启动容器

```sh
docker start 容器id
```



## Docker的应用

```sh
#可以通过命令将宿主机的内容复制到容器内部
docker cp 文件名称 容器id:容器内部路径
```



### 数据卷基本操作

> 为了部署SSM的工程，需要使用到cp命令将宿主机内的war包复制到容器内部。
>
> 数据卷：将宿主机的一个目录映射到容器的一个目录中。
>
> 可以在宿主机中操作目录中的内容，那么容器内部映射的文件也会跟着一起改变。

#### 1. 创建数据卷

```sh
# 创建数据卷后，默认会存放在一个目录下 /var/lib/docker/volumes/数据卷名称/_data
docker volume create 数据卷名称
```

#### 2. 查看数据卷的详细信息

```sh
docker volume inspect 数据卷名称
```

#### 3. 查看全部数据卷

```sh
docker volume ls
```

#### 4. 删除数据卷

```sh
docker volume rm 数据卷名称
```

### 数据卷应用

当你映射数据卷是，如果数据卷不存在，docker会帮你自动创建。将容器内部自带的文件，存储在默认的存放路径中。

```sh
docker run -v 数据卷名称:容器内部的路径 镜像id
```

如果指定一个路径作为数据卷的存放位置，这个路径下是空的。

```sh
docker run -v 路径:容器内部的路径 镜像id
```

## Docker 自定义镜像

> 中央仓库上的镜像，也是Docker的用户自己上传的

**1. 创建一个Dockerfile文件，并且自定义镜像信息**

```sh
vim Dockerfile
```

> Dockerfile文件中常用的内容
>
> * from => 指定当前自定义镜像依赖的环境
> * copy  => 将相对路径下的内容复制到自定义镜像中
> * workdir  => 声明镜像的默认工作目录
> * cmd  => 需要执行的命令（在workdir下执行的，cmd可以写多个，只以最后一个为准）

```sh
# 举个栗子 vim /root/workspace/Dockerfile
from daocloud.io/library/tomcat:8.5.15-jre8
copy /root/workspace/ssm.war /usr/local/tomcat/webapps
```

```sh
# 写好Dockerfile文件后，在Dockerfile文件目录下，运行命令制作镜像
docker build -t　镜像名称:[tag] /root/workspace/Dockerfile
```

## Docker-Compose

> * 之前运行一个镜像，需要添加大量参数。可以通过Docker-Compose编写这些参数
>
> * Docker-Compose可以帮助我们批量地管理容器，只需要通过一个`docker-compose.yml`文件去维护即可



### 下载安装

**1. 去GitHub下载Docker-Compose**

[docker-compose-Linux-x86_64](https://github.com/docker/compose/releases/download/1.26.2/docker-compose-Linux-x86_64)

**2. 赋予可执行权限**

```sh
# 重命名，方便接下来的操作
mv docker-compose-Linux-x86_64 docker-compose

# 赋予可执行权限
chmod 755 docker-compose
```

**3. 配置环境变量**

```sh
# 将docker-compose文件移动到 /usr/local/bin
mv docker-compose /usr/local/bin

echo "export PATH=$PATH:/usr/local/bin" >> /etc/profile.d/docker-compose.sh
chmod 755 /etc/profile.d/docker-compose.sh
```



### 编写配置文件

> yml文件以key:value方式来指定配置信息
>
> 多个配置信息以换行+空格缩进的方式来区分
>
> 在docker-compose.yml文件中，不要使用制表符

```yaml
version: '3.1'
services:
  mysql:  # 服务名称
    restart: always  #代表只要docker启动，那么这个容器就跟着一起启动
    image: daocloud.io/library/mysql:5.7.6  #指定镜像路径
    container_name: mysql  # 指定容器名称
    ports:
      - 3306:3306  # 指定端口号的映射，可以指定多个，在下面继续添加即可
    environment:
      MYSQL_ROOT_PASSWORD: root  # 指定mysql的root用户的登录密码
      TZ: Asia/Shanghai  # 指定时区
  tomcat:
    restart: always
    image: daocloud.io/library/tomcat:8.5.15-jre8
    container_name: tomcat
    ports:
      - 8080:8080
    environment:
      TZ: Asia/Shanghai
    volumes:
      - /opt/docker/tomcat/webapps:/usr/local/tomcat/webapps
      - /opt/docker/tomcat/logs:/usr/local/tomcat/logs
```



### 使用docker-compose命名管理容器

> 在使用docker-compose的命令时，默认会在当前目录下找docker-compose.yml文件

```yaml
# 启动管理的容器
docker-compose up -d

# 关闭并删除容器
docker-compose down

# 开启|关闭|重启已经存在的有docker-compose维护的容器
docker-compose start|stop|resart

# 查看容器
docker-compose ps

#查看日志
docker-compose logs -f
```



### docker-compose配置Dockfile使用

> 使用docker-compose.yml文件以及Dockerfile文件在生成自定义镜像的同时启动当前镜像，并且又docker-compose去管理容器

```yaml
# docker-compose.yml 文件
version: '3.1'
services:
  test:
    restart: always
    build:
      context: ./
      dockerfile: Dockerfile
    image: test:1.0.1
    container_name: ssm
    ports:
      - 8081:8080
    environment:
      TZ: Asia/Shanghai
```

```sh
# Dockerfile 文件
from daocloud.io/library/tomcat:8.5.15-jre8
copy /root/workspace/ssm.war /usr/local/tomcat/webapps
```

可以直接启动基于docker-compose.yml以及Dockerfile文件构建的自定义镜像

```sh
docker-compose up -d
```

如果自定义镜像不存在，会帮助我们构建出自定义镜像，如果自定义镜像已经存在，会直接运行这个自定义镜像

如果需要重新构建自定义镜像，使用

```sh
# 只是重新构建
docker-compose build

# 运行前构建
docker-compose up -d --build
```



## Docker 一些问题

### 进入容器内部

```sh
//根据容器id 进入对应镜像文件夹, 可修改相关配置文件，
docker exec -it 容器id /bin/bash

//如果需要以root 用户的身份进入容器则使用
docker exec -it --user root 容器id /bin/bash
```

### 容器自启动

运行容器的时候，加上 `--restart=always` 命令可以令容器自启动

```sh
docker run -d --restart=always -p 10240:8080 -p 10241:50000 -v /var/jenkins_node:/var/jenkins_home -v /etc/localtime:/etc/localtime --name myjenkins jenkins/jenkins
```

如果启动容器的时候没有加 `--restart=always` 命令，可以使用如下命令，来更新

```sh
sudo docker update --restart=always 容器id
```

使用on-failure策略时，指定Docker将尝试重新启动容器的最大次数。默认情况下，Docker将尝试永远重新启动容器。

```sh
sudo docker update --restart=on-failure:10 容器id		//最大重启10次
```

> --restart具体参数值详细信息：
>
> * `--restat=no` 容器退出时，不重启容器
> * `--restart=no-failure`只有在非0状态退出时才重新启动容器；
> * `--restart=always` 无论退出状态是如何，都重启容器；

 

## Docker CI, CD






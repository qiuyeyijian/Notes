# README

## Nodejs安装

### Windows

直接下载安装包即可

### Linux

首先安装npm，在Ubuntu下命令是

```bash
sudo apt-get install npm
```

如果Ubuntu下Node版本过低，还需要写在node，使用二进制包进行安装。下载Nodejs二进制包

```bash
wget https://nodejs.org/dist/v16.17.1/node-v16.17.1-linux-x64.tar.xz
```

解压

```bash
tar -xvf node-v16.17.1-linux-x64.tar.xz

# 重命名
mv node-v16.17.1-linux-x64.tar.xz node
```

设置软连接

```bash
sudo ln node/bin/node /usr/bin/node
```

安装node管理模块

```
npm install -g n
```

安装node稳定版本

```bash
n stable
```



## NPM

#### 查看全局配置

```shell
npm config ls
```



#### 设置全局安装路径和缓存

> 由于node全局模块大多数都是可以通过命令行访问的，所以还要把“**D:\Software\nodejs\node_global**”和“**D:\Software\nodejs\node_global\node_modules**”加入到系统PATH中，方便直接使用命令行运行。

```shell
npm config set cache "D:\Node\node_cache"
npm config set prefix "D:\Node\node_global"
npm config set registry https://registry.npmmirror.com
```



```shell
cache=D:\Node\node_cache
prefix=D:\Node\node_global
registry=https://registry.npm.taobao.org
```





## NPM的两种包

### 1. 业务包

程序正常运行时需要的 npm 包, 一般会放在 `package.json` 下的 `dependencies` 字段, 可使用 `npm install $package --save` 的方式进行安装或者更新。

比如, Vue 前端工程中依赖的 `vue` (参考 [https://cli.vuejs.org/guide/creating-a-project.html#vue-create](https://link.zhihu.com/?target=https%3A//cli.vuejs.org/guide/creating-a-project.html%23vue-create) )

### 2. 工具包

程序开发时依赖的 npm 包, 一般会放在 `package.json` 下的 `devDependencies` 字段，可使用 `npm install $package --save-dev` 的方式进行安装或者更新。

比如, 前端工程中的打包工具 `webpack` 及其相关的插件



## YARN

#### 查看全局安装路径

```shell
yarn global dir
```



#### 修改全局安装位置

```shell
yarn config  set global-folder "D:\Node\node_global"
yarn global dir  //在执行查看位置,已经被修改

yarn cache dir //查看缓存位置
yarn cache clean // 清除缓存,在目录
yarn config set cache-folder "D:\Node\node_cache"  //设置D盘
yarn cache dir //在输出一下目录 看看缓存位置
```



#### 重新设置bin目录

```shell
yarn global bin //默认是 c:/,修改到 D:盘
yarn config set prefix "D:\Node\node_global\bin"
```



## Linux环境下升级npm和node的版本

 **一定要先升级node，再升级npm**。高版本的npm依赖高版本的node，如果先升级npm了，node版本很低，npm命令执行就会报错，提示当前node版本太低，需要升级node版本。 

这时候再来按照上面的方式更新node的话就不行了，因为npm不能用了。

```bash
# 清除缓存信息
npm cache clean -f
 
# 下载node安装包
npm install -g n
 
# 升级到nodejs最新稳定版本
n stable
 
# 查看当前版本
node -v
```








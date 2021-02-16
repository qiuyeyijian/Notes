### NPM

#### 查看全局配置

```shell
npm config ls
```



#### 设置全局安装路径和缓存

> 由于node全局模块大多数都是可以通过命令行访问的，所以还要把“**D:\Software\nodejs\node_global**”和“**D:\Software\nodejs\node_global\node_modules**”加入到系统PATH中，方便直接使用命令行运行。

```shell
npm config set cache "D:\Node\node_cache"
npm config set prefix "D:\Node\node_global"
npm config set registry https://registry.npm.taobao.org
```



```shell
cache=D:\Node\node_cache
prefix=D:\Node\node_global
registry=https://registry.npm.taobao.org
```





### YARN

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






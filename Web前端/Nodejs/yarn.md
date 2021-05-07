

```js
npm init                              ---- yarn init
npm install                           ---- yarn 
npm install xxx@1.1.1 -g              ---- yarn global add xxx@1.1.1
npm install xxx@1.1.1 --save          ---- yarn add xxx@1.1.1
npm install xxx@1.1.1 --save-dev      ---- yarn add xxx@1.1.1 --dev
npm uninstall xxx --save(-dev)        ----yarn remove xxx
npm run xxx                           ---- yarn run xxxx
```

## **初始化一个新的项目**

```js
yarn init --yes # 简写 -y

npm init --yes # 简写 -y
```

## 添加项目依赖/开发依赖

```js
yarn add <package...> [--dev/-D] //不带-D默认生产环境
yarn add [package]@[version] #带版本

npm install XXX --save 可以简写成npm i XXX -S --------> 安装项目依赖
npm install XXX --save-dev可以简写成npm i XXX -D ------> 安装开发依赖
```

## 查看源和换源

```js
npm config get registry  // 查看npm当前镜像源
npm config set registry https://registry.npm.taobao.org/  // 设置npm镜像源为淘宝镜像

yarn config get registry  // 查看yarn当前镜像源
yarn config set registry https://registry.npm.taobao.org/  // 设置yarn镜像源为淘宝镜像

镜像源地址部分如下：
npm --- https://registry.npmjs.org/
cnpm --- https://r.cnpmjs.org/
taobao --- https://registry.npm.taobao.org/
```

## 全局安装一个依赖

```js
yarn global add [package]

npm install [package] -g 
```

## 移除一个依赖

```js
yarn remove <packageName>
    
npm uninstall <packageName> -S
```

## 全局删除一个依赖

```js
yarn global remove <packageName>

npm uninstall -g <packageName>    
```

## 安装所有依赖包

```js
yarn 

npm i
```

## 升级依赖

```js
yarn upgrade # 升级所有依赖项，不记录在 package.json 中
npm update # npm 可以通过 ‘--save|--save-dev’ 指定升级哪类依赖

yarn upgrade webpack # 升级指定包
npm update webpack --save-dev # npm

yarn upgrade --latest # 忽略版本规则，升级到最新版本，并且更新 package.json
```

## 运行脚本

```js
yarn run

npm run
```

## 列出全局安装的所有依赖

```js
yarn global list --depth=0    # 限制依赖的深度

npm list -g --depth=0
```

## 缓存清理

```js
yarn cache clean

npm cache clean --force
```

## 查看依赖所有历史版本

```js
yarn info <package...>

npm v <package...> versions  //缩写
```
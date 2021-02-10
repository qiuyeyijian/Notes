
> 学习Git时记录的一些笔记。随着学习的不断深入，会不断对文章进行修改。

## 基本操作

### Git全局配置

```bash
git config --global user.name "用户名"
git config --global user.email "邮箱"
```

```shell
# 生成SSH密钥
ssh-keygen -t rsa -b 4096 -C "qiuyeyijians@gmail.com"
```



## 进阶操作

### 创建本地仓库

在本地创建仓库一般有两种方法：

* 手动从0到1创建，并关联远程仓库。此方法较为麻烦，但在一些场合十分有用。
* 直接克隆已有的远程仓库到本地。此方法常用，简单易行。

这两种方法都要求在Github提前创建远程仓库，为了演示方便，博主在Github上创建了一个测试仓库。

测试仓库地址：`git@github.com:qiuyeyijian/test.git`

#### 手动从0到1创建

1. 新建一个文件夹，然后进入到此文件夹下并打开终端
2. 初始化Git

```shell
git init
```

3. 关联一个远程仓库，命名为`origin`，默认仓库以此命名。

```shell
git remote add origin git@github.com:qiuyeyijian/test.git

git remote -v		# 查看是否添加成功
```

4. 同步远程所有分支

```shell
git fetch --all				# 此时只是同步分支信息而已

git pull origin master		# 拉取远程主分支，此时会拉取远程分支master中的所有文件
```

5. 将远程的主分支和我们本地的主分支关联起来

```shell
git branch --set-upstream-to=origin/master
```

> 说明：
>
> `git branch --set-upstream-to=<remote>/<branch> master` 
>
> * remote 改成之前我们设置的远程仓库的名字，然后 branch 换成远程仓库的分支
> * master 是本地的 master 分支

至此创建完成，接下来就可以在此文件夹下编写代码并提交Github，如何提交接下来也会说明。



#### 直接克隆远程分支到本地

```shell
git clone git@github.com:qiuyeyijian/test.git
```

Git为了能进行版本控制，方便进行版本回退，会将用户所有修改记录保存到`.git`  文件夹下，时间久了就会导致记录文件很大。我们在克隆下载别人仓库的时候，可以只克隆最近一次提交记录即可，可以加快下载速度。只需要在后面加上`--depth 1`参数即可。

```shell
git clone git@github.com:qiuyeyijian/test.git --depth 1
```



### 提交远程仓库

我们在本地仓库编辑好代码后，就可以提交到远程仓库了，具体流程如下：

1. 添加所有修改到暂存区，注意add后面的点

```shell
git add .
```

2. 为你的修改做一些说明

```shell
git commit -m "first commit"    //将first commit 替换成你的一些说明
```

3. 推送到远程仓库的`master`分支

```shell
git push origin master

# 可以简写成，因为origin是默认远程仓库，master是默认主分支
git push
```



### 同时关联Github和Gitee远程仓库

利用`创建本地仓库`方法，我们的本地仓库已经关联了Github远程仓库，接下来将演示如何再添加一个Gitee远程仓库。博主已经提前在Gitee上创建好了测试仓库：`git@gitee.com:qiuyeyijian/test.git`

1. 添加Gitee远程仓库

```bash
git remote add gitee git@gitee.com:qiuyeyijian/test.git
```

> 说明：
>
> `git remote add <远程仓库名> url`
>
> * **远程仓库名**可以随便起，容易记就行。
> * url 可以是 https://形式的，如果你添加了ssh，就可以使用上面那种形式

2. 使用`git remote -v`查看所有远程分支，配置成功会出现：

```bash
gitee   git@gitee.com:qiuyeyijian/test.git (fetch)
gitee   git@gitee.com:qiuyeyijian/test.git (push)
origin  git@github.com:qiuyeyijian/test.git (fetch)
origin  git@github.com:qiuyeyijian/test.git (push)
```

3. 同步远程所有分支，即同步Gitee和Github所有分支

```bash
git fetch --all
```

```bash
git pull gitee master --allow-unrelated-histories	# 拉取Gitee的master仓库分支内容
```

> 说明：
>
> * 拉取gitee上的远程分支，后面加的命令的意思是忽略版本不同，不然会报错`fatal: refusing to merge unrelated histories` 
> * 如果有冲突的话就直接解决冲突，不在赘述

4. 至此，Gitee远程仓库已经添加完毕。之后我们需要提交到远程仓库时：

* 提交到Github

```shell
git push origin master		 #可以简写 git push
```

* 提交到Gitee

```shell
git push gitee master		# 可以简写 git push gitee
```



### 6. 查看Git状态

```bash
git status
```

### 7. 查看历史提交
```bash
git log
git log --decorate                      //显示指向这个提交的所有引用，比单独的git log 显示信息更多
git log --decorate --oneline --graph --all  //以图形的形式显示分支信息

git log --oneline 
git log --pretty=oneline 	//都能发挥log， 没有pretty的是，只有commit id 前7位，加pretty的是全部的id
```

### 8. reset 命令回滚选项
移动HEAD的指向，将其指向上一个快照，将HEAD移动后指向的快照回滚到暂存区域
```bash
git reset --mixed HEAD~
```

移动HEAD的指向，将其指向上一个快照，相当于撤销最近一次的commit提交
```bash
git reset --soft HEAD~
```
移动HEAD的指向，将其指向上一个快照，将HEAD移动后指向的快照回滚到暂存区域，将暂存区域的文件还原到工作目录
```bash
git reset --hard HEAD~

git reset --hard commit_id
```

### 9. diff 比较命令
```c
git diff
//比较工作目录和暂存区域的
```
```bash
   HP@QiuYeYiJian MINGW64 /f/GitPractice/myproject (master)
   $ git diff
   diff --git a/README.md b/README.md   //比较暂存区域的README和工作目录的README
   index 0cb0ebd..1be4651 100644        //文件id 权限
   --- a/README.md                      //旧文件，存放在暂存区域的文件
   +++ b/README.md                      //新文件，存放在工作目录的文件
   @@ -1 +1,2 @@                        //-1：旧文件开始的行数，+1：新文件开始的行数，2：连续的行号
   -this is a big project                
   \ No newline at end of file          //文件不是以换行符结束   
   +this is a big project
   +qiuyeyijian
   \ No newline at end of file
   diff --git a/game.py b/game.py
   index e69de29..8671739 100644
   --- a/game.py
   +++ b/game.py
   @@ -0,0 +1 @@
   +print（"hello,world");
   \ No newline at end of file
```

### 10. 修改最后一次提交

```c
git commit --amend
```

### 11. 删除文件

```bash
git rm <file name>
git reset --soft HEAD~                      //回退当前指针
git rm -f <file name>                       //暴力删除工作目录和暂存区的文件
git rm --cached <file name>                 //只删除暂存区域的文件
```



### 13. 分支

```bash
git branch <name>                             //创建分支
git checkout <分支名>                          //切换分支
git checkout -b <分支名>                       //创建并切换分支
```

### 14. 合并分支
```bash
//先提交A代码
git checkout A 
git commit
git push
//切换到B分支
git checkout B
//开始合并
git merge A 
//合并完成 有冲突解决冲突
git push
```

### 15. 删除分支

```bash
git branch -d <分支名>                      //删除本地分支
git push origin --delete [branchname]      //删除远程分支
```

### 16. 匿名分支
使用checkout切换，但不加分支名，git会自动创建一个匿名分支。可以用来做实验，切换到主分支后，匿名分支不会保存。
```bash
git checkout HEAD~                          
```

### 17. 提交到远程仓库
```bash
git push                                    
```

或者提交到远程其他仓库
```bash
git remote add origin git@github.com:qiuyeyijian/test.git  //换成你要提交的GitHub仓库SSH地址
git push -u origin master                                   //提交到其他仓库
```


### 18. 解决合并冲突
```bash
git status         //查看状态
cat <file name>    //查看冲突文件
vi  <file name>    //修复冲突文件
git add <file>     //单独添加冲突文件
git commit -m "confict fixed"   //提交
git log --graph --pretty=oneline --abbrev-commit          //查看分支合并情况
```
### 20. 关联远程仓库
```bash
git remote add origin url

git remote rm origin		//删除关联
```

### 21. 忽略某些文件
首先在git仓库下新建一个`.gitignore`文件：
```bash
touch .gitignore
```

然后编辑`.gitignore`文件
    
```bash
vi .gitignore
```

默认会生成一个模板，你可以添加想忽略的某些文件或文件夹，下面是一些常用的的
```bash
target
.gitignore
.idea/
.classpath
.project
.settings
 ##filter databfile、sln file##
*.mdb
*.ldb
*.sln
##class file##
*.com
*.class
*.dll
*.exe
*.o
*.so
# compression file
*.7z
*.dmg
*.gz
*.iso
*.jar
*.rar
*.iml
*.ipr
*.iws
```

举个例子： 
1. 如果只想忽略一个文件或者文件夹，可以直接手动解除跟踪，而不必编写`.gitignore`文件，例如删除 `.idea`	文件夹
   
```bash
git rm -r --cached .idea/
git commit -a -m"delete .idea/ dir"
git push -u origin
```

2. 如果想一次性忽略多个文件或者文件夹，则需要别写编写`.gitignore`文件，编写之后，输入一下命令：
```bash
git rm -r --cached .				//注意后面的点
git commit -a -m"删除文件“
git push -u origin
```

### 22. git 版本回退

查看日志

```bash
$ git log --pretty
```

本地回退

```bash
$ git reset --hard commit_id(commit字符)
```

将远程同步

```bash
$ git push origin HEAD --force
```





### 25. git 瘦身

随着项目的版本不断迭代，仓库可能会非常臃肿，可以简单给git瘦身

#### A. 垃圾回收

```bash
git gc --prune=now
```

Git最初向磁盘中存储对象使用`松散`的格式，后续会将多个对象打包为一个二进制的`包文件`（`packfile`），以`节省磁盘空间`

#### B. 核弹级选项--git filter命令

1. 找出大文件前5个

```
   git verify-pack -v .git/objects/pack/pack-*.idx | sort -k 3 -g | tail -5
```

> $ git verify-pack -v .git/objects/pack/pack-*.idx | sort -k 3 -g | tail -5
> 50e6afdb7f535113126354e848033e7b591ff615 blob   4423175 4247726 43601345
> 154ee2def998228d1ec4b3c004d010bfa14573c0 blob   4428924 215946 17679055
> f7a32286c90c07e2f2e3cc1f9b01e656c6e0316d blob   8402104 931892 18074255
> 53184164d6c7096455e2d0f93a90ebbfd17f3539 blob   8561224 4190653 9083285
> c09e19d8931e7c3602e61b0b51892da2455cf049 blob   10224350 10170488 29150326

2. 找出大文件名, 以上面最后一个为例

```
git rev-list --objects --all | grep c09e19d8931e7c3602e61b0b51892da2455cf049 
```

> $ git rev-list --objects --all | grep c09e19d8931e7c3602e61b0b51892da2455cf049
> c09e19d8931e7c3602e61b0b51892da2455cf049 MyWebSite.zip

3. 使用 git filter，将`your file path>` 替换成你的**路径**， 例如上面的 `MyWebsite.zip`

   * 强制 Git 处理但不检出每个分支和标记的完整历史记录

   * 删除指定的文件，以及因此生成的任何空提交

```
$ git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch <your file path>" \
  --prune-empty --tag-name-filter cat -- --all
```

```
$ git for-each-ref --format="delete %(refname)" refs/original | git update-ref --stdin
$ git reflog expire --expire=now --all
$ git gc --prune=now
```



### 26. 重命名分支

**一：本地分支重命名**

```shell
git branch -m old-name new-name
```

**二：远程分支重命名**

> - 1、将远程分支删除掉
> - 2、将本地分支重命名
> - 3、将本地分支推到远程

比如：将远程的dev重命名为develop

```cpp
git push --delete origin dev
git branch -m dev develop
git push origin develop
```



### 克隆X的项目，一段时间后再次同步X的提交信息

1. 进入到自己项目，打开终端
2. 添加原作者的远程仓库地址，远程仓库名习惯命名`upstream`，当然也可以自定义其他

```shell
# git remote add [远程仓库名] [远程仓库地址]
git remote add upstream git@github.com:qiuyeyijian/Notes.git
```

3. 将原作者仓库的所有分支同步到本地

```shell
git fetch upstream
```

4. 将其中一个分支，比如`upstream/master`合并到自己本地的`master`分支

```shell
git checkout master
git merge upstream/master
```

5. 提交到自己的Github远程托管分支

```shell
git push
```




> 学习Git时记录的一些笔记，

1. Git全局配置

   ```c
   git config --global user.name "用户名"
   git config --global user.email "邮箱"
   ```

2. Git初始化

   ```c
   git init
   ```

3. 将需要进行版本管理的文件放入暂存区域

   ```
   git add .
   ```

4. 必须为你的修改做一些说明

   ```c
   git commit -m "first commit"    //将first commit 替换成你的一些说明
   ```

5. 将暂存区域中的文件提交到Git仓库

   ```
   git remote add origin git@github.com:qiuyeyijian/test.git  //换成你要提交的GitHub仓库SSH地址
   git push -u origin master
   ```

6. 查看Git状态

   ```c
   git status
   ```

7. 查看历史提交

   ```c
   git log
   git log --decorate                      //显示指向这个提交的所有引用，比单独的git log 显示信息更多
   git log --decorate --oneline --graph --all  //以图形的形式显示分支信息
   ```

8. reset 命令回滚选项

   ```c
   git reset --mixed HEAD~
   //移动HEAD的指向，将其指向上一个快照
   //将HEAD移动后指向的快照回滚到暂存区域
   ```

   ```c
   git reset --soft HEAD~
   //移动HEAD的指向，将其指向上一个快照，相当于撤销最近一次的commit提交
   ```

   ```c
   git reset --hard HEAD~
   //移动HEAD的指向，将其指向上一个快照
   //将HEAD移动后指向的快照回滚到暂存区域
   //将暂存区域的文件还原到工作目录
   ```

9. diff 比较命令

   ```c
   git diff
   //比较工作目录和暂存区域的
   ```

   ``` c
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

10. 修改最后一次提交

    ``` c
    git commit --amend
    ```

11. 删除文件

    ``` c
    git rm <file name>
    git reset --soft HEAD~                      //回退当前指针
    git rm -f <file name>                       //暴力删除工作目录和暂存区的文件
    git rm --cached <file name>                 //只删除暂存区域的文件
    ```

12. 重命名文件

    ``` c
    git mv <old file name> <new file name>
    ```

13. 分支

    ``` c
    git branch <name>                             //创建分支
    git checkout <分支名>                          //切换分支
    git checkout -b <分支名>                       //创建并切换分支
    ```

14. 合并分支

    ``` 
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

15. 删除分支

    ```c
    git branch -d <分支名>                      //删除本地分支
    git push origin --delete [branchname]      //删除远程分支
    ```

16. 匿名分支

    ``` 
    git checkout HEAD~                          //使用checkout切换，但不加分支名，git会自动创建一个匿名分支                                                 //可以用来做实验，切换到主分支后，匿名分支不会保存。
    ```

17. 提交到远程仓库

    ``` c
    git push                                    
    ```

    或者提交到远程其他仓库

    ``` c
    git remote add origin git@github.com:qiuyeyijian/test.git  //换成你要提交的GitHub仓库SSH地址
    git push -u origin master                                   //提交到其他仓库
    ```


18. 解决合并冲突

    ``` 
    git status         //查看状态
    cat <file name>    //查看冲突文件
    vi  <file name>    //修复冲突文件
    git add <file>     //单独添加冲突文件
    git commit -m "confict fixed"   //提价
    git log --graph --pretty=oneline --abbrev-commit          //查看分支合并情况             
    ```

    


## git add未commit reset恢复文件

系统环境：CentOS

恢复步骤：

​      1、git fsck > files.txt   或获取到所有悬挂的文件，当然也可通过find .git/objects -type f | xargs ls -lt | sed 60q查看最近60次add的文件，将悬挂的文件名称存入files.txt中，文件内容如下

```
dangling blob d6bf40c9f290161c87230787a1056d977d36c821dangling blob d61f00d8cad3920809f4d992ac3031b3f32e7f10dangling blob d7af99b5e2ae9a21d534f1965c35a2b572143322dangling blob d96f555491868caffb665c2dd391108abfcac581dangling blob da2f86e1710b8539b8047e4452f1ff6cb0e1f211dangling blob e0dfd04e4d3fcbaa6588c8cbb9e9065609bcb862dangling blob e06f361eb6d429290806b9f9cd7a0aebce22be4ddangling blob e2bfcf6c21b1b9116459e2213b0bd9b5f52b4b67dangling blob e23f0b42283d43c029f747596ed573859c917876dangling blob e3dfe04304e451a9a75a46fd0d052279f601f09ddangling blob f50fc6c14e67a228c4ba9a61b1357c16410e8228dangling blob f55f1b358726d8f23b1a2e57bee6863387bd7ad4dangling blob fe4f6f5494085ec15a05838bdf793f3ef0532f5f
```

​      2、编辑文件去掉dangling blob 关键字，当然也可在后面脚本处理，这里直接vim处理 1,$s/dangling blob //g，最终只留下类似fe4f6f5494085ec15a05838bdf793f3ef0532f5f的文件信息

​       3、将blob字节文件还原为原文，这里注意，文件名称已经丢失，最终只能通过手动更新文件名称

```
#!/bin/bashfor line in `cat files.txt`do        echo "File:${line}"        git show ${line} > files/${line}.txtdone
```

​      通过git show 将blob文件还原为对应文件。

​      4、从还原的所有文件中查找需要找回的文件，并且更名。

​            grep "关键" *.txt

 

 

至此git  add 且未commit的文件，在执行reset --hard commitId后的恢复操作结束。如果有更好的操作请留言！

 
# Vue

## Delete `␍`eslint(prettier/prettier)，解决方案

在gitLab上新建一个项目，拉到VSCode上面之后，每次保存就会报错Delete `␍`eslint(prettier/prettier)，问题的根源在于git的一个配置属性：core.autocrlf

windows下和linux下的文本文件的换行符不一致

windows：在换行的时候同时使用了CR和LF换行符

Mac和Linux：仅仅使用了换行符LF

我的项目仓库中默认是Linux环境下提交的代码，文件默认是以LF结尾的

当我在windows环境下克隆代码的时候，autocrlf属性默认为true，换行符为CRLF，二者不兼容所以会报错

**点击VSCode右下角LF/CRLF，然后根据弹窗进行修改即可**
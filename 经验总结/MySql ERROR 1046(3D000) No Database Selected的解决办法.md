# MySql ERROR 1046(3D000): No Database Selected的解决办法

2018年11月16日 22:47:16 [快乐的大白](https://me.csdn.net/qq_43116788) 阅读数 7489



[1.No](http://1.no/) DataBase Selected 翻译->意思是说没有选种数据库.
首先要建立数据库,在将表放入数据库中:
create database 如果上一步已经做好了,那么在命令行中敲入:
use 我试过这种方法!
2.第二个有可能的原因也是一个比较低级的错误，我们都知道mysql一创建就默认带一个叫mysql的数据库，如下图所示：![在这里插入图片描述](https://img-blog.csdnimg.cn/20181116224436795.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMTE2Nzg4,size_16,color_FFFFFF,t_70)
简单来说select user，password，host from user；这个命令必须是针对某个具体的数据库而言的。
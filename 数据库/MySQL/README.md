## MySQL 数据库简单操作命令

### MySQL数据库的连接和导入表

```mysql
[root@host]# mysql -u root -p
Enter password:******
```

source 命令导入数据库需要先登录到数库终端：

```
mysql> create database abc;      # 创建数据库
mysql> use abc;                  # 使用已创建的数据库 
mysql> set names utf8;           # 设置编码
mysql> source /home/abc/abc.sql  # 导入备份数据库
```

###  idea 连接MySQL报错

```mysql

Server returns invalid timezone. Go to 'Advanced' tab and set 'serverTimezone' property manually. 
```

只需要在连接语句后面加上`?serverTimezone=GMT`

```mysql
jdbc:mysql://localhost:3306?serverTimezone=GMT
```



### Mysql 基本命令



```mysql
show databases;				//显示所有数据库
show create database mysql;				//显示创建名字为"mysql"的数据库的语句

```





### Mysql 数据库CURD

#### 1. 删除数据

delete 语句用于删除表中的数据, 基本用法为:

```mysql
delete from 表名称 where 删除条件;
```

使用示例:

```mysql
delete from students where id=2;		//删除id为2的行
delete from students where age<20;		//删除所有年龄小于21岁的数据
delete from students;					//删除表中的所有数据
```

清空表信息的方式有两种 :

```mysql
truncate table table_name;
delete * from table_name;
```

truncate操作中的table可以省略，delete操作中的*可以省略

- truncate、delete 清空表数据的区别 :

> * truncate 是整体删除 (速度较快)，delete是逐条删除 (速度较慢)
> *  truncate 不写服务器 log，delete 写服务器 log，也就是 truncate 效率比 delete高的原因
> * truncate 不激活trigger (触发器)，但是会重置Identity (标识列、自增字段)，相当于自增列会被置为初始值，又重新从1开始记录，而不是接着原来的 ID数。而 delete 删除以后，identity 依旧是接着被删除的最近的那一条记录ID加1后进行记录。如果只需删除表中的部分记录，只能使用 DELETE语句配合 where条件



#### 2. 插入数据

 insert 语句可以用来将一行或多行数据插到数据库表中, 使用的一般形式如下:

```mysql
insert [into] 表名 [(列名1, 列名2, 列名3, ...)] values (值1, 值2, 值3, ...);
```

其中 [] 内的内容是可选的`推荐不要省去，这样可以避免很多错误`

```mysql
 "INSERT INTO books(title,author,publisher,price) VALUES ('汇编语言', '王爽', '清华大学出版社', 39.00);"
```

```mysql
insert into 表名(字段1, 字段2, 字段3) values(值1, 值2, 值3);
```



#### 3. 更新数据

update 语句可用来修改表中的数据, 基本的使用形式为:

```mysql
update 表名称 set 字段=新值 where 更新条件;
```

示例:

```mysql
update students set tel=default where id=5;		//将id为5的手机号改为默认的"-"
update students set age=age+1;				//将所有人的年龄增加1
update students set name="张伟鹏", age=19 where tel="13288097888";
```



#### 4. 删除与添加字段

添加列

```mysql
alter table 表名 add 列名 列数据类型 [after 插入位置];				//添加列
```

示例:

```mysql
alter table students add address char(60);		//在表的最后追加列 address 
alter table students add birthday date after age;	//在名为 age 的列后插入列 birthday 
```

修改列

```mysql
alter table 表名 change 列名称 列新名称 新数据类型;
```

示例:

```mysql
 alter table students change tel telphone char(13) default "-"; //将表 tel 列改名为 telphone:

alter table students change name name char(16) not null;	//将 name 列的数据类型改为 char(16): 
```

删除列

```mysql
alter table 表名 drop 列名称;
```

示例:

```mysql
alter table books drop id;			//删除books表中的id字段
alter table books add id int(20) primary key auto_increment first;	//添加id字段
```



#### 5. 查询语句

按特定条件查询:

where 关键词用于指定查询条件, 用法形式为: 

```mysql
select 列名称 from 表名称 where 条件
```

以查询所有性别为女的信息为例, 输入查询语句: 

```mysql
select * from students where sex="女";
```

> where 子句不仅仅支持 "where 列名 = 值" 这种名等于值的查询形式, 对一般的比较运算的运算符都是支持的, 例如 =、>、<、>=、<、!= 以及一些扩展运算符 is [not] null、in、like 等等。 还可以对查询条件使用 or 和 and 进行组合查询, 以后还会学到更加高级的条件查询方式, 这里不再多做介绍。

示例:

```mysql
select * from students where age > 21;			//查询年龄在21岁以上的所有人信息: 

select * from students where name like "%王%";	//查询名字中带有 "王" 字的所有人信息: 

select * from students where id<5 and age>20;	//查询id小于5且年龄大于20的所有人信息: 
```



#### 6. 数据库和表

**重命名表**

```mysql
alter table 表名 rename 新表名;
```

示例:

```mysql
alter table students rename workmates;		//重命名 students 表为 workmates: 
```

**删除整张表**

```mysql
drop table 表名;
```

**删除整个数据库**

```mysql
drop database 数据库名;
```





## 多表查询

https://blog.csdn.net/u012660464/article/details/113977173

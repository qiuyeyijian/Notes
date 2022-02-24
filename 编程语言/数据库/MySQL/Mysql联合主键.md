 # Mysql联合主键



> 联合主键，指的是把两个列看成是一个整体，这个整体是不为空，唯一，不重复



### 创建表的同时创建联合主键

```mysql
create table 表名(

列名1 数据类型,

列名2 数据类型,

constraint  主键约束的名字  primary key(列名1,列名2)

);
```

```mysql
create table 表名(

列名1 数据类型,

列名2 数据类型,

primary key(列名1,列名2)

);
```



### 针对已经存在表，添加联合主键

```mysql
alter table 表名 add primary key(列名1,列名2);

alter table 表名 add constraint 主键约束的名字 primary key(列名1,列名2);

# 删除主键约束格式
alter table 表名 drop primary key;
```



 
## Oracle



### 安装教程

https://blog.csdn.net/qiuyeyijian/article/details/110142470

### 基本知识

oracle不同于mysql，mysql有不同的数据库，每个数据库下有很多表，oracle中的库是用用户名区分的，用户名作用类似于mysql中的数据库名。

打开cmd窗口，使用sqlplus命令登录oracle数据库

```sql
sqlplus 用户名/密码		// 例如 sqlplus qiuyeyijian/123456
```

如果使用 sys或者 system账号登录，后面要加 `as dba`

```sql
sqlplus sys/密码 as sysdba
```



### 管理命令

登录之后，可以使用一些命令来管理。

```sql
show user;		// 显示当前用户
alter user 用户名 account unlock;	//解锁用户
alter user 用户名 account lock;	//锁定用户
alter user 用户名 identified by 新密码;	// 修改密码

create user 用户名 identified by 密码;		// 创建用户并指定密码
grant dba to 用户名;		// 给用户添加DBA权限，也就是将用户变成数据库管理员

```



### 修改http端口

Oracle Express Edition(XE)默认的http端口是8080，这跟JBoss/Tomcat的默认端口相同，导致Jboss启动冲突。

以dba的身份登录XE，执行以下语句，注意最后的斜杠也要输入。

```shell
begin
  dbms_xdb.sethttpport('7000');
  dbms_xdb.setftpport('0');
end;
/
```

修改下面二个internet快捷方式(位于oraclexe安装目录的product\11.2.0\server下)

> X:\oraclexe\app\oracle\product\11.2.0\server\Get_Started.url
>
> X:\oraclexe\app\oracle\product\11.2.0\server\Database_homepage.url

用记事本打开这二个文件，把8080换成7000

以上处理对Oracle 11g /R2 同样有效



### 卸载Oracle数据库

1. 删除安装目录下所有软件，删除开始菜单里的快捷图标
2. 删除注册表， 需要删除的有以下几个：

> HKEY_LOCAL_MACHINE\SOFTWARE\ORACLE
>
> HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services节点下的所有Oracle选项
>
> HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Eventlog\Application下的所有Oracle选项

3. 删除环境变量



## 数据库和数据表的基本操作

### 创建数据表

#### 创建普通Oracle数据表

```sql
create table <表名>
(
	字段名1 数据类型 [列级别约束条件] [默认值],
    字段名2 数据类型 [列级别约束条件] [默认值],
    ...
    [表级别的约束条件]
);
```

```sql
/* 举个例子 */
create table db_1
(
	id		number(11),
	name	varchar(25),
	sex		char(2),
	salary	number(9, 2)	--精度9位，小数点后占2位，小数点前占7位
);

desc	db_1;		--查看表的结构
```



#### 创建带有主键约束的表 PRIMARY KEY

```sql
/* 举个例子 */
create table db_1
(
	id		number(11) primary key,		--在定义列的同时指定主键
	name	varchar(25),
	sex		char(2),
	salary	number(9, 2),
    /* 定义完所有列后指定主键 */
    --primary key(id)			单字段主键
    --primary key(name, sex)	多字段联合主键
);
```

如果创建的时候没有添加主键约束，可以通过`alter`添加主键约束。

如果已创建主键约束，也可以通过`alter`删除主键约束

```sql
alter table 表名
add constraints 约束名称 primary key(字段名称);

alter table 表名
drop constraints 约束名称;
```

```sql
alter table db_1
add constraints pk_id primary key(id);

alter table db_1
drop constraints pk_id;
```



#### 创建带有外键约束的表 FOREIGN KEY

通过定义`froeign key`约束来创建外键，一个表可以有一个或者多个外键。外键对应的是参照完整性，一个表的外键可以是空值，若不为空值，则每一个外键值必须等于另一表中主键的某个值。

主表（父表）：对于两个具有关联关系的表而言，相关联字段中主键所在的那个表是主表。

从表（自表）：对于两个具有关联关系的表而言，相关联字段中外键所在的那个表是从表。

```sql
[constraint<外键名>]foreign key 字段名1[,字段名2,...]
references<主表名> 主键列1[,主键列2,...]
```

```sql
create table tb_dept (
	id			number(11) primary key,
    name		varchar2(22) not null,
    location 	varchar2(50)
);
```

```sql
create table tb_employee (
	id			number(11) primary key,
    name		varchar2(22) not null,
    deptId		number(11),
    salary		number(9, 2),
    constraint fk_dept foreign key(deptId) references tb_dept(id)
);
```

上述语句执行成功后，在表`tb_employee`上添加了名为`fk_dept`的外键约束，外键字段值为`deptId`，其依赖于表`tb_dept`的主键`id`



**修改数据表的时候添加外键约束**

```sql
alter table 表名
add constraints 约束名称 foreign key(外键约束的字段名称)
references 表名(字段名称)
on  delete cascade;
```

```sql
alter table tb_employee
add constraints fk_dept foreign key(depId)
references tb_dept(id)
on  delete cascade;
```



**移除外键约束**

```sql
alter table 数据表名称
drop constraints 约束名称
```

```sql
alter table tb_employee
drop constraints fk_dept;
```



#### 创建带有非空约束的表 NOT NULL

非空约束是指字段的值不能为空值，这个空值（或NULL）不同于零、空白、或长度为0的字符串（如""），NULL 的意思是没有输入。

```sql
字段名 数据类型 not null
```

```sql
create table tb_6(
	id NUMBER(11) primary key,
	name varchar(22),
	sex char(2) not null
);
```



**修改表时添加非空约束**

```sql
alter table 数据表名称
modify 字段名称 not null
```

```sql
alter table tb_6
modify name not null;
```



**移除非空约束**

```sql
alter table 数据表名称
modify 字段名称 null
```

```sql
alter table tb_6
modify name null;
```



#### 创建带有唯一性约束的表 UNIQUE

唯一性约束用于强制实施列表集中值的唯一性。根据约束条件，要求该列的值唯一，允许为空，但只能出现一个空值。另外、主键也强制实行唯一性，但主键不允许NULL作为唯一的空值。

```sql
字段名 数据类型 unique
```

```sql
create table tb_8(
	id number(10) primary key,
	name varchar(10) unique,		-- 第一种
	sex varchar(2) not null,
	-- constraint uc_name unique(name)		--第二种
);
```



**在修改表时添加唯一性约束**

```sql
alter table 数据表名称
add constraint 约束名称 unique(字段名称)
```

```sql
alter table tb_8
add constraint uc_name unique(name);
```



**移除唯一性约束**

```sql
alter table 数据表名称
drop constraints 约束名称
```

```sql
alter table tb_8
drop constraints uc_name;
```








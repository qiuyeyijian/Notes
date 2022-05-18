# Oracle

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
alter table tb_1
add constraints pk_id primary key(id);

alter table tb_1
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
alter table 数据表名称 modify 字段名称 not null;	--添加非空约束
alter table 数据表名称 modify 字段名称 null;		--移除非空约束
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


alter table 数据表名称 add constraint 约束名称 unique(字段名称);	--添加唯一性约束
alter table 数据表名称 drop constraints 约束名称;	-- 移除唯一性约束
```



#### 创建带有检查约束的表 CHECK

唯一性约束用于强制实施列表集中值的唯一性。根据约束条件，要求该列的值唯一，允许为空，但只能出现一个空值。另外、主键也强制实行唯一性，但主键不允许NULL作为唯一的空值。

```sql
字段名 数据类型 unique
```

```sql
create table tb_8(
	id number(10) primary key,
	name varchar(10) unique,		
	sex varchar(2) not null,
    age gender varchar(2)
	constraint chk_gender check(gender='男' or gender='女')		--第二种
);


alter table 数据表名称 add constraint 约束名称 unique(字段名称);	--添加唯一性约束
alter table 数据表名称 drop constraints 约束名称;	-- 移除唯一性约束
```



### 查看数据表的结构

```sql
describe 表名;		-- 也可以简写desc
desc 表名
```



### 修改数据表

```sql
alter table 旧表名 rename to 新表名;					   -- 修改数据表名
alter table 表名 rename column 旧字段名 to 新字段名;		-- 修改数据表字段名
alter table 表名 add 新字段名 数据类型 not null;			-- 添加数据字段
alter table 表名 modify 字段名 数据类型;					-- 修改字段类型
```



### 删除数据表与数据库

```sql
drop table 表名;		-- 删除没有被关联的表
-- 有外键关联的表需要先删除关联的外键约束，再删除该表
-- Oracle删除数据库删除账号就行
```



## 数据类型和运算符

### Oracle 数据类型



| 类型      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| DATE      | 日期（日-月-年），DD-MM-YY(HH-MI-SS)，用来存储日期和时间，取值范围是公元前4712年到公园9999年12月31 |
| TIMESTAMP | 日期（日-月-年），DD-MM-YY(HH-MI-SS:FF3)，用来存储日期和时间，相比于date显示时间更精确，date精确到秒，timestamp精确到小数秒。timestamp存放日期和时间还能显示上午、下午和时区。 |



```sql
create table tb_emp4 (
	id number(10),
	birthday date
)

 select sysdate from dual;		-- 查看系统时间
 
 insert into tb_emp4(birthday) values('16-12月-2020');		-- 插入时间
 insert into tb_emp4 values(SYSDATE);		-- 插入系统时间
 
 alter session set nls_date_format='yyyy-mm-dd';	-- 修改时间的默认格式
 
 
```



### 常见运算符







## 查询数据表中的数据

### 基本查询语句

```sql
在Oracle中查看所bai有的表: select * from tab/dba_tables/dba_objects/cat;
看用户建立的表 : select table_name from user_tables; //
当前用户的表 select table_name from all_tables; //
所有用户的表 select table_name from dba_tables; //包括系统表
可以查询出所有的用户表索引 select * from user_indexes //
```





### 单表查询



### 使用聚合函数查询



### 多表之间的连接查询



### 带有附加条件的子查询



### 带有正则表达式查询





## 数据的基本操作

### 插入数据



### 更新数据



### 删除数据



## 视图的基本操作



### 创建视图



### 查看视图



### 修改视图



### 更新视图



### 删除视图



### 限制视图的数据操作



## 游标的基本操作



### 显示游标的使用和属性





### 隐式游标的使用和属性





## Oracle触发器的应用



### 创建触发器



### 查看触发器



### 修改触发器



### 删除触发器



## Oracle函数的应用



### 数学函数



### 字符串函数



### 日期和时间函数



### 转换函数



## Oracle的表空间管理



### 了解表空间



### 表空间的类型



### 创建表空间



### 查看表空间



### 表空间的管理



### 数据文件的管理



## Oracle的事物与锁



### 事务管理



### 锁的应用

Oracle的锁机制主要是执行对多个活动事务的并发控制，他可以控制多个用户对同一数据进行操作，使用锁机制，可以解决数据库的并发问题，从而保证数据库的完整性和一致性。

如果不使用锁机制，对数据的并发操作会带来下面一些问题：

脏读：当一个事务读取的记录式另一个事务的一部分时，如果第一个事务正常完成，就没什么问题。如果此时第一个事务未完成，就产生了脏读。

幻读：

非重复性读取：如果一个事务不止一次读取相同 的记录，但在两次读取中间有另一个事务刚好修改了数据，则两次读取的数据将出现差异。

丢失更新：一个事务更新了数据库之后，另一个事务再次对数据库更新，此时系统只能保留最后一个数据的修改。



在数据库最中有两种基本的锁：排它锁（Exclusive Locks，即X锁）和共享锁（Share Locks，即S锁）。

* 排它锁：当数据对象被加上排它锁时，其他事务不能对它读取和修改。
* 共享锁： 加了共享锁的数据对象可以被其他事务读取，但不能修改。

如果事务T获得了数据对象的排它锁，则事务T可以对数据对象读取和修改，其他事务不可读取和修改。

如果事务T获得了数据对象的共享锁，则所有事务只能读取，不能修改。

在Oracle中最重要的是DML锁，DML锁的目的在于保证并发情况下的数据完整性。在Oracle数据库中，DML锁主要包括TM锁和TX锁，其中TM锁称为表级所，TX锁彻称为事务锁或行级锁。

### 死锁的发生过程





## Oracle数据库安全管理



### 用户账户管理



### 用户权限管理



### 数据库角色管理



### 概要文件管理



### 资源限制与口令管理



### 锁定与解锁用户



## Oracle控制文件与日志管理



### 了解和管理控制文件



### 了解和管理日志文件



## Oracle的数据备份与还原



### 数据的备份与还原



### 数据表的导出与导入



## Oracle数据库的性能优化



### 性能优化原则



### 优化Oracle内存



### 优化查询操作



### 优化数据库结构



### 优化Oracle服务器
















# SQL基础

下面记录一下SQL的基础知识。

SQL是一门ANSI标准的计算机语言，虽然存在着不同版本的SQL语言，比如MySQL，Oracle，SQL Server等。但是，为了与ANSI标准兼容，他们必须以相似的方式共同来支持一些主要的命令，如SELECT、UPDATE、DELETE、INSERT、WHERE等。

**SQL数据库的表的字段名不要和SQL内置关键字一样，比如`password`**



## 认识SQL

### 定义

> **Structured Query Language（SQL）：结构化查询语言**
>
> 其实就是定义了操作所有关系型数据库的规则。每一种数据库操作的方式存在不一样的地方，称为“方言”。

```
1. SQL 语句可以单行或多行书写，以分号结尾。
2. 可使用空格和缩进来增强语句的可读性。
3. 一般数据库的 SQL 语句不区分大小写，关键字建议使用大写。
4. 注释
    单行注释: -- 注释内容 或 # 注释内容(mysql 特有) 
    多行注释: /* 注释 */
```



### 标准

**SQL是数据库沟通的语言标准，有3个主要标准：**

* ANSI SQL。对于ANSI SQL修改后在1992年采纳的标准成为SQL-92或者SQL2
* SQL-99。从SQL-92扩充而来，增加了对象关系特征和许多其他新的功能。
* 各大数据库厂商提供不同版本的的SQL。这些版本SQL不但能包括原始的ANSI标准，而且能很大程度上支持新推出的SQL-92标准。



#### 分类

> * **DDL(Data Definition Language)数据定义语言**
>   用来定义数据库对象：数据库，表，列等。关键字：create，drop，alter。
> * **DML(Data Manipulation Language)数据操作语言**
>   用来对数据库中表的数据进行增删改。关键字：insert，delete，update。
> * **DQL(Data Query Language)数据查询语言**
>   用来查询数据库中表的记录(数据)。关键字：select, where。
> * **DCL(Data Control Language)数据控制语言**
>   用来定义数据库的访问权限和安全级别，及创建用户。grant，revoke，commit，rollback。





## DDL:操作数据库、表

#### 数据库 CRUD

**数据库创建（Create）**

```mysql
create database 数据库名称;						/*创建数据库*/
create database if not exists 数据库名称;		/*创建数据库，判断不存在，再创建*/
create database 数据库名称 character set 字符集名;	/*创建数据库，并指定字符集*/

create database if not exists db4 character set gbk;	/*综合练习*/
```

**数据库查询 （Retrieve）**

```mysql
show databases;						/*查询所有数据库的名称*/
show create database 数据库名称;		/*查询某个数据库的字符集:查询某个数据库的创建语句*/
```

**数据库修改 （Update）**

```mysql
alter database 数据库名称 character set 字符集名称;		/*修改数据库的字符集*/
```

**数据库删除 （Delete）**

```mysql
drop database 数据库名称;				/*删除数据库*/
drop database if exists 数据库名称;		/*判断数据库存在，如果存在再删除*/
```



#### 数据库使用

```mysql
select database();				/*查询当前正在使用的数据库名称*/
use 数据库名称;					/*使用数据库*/
```



#### 表操作 CRUD

**表创建（Create）**

```mysql
create table 表名(
    列名1 数据类型1,
    列名2 数据类型2,
    ....
    列名n 数据类型n
);					/*注意：最后一列，不需要加逗号（,）*/
```

> * int：整数类型
>
> * double: 小数类型
>
> * date: 日期，只包含年月日，yyyy-MM-dd
>
> * datetime: 日期，包含年月日时分秒	 yyyy-MM-dd HH:mm:ss
>
> * timestamp: 时间错类型	包含年月日时分秒	 yyyy-MM-dd HH:mm:ss	
>
>   如果将来不给这个字段赋值，或赋值为null，则默认使用当前的系统时间，来自动赋值
>
> * varchar：字符串

```mysql
/* 来个栗子 */
create table student(
    id int,
    name varchar(32),
    age int ,
    score double(4,1),				/* 一共4位，小数占1位，最大为999.9 */
    birthday date,
    insert_time timestamp
);
```

```mysql
create table 表名 like 被复制的表名;	  		/*复制表*/
```



**表查询 （Retrieve）**

```mysql
show tables;					  /*查询某个数据库中所有的表名称*/
desc 表名;						/*查询表结构*/
```

**表修改 （Update）**

```mysql
alter table 表名 rename to 新的表名;				 /*修改表名*/
alter table 表名 character set 字符集名称;			/*修改表的字符集*/
alter table 表名 add 列名 数据类型;					/*添加一列*/
alter table 表名 change 列名 新列别 新数据类型;		 /*修改列名称 类型*/
alter table 表名 modify 列名 新数据类型;				/*只修改修改列类型*/
alter table 表名 drop 列名;							/*删除列*/
```

**表删除 （Delete）**

```mysql
drop table 表名;
drop table if exists 表名 ;
```

​	

## DML：增删改表中数据

#### 添加数据

```mysql
insert into 表名(列名1,列名2,...列名n) values(值1,值2,...值n);
```

> * 列名和值要一一对应。
> * 如果表名后，不定义列名，则默认给所有列添加值
>   `insert into 表名 values(值1,值2,...值n);`
> * **除了数字类型，其他类型需要使用引号(单双都可以)引起来**

#### 删除数据

```mysql
delete from 表名 [where 条件];
```

> * 如果不加条件，则删除表中所有记录。
> * 如果要删除所有记录
>   * `delete from 表名; `   -- 不推荐使用。有多少条记录就会执行多少次删除操
>   * `TRUNCATE TABLE 表名;  `   -- 推荐使用，效率更高 先删除表，然后再创建一张一样的表。

#### 修改数据

```mysql
update 表名 set 列名1 = 值1, 列名2 = 值2,... [where 条件];
```

> 如果不加任何条件，则会将表中所有记录全部修改。



## DQL：查询表中的记录

```mysql
select * from 表名;			//查询表中所有数据
```

**语法**

```mysql
select
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段
having
	分组之后的条件
order by
	排序
limit
	分页限定
```



创建数据

```mysql
CREATE TABLE student  ( id int, 			/*编号*/
                       name varchar(20), 	/*姓名*/ 
                       age int, 			/*年龄*/ 
                       sex varchar(5), 		/*性别*/ 
                       address varchar(100), /*地址*/ 
                       math int, 			/*数学*/ 
                       english int 			/*英语*/ 
                      ); 

INSERT INTO student (id,NAME,age,sex,address,math,english) 
                       VALUES(1,'马云',55,'男','杭州',66,78),
                       (2,'马化腾',45,'女','深圳',98,87),
                       (3,'马景涛',55,'男','香港',56,77),
                       (4,'柳岩',20,'女','湖南',76,65),
                       (5,'柳青',20,'男','湖南',86,NULL),
                       (6,'刘德华',57,'男','香港',99,99),
                       (7,'马德',22,'女','香港',99,99),
                       (8,'德玛西亚',18,'男','南京',56,65);
```

#### 基础查询

```mysql
select 字段名1，字段名2... from 表名;	/*如果查询所有字段，则可以使用*来替代字段列表*/
select distinct 字段名1 from 表名;	/* 去除重复结果集 */

/*
1. 如果null参与运算，那么结果为null。
2. 可以使用ifnull(null, 0),如果为null就替换成0;
3. as 可以用来起别名
*/
select name as 姓名, math as 数学, english as 英语, math+ifnull(english, 0) as 总分 from student;
```





#### 条件查询

**运算符**

| 比较运算符            | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| `>, <, <=,>=, =, <> ` | `<>`在SQL中表示不等于，在mysql中也可以使用`!=`， 但是没有`==` |
| `between...and`       | 在一个范围之内，如：`between 100 and 200`，相当于[100, 200]  |
| `in(集合)`            | 集合表示多个值，用逗号分隔                                   |
| `like"张%"`           | 模糊查询                                                     |
| `is null`             | 查询某一列为null的值，不能写`=null`                          |
| `is no null`          | 查询某一列不是null的值，不能写做`!=null`                     |

```mysql
-- 查询math分数大于80分的学生
select * from student3 where math>80;
-- 查询english分数小于或等于80分的学生
select * from student3 where english <=80;
-- 查询age等于20岁的学生
select * from student3 where age = 20;
-- 查询age不等于20岁的学生，注：不等于有两种写法
select * from student3 where age <> 20;
select * from student3 where age != 20;
-- 查询age为空的学生
select * from student3 where age is null;
```

**逻辑运算符**

| 逻辑运算符     | 说明                              |
| -------------- | --------------------------------- |
| `and`或者 `&&` | 与，SQL中建议使用前者，后者不通用 |
| `or`或者 `||`  | 或                                |
| `not`或者 `!`  | 非                                |

```mysql
-- 查询age大于35且性别为男的学生(两个条件同时满足)
select * from student3 where age>35 and sex='男';
-- 查询age大于35或性别为男的学生(两个条件其中一个满足)
select * from student3 where age>35 or sex='男';
-- 查询id是1或3或5的学生
select * from student3 where id=1 or id=3 or id=5;
```



**in 关键字**

| select 字段名 from 表名 where 字段 in (数据1， 数据2，...); |
| ----------------------------------------------------------- |
| in里面的每个数据都会作为一次条件，只要满足条件的就会显示    |

```mysql
-- 查询id是1或3或5的学生
select * from student3 where id in(1,3,5);
-- 查询id不是1或3或5的学生
select * from student3 where id not in(1,3,5);
```



**Exists查询**

> 1. 先执行外层SQL
> 2. 将外层SQL的结果集循环传入内层
> 3. 如果内层sql有返回结果，就将外层的当前结果加入查询结果集

**exists与子查询区别？**

1. 子查询先执行内层sql，结果传到外层
2. exists先执行外层，结果传到内层



**范围查询**

| between 值1 and 值2                                          |
| ------------------------------------------------------------ |
| 表示从值1到值2范围，包含头部和尾部                           |
| 比如： `age between 80 and 100`，相当于： `age >= 80 && age <= 100` |

```mysql
-- 查询english成绩大于等于75，且小于等于90的学生
select * from student3 where english between 75 and 90;
```



**like 关键字**

| like表示模糊查询                                       |
| ------------------------------------------------------ |
| `select * from 表名 where 字段名 like "通配符字符串"`; |



**Mysql 通配符**

| 通配符             | 说明               |
| ------------------ | ------------------ |
| `%`                | 匹配任意多个字符串 |
| `_` （一个下划线） | 匹配一个字符       |

```mysql
-- 查询姓马的学生
select * from student3 where name like '马%';
select * from student3 where name like '马';
-- 查询姓名中包含'德'字的学生
select * from student3 where name like '%德%';
-- 查询姓马，且姓名有两个字的学生
select * from student3 where name like '马_';
```







#### 排序查询

| order by 排序字段1 排序方式1 ，  排序字段2 排序方式2... |
| ------------------------------------------------------- |
| `select * from student order by math asc;`              |

> 如果有多个排序条件，则当前边的条件值一样时，才会判断第二条件。
>
> * ASC：升序，默认的。
>
> * DESC：降序。

```mysql
/* 按照数学成绩排名，如果数学成绩一样，则按照英语成绩排名*/
select * from student order by math asc, english desc;
```



#### 聚合函数

| 函数名称 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| count    | 计算个数，一般选择非空的列：主键。count(*)计算所有个数，包含null |
| max      | 计算最大值                                                   |
| min      | 计算最小值                                                   |
| sum      | 计算和                                                       |
| avg      | 计算平均值                                                   |

> 注意：**聚合函数的计算，排除null值。**
>
> 解决方案：
>
> 1. 选择不包含非空的列进行计算
> 2. IFNULL函数



```mysql
SELECT COUNT(ifnull(english, 0)) FROM student;
SELECT MAX(math) FROM student;
SELECT AVG(english) FROM student;
```



#### 分组查询

> **分组之后查询的字段：分组字段、聚合函数**

| group by 分组字段; |
| ------------------ |
|                    |


```mysql
-- 按照性别分组。分别查询男、女同学的平均分
SELECT sex , AVG(math) FROM student GROUP BY sex;
-- 按照性别分组。分别查询男、女同学的平均分,人数
SELECT sex , AVG(math),COUNT(id) FROM student GROUP BY sex;
--  按照性别分组。分别查询男、女同学的平均分,人数 要求：分数低于70分的人，不参与分组
SELECT sex , AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex;
--  按照性别分组。分别查询男、女同学的平均分,人数 要求：分数低于70分的人，不参与分组,分组之后。人数要大于2个人
SELECT sex , AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex HAVING COUNT(id) > 2;
SELECT sex , AVG(math),COUNT(id) 人数 FROM student WHERE math > 70 GROUP BY sex HAVING 人数 > 2;
```

**where 和 having 的区别**

> 1. where 在分组之前进行限定，如果不满足条件，则不参与分组。having在分组之后进行限定，如果不满足结果，则不会被查询出来
>
> 2. where 后不可以跟聚合函数，having可以进行聚合函数的判断。

​		

#### 分页查询

| limit 开始的索引,每页查询的条数;                 |
| ------------------------------------------------ |
| 开始的索引 = （当前的页码 - 1） * 每页显示的条数 |

**limit 是一个MySQL"方言"**

```mysql
-- 每页显示3条记录 
SELECT * FROM student LIMIT 0,3; -- 第1页			//第0，1, 2条记录
SELECT * FROM student LIMIT 3,3; -- 第2页			//第3, 4, 5 条记录
SELECT * FROM student LIMIT 6,3; -- 第3页			//第6，7, 8条记录

```



## 约束

> 约束：对表中的数据进行限定，保证数据的正确性、有效性和完整性。	
>
> 1. 主键约束：primary key
> 2. 非空约束：not null
> 3. 唯一约束：unique
> 4. 外键约束：foreign key

#### 非空约束：not null

> 值不能为null

1. 创建表时添加约束

   ```mysql
   CREATE TABLE stu(
   		id INT,
   		NAME VARCHAR(20) NOT NULL -- name为非空
   );
   ```

2. 创建表完后，添加非空约束

   ```mysql
   ALTER TABLE stu MODIFY NAME VARCHAR(20) NOT NULL;
   ```

3. 删除name的非空约束

   ```mysql
   ALTER TABLE stu MODIFY NAME VARCHAR(20);
   ```

   

####  唯一约束：unique

> 值不能重复

 1. 创建表时，添加唯一约束

    注意mysql中，唯一约束限定的列的值可以有多个null

    ```mysql
    CREATE TABLE stu(
    	id INT,
    	phone_number VARCHAR(20) UNIQUE -- 添加了唯一约束
    );
    ```

2. 删除唯一约束

   ```mysql
   ALTER TABLE stu DROP INDEX phone_number;
   ```

 3. 在创建表后，添加唯一约束

    ```mysql
    ALTER TABLE stu MODIFY phone_number VARCHAR(20) UNIQUE;
    ```



#### 主键约束：primary key

> 注意：
>
> 1. 含义：非空且唯一
> 2. 一张表只能有一个字段为主键
> 3. 主键就是表中记录的唯一标识

1. 在创建表时，添加主键约束

   ```mysql
   create table stu(
   	id int primary key,-- 给id添加主键约束
   	name varchar(20)
   );
   ```

2. 删除主键

   ```mysql
   -- 错误 alter table stu modify id int ;
   ALTER TABLE stu DROP PRIMARY KEY;
   ```

3. 创建完表后，添加主键

   ```mysql
   ALTER TABLE stu MODIFY id INT PRIMARY KEY;
   ```

4. 自动增长：

> 概念：如果某一列是数值类型的，使用 auto_increment 可以来完成值得自动增长

```mysql
/*在创建表时，添加主键约束，并且完成主键自增长*/
	create table stu(
		id int primary key auto_increment,-- 给id添加主键约束
		name varchar(20)
	);
	
/*删除自动增长*/
ALTER TABLE stu MODIFY id INT;

/*添加自动增长*/
ALTER TABLE stu MODIFY id INT AUTO_INCREMENT;
```

#### 外键约束：foreign key

> 让表与表产生关系，从而保证数据的正确性

1. 在创建表时，可以添加外键

   ```mysql
   create table 表名(
   	....
   	外键列
   	constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称)
   );
   ```

   

2. 删除外键

   ```mysql
   ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;
   ```

   

3. 创建表之后，添加外键

   ```mysql
   ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称);
   ```

4. 级联操作

   ```mysql
   ALTER TABLE 表名 ADD CONSTRAINT 外键名称 
   			FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称) ON UPDATE CASCADE ON DELETE CASCADE  ;
   			
   			
   -- 级联更新：ON UPDATE CASCADE 
   
   -- 级联删除：ON DELETE CASCADE 
   ```

   

## 视图

>视图是从一个或多个表（或视图）导出的表，它是一个虚拟表。数据库中只存放视图的定义，而不存放视图对应数据，这些数据仍然存放在原来的基本表中。在对视图的数据进行操作时，系统根据视图的定义去操作与视图相关联的基本表。

视图一旦一定义，就可以和基本表一样被查询、被删除。也可以在一个视图之上再定义新的视图，但对视图的更新（增、删、改）操作则有一定的限制。

视图的作用：

**简单化：**用户常用的操作不必每次都指定全部条件

**安全性：**用户只能查询和修改他们所能见到的数据

**独立性：**视图可以帮助用户屏蔽真实表结构变化带来的影响

视图能够简化用户的操作、使用户能以多种角度看待同一数据、对重构数据库提供了一定程度的逻辑独立性、能够对机密数据提供安全保护。



### 创建视图

创建视图使用`create view`语句，基本语法如下

```sql
create view 视图名 as 子查询语句;
```

```sql
create table tb_1(
	id number(10),
    quantity number(9),
    price number(9)
);

create view view_t1 as select id, quantity, price from tb_1;	-- 创建视图
-- 创建视图的时候重新命名字段，屏蔽真实字段名，起到安全保护作用
create view view_t2(id, qty, price) as select id, quantity, price from tb_1;

select * from view_t1;		-- 对刚刚创建的视图进行查询操作
```

**在多表上创建视图**

```sql
create table student(
	s_id number(9),
    name varchar(40)
);

create table student(
	s_id number(9),
    name varchar(40),
    glass varchar(40),
    addr varchar(20)
);

create view stu_glass(id, name, glass) 
as select student.s_id, student.name, stu_info.glass 
from student, stu_info
where student.s_id = stu_info.s_id;		-- 可以看到，视图很好地保护了基本表的结构


select * from stu_glass;		-- 查询刚刚创建的视图
desc stu_glass;		--查看视图结构
```



### 修改视图

```sql
-- 如果存在视图就修改，没有就创建
create or replace view view_t1(id, quty, pri) as select * from tb_1;

-- 修改视图的约束
alter view view_t1
add constraint unq_t unique(quty)
disable novalidate;		-- 以前数据和以后数据都不检查

-- 删除视图的约束
alter view view_t1
drop constraint unq_t;
```



### 更新视图

更新视图所使用的命令：`update`， `insert` ，`delete`和操作表名一样，因为视图不会存储具体数据，所以对视图的操作都会转化成对表的操作。

```sql
update view_t1 set quty=5;
insert into view_t1 vlaues(1, 3, 5);
delete from view_t1 where price=10;
```



### 删除视图

```sql
drop view view_name;
```

### 限制视图的数据操作

```sql
create view view_t2(id, qty, price) as 
select id, quantity, price from tb_1
with read only;		--  设置视图只读

create view view_t2(id, qty, price) as 
select id, quantity, price from tb_1
where price > 10
with check option;		-- 设置视图插入，删除，更新操作时检查
```



## 事务

> 事务用于保证数据的一致性，它由一组相关的DML语句组成，该组的DML语句要么全部成功，要么全部失败。

事务作为单个逻辑工作单元执行的一系列操作，具有4个特性：

**原子性（Atomicity）：**事务是一个完整的操作。事务的各个操作是不可分的；要么都执行，要么都不执行。

**一致性（Consistency）：**一个查询的结果必须与数据库在查询开始的状态保持一致（读不等待写，写不等待读）。

**隔离性（Isolation）：**对于其他会话来说，未完成的（也就是未提交的）事务必须不可见。

**持久性（Durability）：**事务一旦提交完成后，数据库就不可以丢失这个事务的结果，数据通过日志能够保持事务的持久性。





### 数据库的设计


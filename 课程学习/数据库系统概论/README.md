# 数据库



数据库是**长期存储**在计算机内、**有组织的**、**可共享**和大量数据的集合。

数据库三个阶段：人工管理，文件系统，数据库系统

https://blog.csdn.net/liuxyen/article/details/78591722

https://blog.csdn.net/qq_36982160/article/details/89258056



### 基本概念

一、相关概念

Data：数据，是数据库中存储的基本对象，是描述事物的符号记录。

Database：数据库，是长期储存在计算机内、有组织的、可共享的大量数据的集合。

DBMS：数据库管理系统，是位于用户与操作系统之间的一层数据管理软件，用于科学地组织、存储和管理数据、高效地获取和维护数据。

DBS：数据库系统，指在计算机系统中引入数据库后的系统，一般由数据库、数据库管理系统、应用系统、数据库管理员（DBA）构成。

数据模型：是用来抽象、表示和处理现实世界中的数据和信息的工具，是对现实世界的模拟，是数据库系统的核心和基础；其组成元素有数据结构、数据操作和完整性约束。

概念模型：也称信息模型，是按用户的观点来对数据和信息建模，主要用于数据库设计。

逻辑模型：是按计算机系统的观点对数据建模，用于DBMS实现。

物理模型：是对数据最底层的抽象，描述数据在系统内部的表示方式和存取方法，在磁盘或磁带上的存储方式和存取方法，是面向计算机系统的。

实体和属性：客观存在并可相互区别的事物称为实体。实体所具有的某一特性称为属性。

E-R图：即实体-关系图，用于描述现实世界的事物及其相互关系，是数据库概念模型设计的主要工具。

关系模式：从用户观点看，关系模式是由一组关系组成，每个关系的数据结构是一张规范化的二维表。

型/值：型是对某一类数据的结构和属性的说明；值是型的一个具体赋值，是型的实例。

数据库模式：是对数据库中全体数据的逻辑结构（数据项的名字、类型、取值范围等）和特征（数据之间的联系以及数据有关的安全性、完整性要求）的描述。

数据库的三级系统结构：外模式、模式和内模式。

数据库内模式：又称为存储模式，是对数据库物理结构和存储方式的描述，是数据在数据库内部的表示方式。一个数据库只有一个内模式。

数据库外模式：又称为子模式或用户模式，它是数据库用户能够看见和使用的局部数据的逻辑结构和特征的描述，是数据库用户的数据视图。通常是模式的子集。一个数据库可有多个外模式。

数据库的二级映像：外模式/模式映像、模式/内模式映像。


​     

 二、重点知识点

数据库系统由数据库、数据库管理系统、应用系统和数据库管理员构成。

数据模型的组成要素是：数据结构、数据操作、完整性约束条件。

实体型之间的联系分为一对一、一对多和多对多三种类型。

常见的数据模型包括：关系、层次、网状、面向对象、对象关系映射等几种。

关系模型的完整性约束包括：实体完整性、参照完整性和用户定义完整性。

阐述数据库三级模式、二级映象的含义及作用。

数据库三级模式反映的是数据的三个抽象层次： 模式是对数据库中全体数据的逻辑结构和特征的描述。内模式又称为存储模式，是对数据库物理结构和存储方式的描述。外模式又称为子模式或用户模式，是对特定数据库用户相关的局部数据的逻辑结构和特征的描述。
数据库三级模式通过二级映象在 DBMS 内部实现这三个抽象层次的联系和转换。外模式面向应用程序， 通过外模式/模式映象与逻辑模式建立联系， 实现数据的逻辑独立性。 模式/内模式映象建立模式与内模式之间的一对一映射， 实现数据的物理独立性。



### 关系代数

一、相关概念

主键： 能够唯一地标识一个元组的属性或属性组称为关系的键或候选键。 若一个关系有多个候选键则可选其一作为主键(Primary key)。

外键：如果一个关系的一个或一组属性引用(参照)了另一个关系的主键，则称这个或这组属性为外码或外键(Foreign key)。

关系数据库： 依照关系模型建立的数据库称为关系数据库。 它是在某个应用领域的所有关系的集合。

关系模式： 简单地说，关系模式就是对关系的型的定义， 包括关系的属性构成、各属性的数据类型、 属性间的依赖、 元组语义及完整性约束等。 关系是关系模式在某一时刻的状态或内容， 关系模型是型， 关系是值， 关系模型是静态的、 稳定的， 而关系是动态的、随时间不断变化的，因为关系操作在不断地更新着数据库中的数据。

实体完整性：用于标识实体的唯一性。它要求基本关系必须要有一个能够标识元组唯一性的主键，主键不能为空，也不可取重复值。

参照完整性： 用于维护实体之间的引用关系。 它要求一个关系的外键要么为空， 要么取与被参照关系对应的主键值，即外键值必须是主键中已存在的值。

用户定义的完整性：就是针对某一具体应用的数据必须满足的语义约束。包括非空、 唯一和布尔条件约束三种情况。

  二、重要知识点

关系数据库语言分为关系代数、关系演算和结构化查询语言三大类。

关系的 5 种基本操作是选择、投影、并、差、笛卡尔积。

关系模式是对关系的描述，五元组形式化表示为：R（U，D，DOM，F），其中

    R —— 关系名
    U —— 组成该关系的属性名集合
    D —— 属性组 U 中属性所来自的域
    DOM —— 属性向域的映象集合
    F —— 属性间的数据依赖关系集合

笛卡尔乘积，选择和投影运算如下

![img](assets/README/20130903203749812)

### SQL语句

一、相关概念

SQL：结构化查询语言的简称， 是关系数据库的标准语言。SQL 是一种通用的、 功能极强的关系数据库语言， 是对关系数据存取的标准接口， 也是不同数据库系统之间互操作的基础。集数据查询、数据操作、数据定义、和数据控制功能于一体。

数据定义：数据定义功能包括模式定义、表定义、视图和索引的定义。

嵌套查询：指将一个查询块嵌套在另一个查询块的 WHERE 子句或 HAVING 短语的条件中的查询。

  二、重要知识点

SQL 数据定义语句的操作对象有：模式、表、视图和索引。

SQL 数据定义语句的命令动词是：CREATE、DROP 和 ALTER。

RDBMS 中索引一般采用 B+树或 HASH 来实现。

索引可以分为唯一索引、非唯一索引和聚簇索引三种类型。



**SQL 创建表语句的一般格式**


```sql
create table <表名> (
	<列名> <数据类型> [<列级完整性约束],
    <列名> <数据类型> [<列级完整性约束],
    [表级完整性约束]
);
```

<数据类型>：数据库系统支持的各种数据类型，包括长度和精度。 

列级完整性约束：针对单个列(本列)的完整性约束， 包括 PRIMARY KEY、 REFERENCES表名(列名)、UNIQUE、NOT NULL 等。 

表级完整性约束：基于表中多列的约束，包括 PRIMARY KEY ( 列名列表) 、FOREIGN KEY REFERENCES 表名(列名) 等。

**SQL  创建索引语句的一般格式**

```sql
create [unique] [cluster] index <索引名> on <表名> (
	<列名列表>
);
```

UNIQUE：表示创建唯一索引，缺省为非唯一索引；

CLUSTER：表示创建聚簇索引，缺省为非聚簇索引；

 <列名列表>：一个或逗号分隔的多个列名，每个列名后可跟 ASC 或 DESC，表示升/降序，缺省为升序。多列时则按为多级排序。



**SQL 查询语句的一般格式**


```sql
select [all | distinct] <算术表达式列表> from <表名或视图名列表>
[ WHERE <条件表达式 1> ]
[ GROUP BY <属性列表 1> [ HAVING <条件表达式 2 > ] ]
[ ORDER BY <属性列表 2> [ ASC｜DESC ] ] ；
```

ALL／DISTINCT： 缺省为 ALL， 即列出所有查询结果记录， 包括重复记录。 DISTINCT则对重复记录只列出一条。

算术表达式列表：一个或多个逗号分隔的算术表达式，表达式由常量(包括数字和字符串)、列名、函数和算术运算符构成。每个表达式后还可跟别名。也可用 *代表查询表中的所有列。

 <表名或视图名列表>： 一个或多个逗号分隔的表或视图名。 表或视图名后可跟别名。

条件表达式 1：包含关系或逻辑运算符的表达式，代表查询条件。

条件表达式 2：包含关系或逻辑运算符的表达式，代表分组条件。

<属性列表 1>：一个或逗号分隔的多个列名。

<属性列表 2>： 一个或逗号分隔的多个列名， 每个列名后可跟 ASC 或 DESC， 表示升/降序，缺省为升序。



### 权限管理

一、相关概念和知识

触发器是用户定义在基本表上的一类由事件驱动的特殊过程。由服务器自动激活， 能执行更为复杂的检查和操作，具有更精细和更强大的数据控制能力。使用 CREATE TRIGGER 命令建立触发器。

计算机系统存在技术安全、管理安全和政策法律三类安全性问题。

TCSEC/TDI 标准由安全策略、责任、保证和文档四个方面内容构成。

常用存取控制方法包括自主存取控制(DAC)和强制存取控制(MAC)两种。

自主存取控制(DAC)的 SQL 语句包括 GRANT 和 REVOKE 两个。 用户权限由数据对象和操作类型两部分构成。


```sql

把对 Student 和 Course 表的全部权限授予所有用户。
GRANT ALL PRIVILIGES ON TABLE Student，Course TO PUBLIC ；

把对 Student 表的查询权和姓名修改权授予用户 U4。
GRANT SELECT，UPDATE(Sname) ON TABLE Student TO U4;

把对 SC 表的插入权限授予 U5 用户，并允许他传播该权限。
GRANT INSERT ON TABLE SC TO U5 WITH GRANT OPTION;

把用户 U5 对 SC 表的 INSERT 权限收回，同时收回被他传播出去的授权。
REVOKE INSERT ON TABLE SC FROM U5 CASCADE;

创建一个角色 R1，并使其对 Student 表具有数据查询和更新权限。
CREATE ROLE R1;
GRANT SELECT，UPDATE ON TABLE Student TO R1;

对修改 Student 表结构的操作进行审计
AUDIT ALTER ON Student ；
```

### 三大范式

 一、相关概念和知识点

数据依赖：反映一个关系内部属性与属性之间的约束关系，是现实世界属性间相互联系的抽象，属于数据内在的性质和语义的体现。

规范化理论：是用来设计良好的关系模式的基本理论。它通过分解关系模式来消除其中不合适的数据依赖，以解决插入异常、删除异常、更新异常和数据冗余问题。

函数依赖：简单地说，对于关系模式的两个属性子集X和Y，若X的任一取值能唯一确定Y的值，则称Y函数依赖于X，记作X→Y。

非平凡函数依赖：对于关系模式的两个属性子集X和Y，如果X→Y，但Y!⊆X，则称X→Y为非平凡函数依赖；如果X→Y，但Y⊆X，则称X→Y为非平凡函数依赖。

完全函数依赖：对于关系模式的两个属性子集X和Y，如果X→Y，并且对于X的任何一个真子集X'，都没有X'→Y，则称Y对X完全函数依赖。

范式：指符合某一种级别的关系模式的集合。在设计关系数据库时，根据满足依赖关系要求的不同定义为不同的范式。

规范化：指将一个低一级范式的关系模式，通过模式分解转换为若干个高一级范式的关系模式的集合的过程。

1NF：若关系模式的所有属性都是不可分的基本数据项，则该关系模式属于1NF。

2NF：1NF关系模式如果同时满足每一个非主属性完全函数依赖于码，则该关系模式属于2NF。

3NF：若关系模式的每一个非主属性既不部分依赖于码也不传递依赖于码，则该关系模式属于3NF。

BCNF：若一个关系模式的每一个决定因素都包含码，则该关系模式属于BCNF。

数据库设计：是指对于一个给定的应用环境，构造优化的数据库逻辑模式和物理结构，并据此建立数据库及其应用系统，使之能够有效地存储和管理数据，满足各种用户的应用需求，包括信息管理要求和数据操作要求。

数据库设计的6个基本步骤：需求分析，概念结构设计，逻辑结构设计，物理结构设计，数据库实施，数据库运行和维护。

概念结构设计：指将需求分析得到的用户需求抽象为信息结构即概念模型的过程。也就是通过对用户需求进行综合、归纳与抽象，形成一个独立于具体DBMS的概念模型。

逻辑结构设计：将概念结构模型（基本E-R图）转换为某个DBMS产品所支持的数据模型相符合的逻辑结构，并对其进行优化。

物理结构设计：指为一个给定的逻辑数据模型选取一个最适合应用环境的物理结构的过程。包括设计数据库的存储结构与存取方法。

抽象：指对实际的人、物、事和概念进行人为处理，抽取所关心的共同特性，忽略非本质的细节，并把这些特性用各种概念精确地加以描述，这些概念组成了某种模型。

数据库设计必须遵循结构设计和行为设计相结合的原则。

数据字典主要包括数据项、数据结构、数据流、数据存储和处理过程五个部分。

三种常用抽象方法是分类、聚集和概括。

局部 E-R 图之间的冲突主要表现在属性冲突、命名冲突和结构冲突三个方面。

数据库常用的存取方法包括索引方法、聚簇方法和 HASH方法三种。

确定数据存放位置和存储结构需要考虑的因素主要有： 存取时间、 存储空间利用率和维护代价等。



####  第一范式（1NF）无重复的列

第一范式（1NF）中数据库表的每一列都是不可分割的基本数据项。同一列中不能有多个值，即实体中的某个属性不能有多个值或者不能有重复的属性。 简而言之，第一范式就是无重复的列。

在任何一个关系数据库中，第一范式（1NF）是对关系模式的基本要求，不满足第一范式（1NF）的数据库就不是关系数据库。

#### 第二范式（2NF）属性完全依赖于主键[消除部分子函数依赖]

满足第二范式（2NF）必须先满足第一范式（1NF）。

第二范式（2NF）要求数据库表中的每个实例或行必须可以被惟一地区分。为实现区分通常需要为表加上一个列，以存储各个实例的惟一标识。 

  **第二范式（2NF）要求实体的属性完全依赖于主关键字。所谓完全依赖是指不能存在仅依赖主关键字一部分的属性**，如果存在，那么这个属性和主关键字的这一部分应该分离出来形成一个新的实体，新实体与原实体之间是一对多的关系。为实现区分通常需要为表加上一个列，以存储各个实例的惟一标识。简而言之，第二范式就是属性完全依赖于主键。

#### 第三范式（3NF）属性不依赖于其它非主属性[消除传递依赖]

满足第三范式（3NF）必须先满足第二范式（2NF）。简而言之，第三范式（3NF）要求一个数据库表中不包含已在其它表中已包含的非主关键字信息。

例如，存在一个部门信息表，其中每个部门有部门编号（dept_id）、部门名称、部门简介等信息。那么在的员工信息表中列出部门编号后就不能再将部门名称、部门简介等与部门有关的信息再加入员工信息表中。如果不存在部门信息表，则根据第三范式（3NF）也应该构建它，否则就会有大量的数据冗余。简而言之，第三范式就是属性不依赖于其它非主属性。

#### 具体实例剖析

      下面列举一个学校的学生系统的实例，以示几个范式的应用。
    
       在设计数据库表结构之前，我们先确定一下要设计的内容包括那些。学号、学生姓名、年龄、性别、课程、课程学分、系别、学科成绩，系办地址、系办电话等信息。为了简单我们暂时只考虑这些字段信息。我们对于这些信息，说关心的问题有如下几个方面。
    
       1）学生有那些基本信息 
       2）学生选了那些课，成绩是什么 
       3）每个课的学分是多少 
       4）学生属于那个系，系的基本信息是什么。
    
       首先第一范式（1NF）：数据库表中的字段都是单一属性的，不可再分。这个单一属性由基本类型构成，包括整型、实数、字符型、逻辑型、日期型等。在当前的任何关系数据库管理系统（DBMS）中，不允许你把数据库表的一列再分成二列或多列，因此做出的都是符合第一范式的数据库。 
    
       我们再考虑第二范式，把所有这些信息放到一个表中(学号，学生姓名、年龄、性别、课程、课程学分、系别、学科成绩，系办地址、系办电话)下面存在如下的依赖关系。 
       1）（学号）→ (姓名, 年龄，性别，系别，系办地址、系办电话) 
       2） (课程名称) → (学分) 
       3）（学号，课程）→ (学科成绩)

根据依赖关系我们可以把选课关系表SelectCourse改为如下三个表： 

       学生：Student(学号，姓名, 年龄，性别，系别，系办地址、系办电话)； 
       课程：Course(课程名称, 学分)； 
       选课关系：SelectCourse(学号, 课程名称, 成绩)。
    
       事实上，对照第二范式的要求，这就是满足第二范式的数据库表，若不满足第二范式，会产生如下问题 
数据冗余： 同一门课程由n个学生选修，"学分"就重复n-1次；同一个学生选修了m门课程，姓名和年龄就重复了m-1次。

更新异常： 若调整了某门课程的学分，数据表中所有行的"学分"值都要更新，否则会出现同一门课程学分不同的情况。 假设要开设一门新的课程，暂时还没有人选修。这样，由于还没有"学号"关键字，课程名称和学分也无法记录入数据库。

删除异常 ： 假设一批学生已经完成课程的选修，这些选修记录就应该从数据库表中删除。但是，与此同时，课程名称和学分信息也被删除了。很显然，这也会导致插入异常。

       我们再考虑如何将其改成满足第三范式的数据库表，接着看上面的学生表Student(学号，姓名, 年龄，性别，系别，系办地址、系办电话)，关键字为单一关键字"学号"，因为存在如下决定关系：
    
      （学号）→ (姓名, 年龄，性别，系别，系办地址、系办电话) 
但是还存在下面的决定关系 
       (学号) → (所在学院)→(学院地点, 学院电话) 
        即存在非关键字段"学院地点"、"学院电话"对关键字段"学号"的传递函数依赖。 
       它也会存在数据冗余、更新异常、插入异常和删除异常的情况（这里就不具体分析了，参照第二范式中的分析）。根据第三范式把学生关系表分为如下两个表就可以满足第三范式了：

       学生：(学号, 姓名, 年龄, 性别，系别)； 
       系别：(系别, 系办地址、系办电话)。



### 实战演练

下面为一个例子，通过它我们应该能很好地掌握以上关键词的使用方法。

```sql
create table student(
	Sid int,
	Sname varchar(10),
	Sage int,
	Ssex varchar(5) not null,
	primary key(Sid)
);

create table course(
	Cid int,
	Cname varchar(10),
	Tid int,
	primary key(Cid),
	constraint fk_tid foreign key(Tid) references teacher(Tid)
);

create table score(
	Sid int,
	Cid int,
	sc int,
	primary key(Sid, Cid),
	constraint fk_sid foreign key(Sid) references student(Sid),
	constraint fk_cid foreign key(Cid) references course(Cid)
);

create table teacher(
	Tid int,
	Tname varchar(10),
	primary key(Tid)
);

insert into student(Sid, Sname, Sage, Ssex) values(1, '张无忌', 18, '男');
insert into student(Sid, Sname, Sage, Ssex) values(2, '赵敏', 17, '女');
insert into student(Sid, Sname, Sage, Ssex) values(3, '郭靖', 19, '男');
insert into student(Sid, Sname, Sage, Ssex) values(4, '黄蓉', 16, '女');
insert into student(Sid, Sname, Sage, Ssex) values(5, '段誉', 20, '男');
insert into student(Sid, Sname, Sage, Ssex) values(6, '周芷若', 17, '女');
insert into student(Sid, Sname, Sage, Ssex) values(7, '虚竹', 21, '男');
insert into student(Sid, Sname, Sage, Ssex) values(8, '李秋水', 18, '女');
insert into student(Sid, Sname, Sage, Ssex) values(9, '韦小宝', 20, '男');

insert into teacher(Tid, Tname) values(100, '丘处机');
insert into teacher(Tid, Tname) values(101, '风清扬');
insert into teacher(Tid, Tname) values(102, '任我行');
insert into teacher(Tid, Tname) values(103, '黄药师');


insert into course(Cid, Cname, Tid) values(200, '语文', 100);
insert into course(Cid, Cname, Tid) values(201, '政治', 100);
insert into course(Cid, Cname, Tid) values(202, '数学', 101);
insert into course(Cid, Cname, Tid) values(203, '物理', 101);
insert into course(Cid, Cname, Tid) values(204, '化学', 103);
insert into course(Cid, Cname, Tid) values(205, '生物', 103);
insert into course(Cid, Cname, Tid) values(206, '地理', 102);


insert into score(Sid, Cid, sc) values(1, 200, 75);
insert into score(Sid, Cid, sc) values(1, 201, 66);
insert into score(Sid, Cid, sc) values(1, 202, 85);
insert into score(Sid, Cid, sc) values(1, 203, 80);
insert into score(Sid, Cid, sc) values(1, 204, 93);
insert into score(Sid, Cid, sc) values(1, 205, 98);
insert into score(Sid, Cid, sc) values(1, 206, 89);
insert into score(Sid, Cid, sc) values(2, 200, 95);
insert into score(Sid, Cid, sc) values(2, 201, 60);
insert into score(Sid, Cid, sc) values(2, 202, 93);
insert into score(Sid, Cid, sc) values(2, 203, 77);
insert into score(Sid, Cid, sc) values(2, 204, 62);
insert into score(Sid, Cid, sc) values(2, 205, 88);
insert into score(Sid, Cid, sc) values(2, 206, 90);
insert into score(Sid, Cid, sc) values(3, 200, 62);
insert into score(Sid, Cid, sc) values(3, 202, 59);
insert into score(Sid, Cid, sc) values(3, 203, 60);
insert into score(Sid, Cid, sc) values(3, 204, 48);
insert into score(Sid, Cid, sc) values(4, 200, 91);
insert into score(Sid, Cid, sc) values(4, 201, 80);
insert into score(Sid, Cid, sc) values(4, 202, 99);
insert into score(Sid, Cid, sc) values(4, 203, 89);
insert into score(Sid, Cid, sc) values(4, 204, 88);
insert into score(Sid, Cid, sc) values(4, 205, 96);
insert into score(Sid, Cid, sc) values(4, 206, 90);
insert into score(Sid, Cid, sc) values(5, 203, 75);
insert into score(Sid, Cid, sc) values(5, 204, 68);
insert into score(Sid, Cid, sc) values(5, 205, 80);
insert into score(Sid, Cid, sc) values(5, 206, 78);
insert into score(Sid, Cid, sc) values(6, 200, 93);
insert into score(Sid, Cid, sc) values(6, 201, 80);
insert into score(Sid, Cid, sc) values(6, 202, 91);
insert into score(Sid, Cid, sc) values(6, 203, 77);
insert into score(Sid, Cid, sc) values(7, 202, 60);
insert into score(Sid, Cid, sc) values(7, 203, 71);
insert into score(Sid, Cid, sc) values(7, 204, 83);
insert into score(Sid, Cid, sc) values(7, 205, 89);
insert into score(Sid, Cid, sc) values(8, 200, 75);
insert into score(Sid, Cid, sc) values(8, 201, 66);
insert into score(Sid, Cid, sc) values(8, 202, 78);
insert into score(Sid, Cid, sc) values(8, 203, 80);
insert into score(Sid, Cid, sc) values(8, 204, 73);
insert into score(Sid, Cid, sc) values(8, 205, 82);
insert into score(Sid, Cid, sc) values(8, 206, 65);
insert into score(Sid, Cid, sc) values(9, 202, 56);
insert into score(Sid, Cid, sc) values(9, 203, 48);
insert into score(Sid, Cid, sc) values(9, 204, 55);
insert into score(Sid, Cid, sc) values(9, 205, 66);
```



**1、查询课程ID200比课程ID202成绩高的所有学生学号；**

```sql
select a.sid from 
(select Sid, sc from score where Cid = 200) a,
(select Sid, sc from score where Cid = 202) b 
where a.sc > b.sc and a.Sid = b.Sid;
```



**2、查询平均成绩大于60分的同学的学号和平均成绩；**

```sql
select Sid, avg(sc) from score group by Sid having avg(sc) > 60;
```



**3、查询所有同学的学号、姓名、选课数、总成绩；**

```sql
select student.Sid, student.Sname, count(score.Cid), sum(sc)
from student left outer join score on student.Sid = score.Sid
group by student.Sid, Sname;
```



**4、查询姓黄的老师的个数；**

```sql
select count(distinct(Tname))
from teacher
where Tname like '黄%';
```



**5、查询没学过黄药师老师课的同学的学号、姓名；**

```sql
select student.Sid, student.Sname
from student
where Sid not in (
	select distinct(score.Sid) 
	from score, course, teacher 
	where score.Cid = course.Cid and teacher.Tid = course.Tid and teacher.Tname = '黄药师'
	);
```



**6、查询学过课程ID203并且也学过课程ID204的同学的学号、姓名；**

```sql
select student.Sid, student.Sname
from student, score
where student.Sid = score.Sid and score.Cid = 200 and exists(
	select * from score
	where score.Cid = 201
	);
```



**7、查询学过风清扬老师所教的所有课的同学的学号、姓名；**

```sql
select Sid, Sname
from student
where Sid in (
	select Sid from score, course, teacher
	where score.Cid = course.Cid and teacher.Tid = course.Tid and teacher.Tname = '风清扬'
	group by Sid 
	having count(score.Cid) = (
		select count(Cid) 
		from course, teacher 
		where teacher.Tid = course.Tid and Tname = '风清扬'
		)
	);
```



**8、查询所有课程成绩小于60分的同学的学号、姓名；**

```sql
select Sid, Sname
from student
where Sid not in (
	select student.Sid 
	from student, score 
	where student.Sid = score.Sid and sc > 60
);
```



**9、查询没有学全所有课的同学的学号、姓名；**

```sql
select Student.S#,Student.Sname
from Student,SC
where Student.S#=SC.S#
group by Student.S#,Student.Sname having count(C#) <(select count(C#) from Course);
```



**10、查询至少有一门课与学号ID4所学相同的所有同学的学号和姓名；**

有问题

```sql
select S#,Sname
from Student,SC
where Student.S#=SC.S# and C# in （select C# from SC where S#='1001'）;
```



**11、删除学习“叶平”老师课的SC表记录；**

暂时不做

```sql
Delect SC
from course ,Teacher
where Course.C#=SC.C# and Course.T#= Teacher.T# and Tname='叶平';
```



**12、查询各科成绩最高和最低的分：以如下形式显示：课程ID，最高分，最低分**



```sql
SELECT L.C# 课程ID,L.score 最高分,R.score 最低分
FROM SC L ,SC R
WHERE L.C# = R.C#
and
L.score = (SELECT MAX(IL.score)
FROM SC IL,Student IM
WHERE IL.C# = L.C# and IM.S#=IL.S#
GROUP BY IL.C#)
and
R.Score = (SELECT MIN(IR.score)
FROM SC IR
WHERE IR.C# = R.C#
GROUP BY IR.C# );
```



**13、查询学生平均成绩及其名次**

```sql
SELECT 1+(SELECT COUNT( distinct 平均成绩)
FROM (SELECT S#,AVG(score) 平均成绩
FROM SC
GROUP BY S# ) T1
WHERE 平均成绩 > T2.平均成绩) 名次, S# 学生学号,平均成绩
FROM (SELECT S#,AVG(score) 平均成绩 FROM SC GROUP BY S# ) T2
ORDER BY 平均成绩 desc;
```



**14、查询各科成绩前三名的记录:(不考虑成绩并列情况)**

```sql
SELECT t1.S# as 学生ID,t1.C# as 课程ID,Score as 分数
FROM SC t1
WHERE score IN (SELECT TOP 3 score
FROM SC
WHERE t1.C#= C#
ORDER BY score DESC)
ORDER BY t1.C#;
```



**15、查询每门功成绩最好的前两名**

```sql
SELECT t1.S# as 学生ID,t1.C# as 课程ID,Score as 分数
FROM SC t1
WHERE score IN (SELECT TOP 2 score
FROM SC
WHERE t1.C#= C#
ORDER BY score DESC )
ORDER BY t1.C#;
```




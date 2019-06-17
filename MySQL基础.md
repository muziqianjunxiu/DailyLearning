# SQL语言（结构化查询语言）

​		DDL语句	数据库定义语言：数据库、表、视图、索引、存储过程、函数、CREATE	DROP	ALERT		//开发人员

​		DML语句	数据库操纵语言：插入数据INSERT、删除数据DELETE、更新数据UPDATE		//开发人员

​		DQL语句	数据库查询语言：查询数据SELECT

​		DCL语句	数据库控制语言：例如控制用户访问权限GRANT、REVOKE

# 系统数据库

## information_schema

​		虚拟库，主要存储了系统中一些数据库对选哪个的信息，如用户表信息，列信息，权限信息，字符信息等

## performance_schema

​		主要存储数据库服务器的性能参数，性能调优

## **mysql**

​		授权库，主要存储系统用户的权限信息

## sys

​		主要存储数据库服务器的性能参数

## 创建自己的数据库

命令不区分大小写，数据库名字区分大小写

`select database( )；`			查看当前所处库，括号内无参数

`create database mydb；`		创建数据库mydb

`show databases；`						查看所有库

`use  mydb`								使用mydb数据库

`drop  database  mydb；`			删除数据库mydb

`help  drop   /  help  drop database`		可以查看drop命令/drop database命令的帮助

`show tables;`							查看库中表	

`desc  mytable`							查看mytable这个表结构

`show  databases；`				显示所有库

`create database  my_db`		创建库my_db

`drop  database  my_db`		删除库my_db

`create	table	表名（属性名   数据类型  [完整性约束]，属性名  数据类型  [完整性约束]）；`	创建表

​		约束条件：

​				`PRIMARY_KEY`		标识该属性为该表的主键，可以唯一地标识对应的记录

​				`FOREIGN_KEY`		标识该属性为该表的外键，与某表的主键关联

​				`NOT NULL`		标识该属性不能为空

​				`UNIQUE`			标识该属性的值是惟一的

​				`AUTO_INCREMENT`			标识该属性的值自动增加

​				`DEFAULT`			为该属性设置默认值

`eg：`

```
create table t_booktype(
	id 	int primary key  auto_increment,
	booktypename	varchar(20),
	booktypeDesc	varchar(20)
);
create table t_book(
	id	int	primary key auto  increment,
	bookName varchar(20),
	author	varchar(10),
	price	decimal(6,2),
	booktypeid	int,
	constraint	`fk` foreign key (`booktypeid`)	references	`t_booktype` (`id`)
);
```



`describe/desc  my_table`		查看表my_table

`show	create	table 	my_table`		查看表my_table

`alter  table  my_table_old  rename  my_table_new;`		将表名my_table_old改为my_table_new

`alter	table  my_table  change  旧字段名   新字段名   新数据类型`		修改字段

`alter  table  my_table  add  字段名1  数据类型  [约束] [FIRST | AFTER  字段名2]`	增加字段

`alter  table  my_table  drop  字段名`			删除字段 

`drop  table  my_table`		删除表my_table

# MySQL数据类型

## 数值类型

整数类型	TINYINT  SMALLINT	MEDIUMINT	INT	BIGINT

浮点型	FLOAT	DOUBLE

定点数类型	DEC

位类型	BIT

## 字符串类型

CHAR类型	CHAR	VARCHAR

TEXT系列	TINYTEXT	TEXT	MEDIUNTEXT	LONGTEXT

BLOB系列	TINYBLOB	BLOB	MEDIUMBLOB	LONGBLOB

BINARY系列	BINARY	VARBINARY

枚举系列	ENUM

集合类型	SET		

## 时间和日期类型

DATA	TIME	DATATIME	TIMESTAMP	YEAR

# 显示表的创建操作细节：

​		`show  create table  my_tab\G`

​		`show  create  table  my_tab\g  = show create table  my_tab;` 

# 创建外键

`create table employees(`

`name varchar(50)  not null,`

`mail  varchar(20) not null,`

`primary key(name)`

`);`

`create tabel payroll(`

`id  int not null  auto_increment,`

`name varchar(20),`

`payroll float(10,2) not null,`

`primary key(id),`

`foreign key(name)  references employees(name) on update cascade on delete cascade`		同步更新  同步删除

`);`

# MySQL表操作  DDL

## 修改表  alter  table 

### 修改表名rename

​	`alter  table  t1  rename  t_new;`

### 增加字段add

`alter table  t1  add  字段名   数据类型   完整性约束条件，add  字段名  数据类型  完整性约束条件`

`alter  table  t1  add  字段名  数据类型  完整性约束条件   first；`//添加到最前面

`alter  table   t1  add   字段名  数据类型  完整性约束条件  after  字段名;`

### 删除字段drop

`alter   table  t1  drop  字段名；`

### 修改字段modify

`alter  table  t1  modify  字段名  数据类型  完整性约束条件；`//注意保留原有的约束条件

### 增加约束auto_increment

针对已有的主键添加auto_increment

`alter  table  t1  modify  id  int  not null primary key  auto_increment;`//错误，该字段已经是primary key

`alter  table  t1  modify  id  int  not null  auto_increment;`//正确

### 增加自动增长

​	`alter table  t1 modify  id  int   not null auto_increment;`

### 增加复合主键

​	`alter  table  t1  add  peimary  key (id,name);`

### 增加主键

​	`alter table  t1  add primary key (id);`

### 删除主键    [`promary key   auto_increment`]

​		先要删除自增约束

​			`alter table t1  modify   id  int  not null;`

​		再删除主键

​			`alter table t1 drop  primary  key;`

## 复制表  create  table

​			复制表结构+记录（key不会复制：主键  外键  索引）

​			`create table t_new select * from t1;`

​			只复制表结构

​			`create table t_new select * from t1 where 1=2;`		//条件为假  查不到任何记录

​			复制表结构，包括key

​			`create  table t_new  like  t1;`

## 删除表  drop  table 

​		`drop  table  t1;`

# MySQL数据操作   DML

## 插入数据   insert  into

### 插入完整数据

`insert  into  t1  values  (值1，值2...)，(值1，值2...);`

### 指定字段插入数据

`insert  into  t1 (id，name...)  values  (值1 ，值2...)，(值1，值2...)；`

### 插入查询结果  ---从另一表取数据插入

`insert  into  t1 (id, name...)  select (id,name..) from t2  where 约束条件;`

## 修改数据update

`update  t1  set  字段1=值1，字段2=值2  where 约束条件；`

eg.   

`update mysql.user  set  authentication_string = password( ' mimashi! ' )  where  user='root'  and  host = 'localhost';`   //修改root密码

`flush  privileges;`  //刷新授权表

## 删除数据delete

`delete from  t1  where  约束条件;`

# MySQL查询操作  DQL

## 单表查询

### 简单查询

`select * from  t1;`

`select  id  name  from  t1;`

### 避免重复  DISTINCT

`select  name  form  t1;`

`select  distinct  post  from  t1;`

只能用于单一字段去重

### 通过四则运算查询

`select  name , salary , salary*14  from  t1;`

`select name ,salary,salary*14 as Annual_salary from t1;`   //使用别名

`select name ,salary,salary*14  Annual_salary from t1;`  //可以不加as

### 定义显示格式

concat( )函数用于连接字符串

`select concat(name,'annual salary:',salary*14)  as  annual_salary  from t1;`

## 单条件查询

### 单条件查询

`select  name,id  from t1  where post='hr';`

### 多条件查询

`select name,salary from t1 where post='hr' and salary >1000;`

### 关键字 BETWEEN  AND

`select name ,salary from t1 where salary between 5000 and 6000;`

`select name ,salary from t1 where salary not between 5000 and 6000;`

### 关键字   IS NULL

`select name ,salsry from t1 where salary is null;`

`select name ,salsry from t1 where salary is not null;`

### 关键字  IN  集合查询

`select name ,salary from t1  where salary=1000  or  salary=2000  or  salary =3000;`

`select name ,salary from t1 where salary in (1000,2000,3000);`

`select name ,salary from t1 where salary not in (1000,2000,3000);`

### 关键字 LIKE  模糊查询  及通配符

通配符  %   任意多个字符

`select  *  from  t1  where  name  like 'wang%';`

通配符  _    任意一个字符，需要匹配几个字符就放几个_

`select  *  from  t1  where  name like  'wang_';`

## 排序查询

### 按单列排序

`select  *  from  t1  order  by  id;`

`select  *  from  t1  order  by  id  ASC;`

`select  *  from  t1  order  by  id  DESC;`

### 按多列排序

`select * from t1 order by id  ASC, salary DESC;`

## 限制查询的记录数   LIMIT

`select * from t1 order  by salary  DESC  limit  5;`		//默认初始位置为0

`select * from t1 order  by salary  DESC  limit  0,5;`  

`select * from t1 order  by salary  DESC  limit  3,5;` //从第四条开始，共显示5条

## 使用集合函数查询

`select  count(*)  from  t1;`   //得到查询结果的数量

`select count(*) from t1 where id=1;`

`select max(salary)  from t1 ;`  //得到查询结果的最大值

`select  min(salary) from  t1;`  //得到查询结果的最小值

`select  avg(salary)  from  t1;`  //得到查询结果的平均值

`select sum(salary) from t1;`  //得到查询结果的总和

`select sum(salary)  from t1  where  id=1;`  

eg.

​	`select  name  from  t1  where  salary=( select max(salary) from t1 );`

## 分组查询

group  by  和  group_concat（）函数一起使用

`select id ,group_concat(name)  from t1  group by  id;`

`select id ,group_concat(name) as new_alias from t1 group by  id;` //按相同的id进行分组，并将name字段括成一组，以别名new_alias字段名展现

group by  和 集合函数一起使用

`select  id ,sum(salary)  from t1  group by id;`

##  使用正则查询

`select  * from t1  where  name REGEXP  '^ali';`

`select * from t1  where  name REGEXP 'yun$';`

`select  * from t1  where name REGEXP 'm{2}';`

## 对字符串匹配的方式

`where  name = 'tom';`			精确匹配

`where  name like  'to%';`		模糊匹配

`where  name like 'to_';`   模糊匹配

`where name REGEXP 'yun$';`  正则匹配

## 多表查询

如果字段唯一，则不用加表名前缀。

###   内连接  

只连接匹配的行

`select t1.name,t1.age,t1.id ,t2.name  from t1,t2 where  t1.id=t2.id;`

### 外连接

 外连接之左连接：会显示左边表内(t1)所有的值，不论在右边表内匹不匹配

`select 字段列表  from t1  left  join  t2  ON  ti.name=t2.name；`

 外连接之右连接：会显示右边表内(t2)所有的值，不论在左边表内匹不匹配

`select 字段列表  from t1  left  join  t2  ON  ti.name=t2.name；`

全外连接：包含左右两个表的全部行

`select 字段列表  from t1  full  join  t2  ON  ti.name=t2.name；`

### 复合条件

以内连接的方式查询employee和department表，并且employee中的age字段值大于25：

`select emp_id,emp_name,age,dept_name from employee,department where employee.dept_id = department.dept_id  AND age>25;`

以内连接的方式查询employee和department表，并且以age字段升序方式显示：

`select emp_id,emp_name,age,dept_name from employee,department where employee.dept_id = department.depy_id ORDER BY age asc;`

### 子查询

将一个查询语句嵌套在另一个查询语句中。内层查询语句的查询结果，可以为外层查询语句提供查询条件。子查询中可以包含：IN,NOT IN, ANY, ALL, EXIST,  NOT EXIST等关键字。还可以包含比较运算符：= 、!=、>、<等。

#### 带IN关键字的子查询

查询employee表，但dept_id必须在department表中出现过：

`select * from employee where depy_id IN (select dept_id from department);`

#### 带比较运算符的子查询

查询年龄大于等于25的员工所在部门：

`select dept_id,dept_name from department where dept_id IN (select DISTINCT dept_id from employee where age>=25 );`

#### 带EXIST关键字的子查询

EXIST表示存在，在使用EXIST关键字时，内层查询语句不返回查询的记录，而是返回一个真假值。True或者False。当返回True时，外层查询语句将进行查询，返回False不查询。

`select * from employee where EXIST (select * from department where depy_id=200);`

# MySQL索引

## 索引简介

索引在MySQL中也叫键，索引优化是对查询优化最有效的手手段了，能将查询性能提升好几个数量级。索引相当于字典的音序表。主键 外键  也都是索引。KEY  或  INDEX。

索引的分类：普通索引   唯一索引   全文索引  单列索引   多列索引   空间索引

## 创建存储过程，实现批量插入数据

`mysql> use school`												//school表使用							

`mysql> delimiter  $$`   								//将mysql语句执行的标志''；''改为$$。

`mysql> create procedure autoinsert()`				//创建存储过程

​	`-> BEGIN`

​	`-> declare  i  int  default  1;`          //声明变量i 为int   初始值为1

​	`-> while (i<20000) do`

​	`-> 		insert  into school.t1 values(i,'abc');` //循环插两个字段值进t1表

​	`-> 		set i=i+1;`

​	`-> end while;`

​	`-> END$$`

`mysql> delimiter  ;`		//将mysql语句执行的标志改回“；”。使用`\d  ;` 也可以。

--------------------------------------------------------------------------------------------------------------

## 查看存储过程

`show procedure  status \G`       查看存储过程内容

`show create  procedure  autoinsert\G`		查看名为autoinsert存储过程的详细内容

## 调用存储过程

`call  autoinsert();`   调用存储过程autoinsert，即插入20000条数据。

## 创建索引

### 创建表时创建

`create  table  表名（`

​		`字段名1  数据类型 [完整性约束条件...]，`

​		`字段名2  数据类型 [完整性约束条件...]，`

​		`[UNIQUE | FULLTEXT | SPATIAL]  INDEX | KEY`

​		`[索引名 ] （字段名 [(长度)]  [ ASC |DESC]`

`);`

创建普通索引示例：

`create  table  department(`

​		`dept_id  int,`

​		`dept_name  varchar(50),`

​		`comment varchar(50),`

​		`INDEX  (dept_name)`

`) ;`

创建唯一索引示例：

`create  table  department(`

​		`dept_id  int,`

​		`dept_name  varchar(50),`

​		`comment varchar(50),`

​		`UNIQUE INDEX  (dept_name)`

`) ;`

创建全文索引示例：

`create  table  department(`

​		`dept_id  int,`

​		`dept_name  varchar(50),`

​		`comment varchar(50),`

​		`FULLTEXT INDEX  (dept_name)`

`) ;`

创建多列索引示例：

`create  table  department(`

​		`dept_id  int,`

​		`dept_name  varchar(50),`

​		`comment varchar(50),`

​		`INDEX  (dept_name,comment)`

`) ;`

### CREATE 在已存在表上创建索引  (常用)

`create  [UNIQUE|FULLTEXT|SPACIAL]  INDEX  索引名  ON  表名（字段名 [(长度)]  [ASC | DESC]）；`

创建普通索引示例：

`create  index  index_dept_name  on department(dept_name);`

创建唯一索引示例：

`create  fulltext index  index_dept_name  on department(dept_name);`

创建多列索引示例：

`create  index  index_dept_name_comment  on  department(dept_name,comment);`

### ALTER TABLE 在已存在的表上创建索引

 `ALTER TABLE 表名 ADD [UNIQUE | FULLTEXT | SPATIAL] INDEX  索引名（字段名 [ （长度）]  [ASC |DESC ];`

创建普通索引示例：

`alter table department ADD  INDEX index_dept_name (dept_name);`

创建唯一索引示例：

`alter  table  department ADD UNIQUE INDEX index_dept_name (dept_name);`

创建全文索引示例：

`alter table department ADD FULLTEXT INDEX index_dept_name (dept_name);`

创建多列索引示例：

`alter table department ADD INDEX index_dept_name_comment (dept_name,comment);`

## 查看索引

`show  create  table  表名\G`

`explain  select * from t1 where id=100000\G`      //explain 的作用是查看**查询优化器**如何执行查询 ,而非执行查询

## 删除索引

`show  create  table  表名\G`

`drop  INDEX  索引名  ON 表名；`

## TIPS：

如果已建索引的数据表要重新插入大量数据，最好先将索引删除，再插入数据，然后重建索引。

\G后面不加；

# MySQL视图

## 视图简介

视图是一个虚拟表，其内容由查询定义。同真实的表一样，视图包含一系列带有名称的列和行数据。但是视图并不以存储的数据值集形式存在于数据库中。行和列数据由定义视图的查询所引用的表，并且在引用视图时动态生成。对其中所引用的基础表来说，MySQL视图的作用类似于筛选。定义视图的筛选可以来自当前或其他数据库的一个活多个表，或者其他视图。通过视图进行查询没有任何限制，通过他们进行数据修改时的限制也很少。视图是存储在数据库中的SQL查询语句，他主要出于两种原因：安全原因：视图可以隐藏一些数据。另一原因可以使复杂查询易于理解和使用。

## 创建视图

语法一

`create [ALGORITHM = { UNDEFINED | MERGE | TEMPLATE } ]  VIEW 视图名 [（字段1，字段2...）]  AS   select 语句  [ WITH [ CASCADED | LOCAL ]  CHECK OPTION ];`

语法二

`create  view  视图名  AS  select语句 ;`

单表创建视图示例：

`create view my_view AS select user,host,authentication_string from mysql.user;`

多表创建视图示例：

`create  view my_view AS select  product.name,product.price,purchase.quantity,product.price*purchase.quantity as  total_val  from product,purchase  where product.name=purchase.name;`

## 查看视图

DESCRIBE

​		`desc my_view;`   查看视图结构

`SHOW TABLES;`		查看视图名

`SHOW TABLE STATUS`  	

​		`show table status from mysql \G` 查看数据库MySQL中视图及所有表详细信息

​		`show table status from mysql like 'my_view' \G;`  查看数据库MySQL中视图名my_view详细信息

`SHOW REATE VIEW`

​		`show  create view my_view\G`  查看视图定义信息

## 删除视图

`drop  view  视图名；`

# MySQL触发器 Triggers

## 触发器简介

触发器是一个特殊的存储过程，它的执行不是由程序调用，也不是手工启动，而是由事件来处发，比如当对一个表进行操作（insert  ，  delete  ，  update ）时就会激活他执行。触发器经常用语加强数据的完整性约束和业务规则等。

例如，当学生表增加了一个学生的信息后，学生的总数就会改变，因此可以针对学生表创建一个触发器，每次增加一个学生记录时，就执行一次学生总数的计算操作，从而保证学生总数与记录数的一致性。

## 创建Triggers

`create triggers  触发器名称  before|after 触发事件  on  表名 for  each  row`

`BEGIN`

​		`触发器程序体；`

`END`

<触发器名称>		最多64个字符

{BEFORE | AFTER}  触发器时期

{INSERT | UPDATE | DELETE }	触发的事件

ON <表名称>		标识建立触发器的表名，即在哪张表上建立触发器

FOR EACH ROW		触发器的执行间隔，每隔一行执行一次动作，而不是对整个表执行一次

<触发器程序体>		要触发的sql语句：可用顺序  判断  循环等语句实现一般程序需要的逻辑功能

**1.创建表**

`mysql->  create table student(`

​		`->	id  int  unsinged  auto_increment  primary key not null,`

​		`->    name  varchar(50)`

​		`->  );`

`mysql->  create table student_total( total  int );`

`mysql->   insert into student_total values(0);`

**2.创建触发器student_insert_trigger**

`mysql->  \d  $$`

`mysql->  create trigger student_insert_triggers after insert  on  student for  each row`

​		`->  BEGIN`

​		`->  	update student_total set total=total+1;`

​		`->  END$$`

`mysql->  \d ;`

**3.创建触发器student_delete_trigger**

`mysql-> delimiter  $$`

`mysql-> create  trigger student_delete_trigger after delete on student for each row`

​		  `->  BEGIN`

​		  `->  	  update student_total  set  total=total-1;`

​		  `->  END$$`

`mysql-> \d  ;`

## 查看触发器

1.通过  SHOW  TRIGGERS  语句查看

`show  triggers\G`

2.通过系统表  triggers  查看

`use  information_schema`

`select * from triggers\G`

`select * from triggers where TRIGGER_NAME ='触发器名称'\G`

## 删除触发器

通过  DROP  TRIGGERS  语句删除

drop trigger  触发器名称

## 触发器案例

1.创建表  t1

`drop table if exist t1;`

`create table t1(`

​		`id int primary key auto_increment,`

​		`name carchar(50),`

​		`sex enum('m','f'),`

​		`age int`

`);`

2.创建表   t2

`drop table if exist t2;`

`create table t2(`

​		`id int primary key auto_increment,`

​		`name carchar(50),`

​		`salary double(10,2)`

`);`

3.创建触发器  t1_after_delete_trigger

作用：t1表删除记录后，自动将t2表中对应记录删除

`\d $$`

`create trigger t1_after_delete_trigger after delete on t1 for  each row`

`BEGIN`

​		//`delete from t2 where name=old.name;`//有可能名字相同，这样t2表内重名的数据就被全部删光了，所以删除操作应只对主键进行操作！！

​		`delete from t2 where id=old.id;`//删除必须通过主键进行操作

`END$$`

4.创建触发器   t1_afte_update_trigger

作用:当t1更新后，自动更新t2

`create trigger t1_after_update_trigger after update on t1 for each row`

`BEGIN`

​		`update t2 set name=old.name where id=old.id;`//更新操作也必须通过主键

`END$$`

5.创建触发器   t1_after_insert_trigger

作用：t1表插入数据后，t2表自动插入相应数据

`create trigger t1_after_insert_trigger after insert on t1 for each row`

`BEGIN`

​		`insert into t2(name,salary) values(new.name,5000);`

`END$$`

`\d ;`

-----------------

# MySQL存储过程  与函数

------------

## 存储过程概述

存储过程和函数是实现经过编译并存储在数据库中的一段SQL语句的集合。存储过程与函数的区别：

​		函数必须有返回值，通过return语句返回。而存储过程是通过定义参数类型将需要的值传出。

​		存储过程的参数可以使IN，OUT，INOUT类型 ，而函数的参数只能是IN。

优点：

​			存储过程只在创建时进行编译；而SQL语句每执行一次就编译一次，所以使用存储过程可以提高数据库执行速度。简化复杂操作，结合事务一起封装。复用性好。安全性高，可指定存储过程的使用权。

函数优点：

​			函数调用方式简单，可以和其他语句结合，灵活使用。而存储过程的调用相对来说是固定化的，给存储过程一些数据，进行加工。从某种程度上看，存储过程和函数是相似的。

说明：并发量少的情况下，很少使用存储过程。并发量高的情况下，为了提高效率，用存储过程较多。

-------

## 存储过程创建

创建存储过程语法：

`create procedure  sp_name(参数列表)  [特性...] 过程体`

存储过程的参数形式：[IN  | OUT  | INOUT ]  参数名 类型

​		IN 	输入参数		OUT		输出参数		INOUT	输入输出参数

-------------------

```mysql
delimiter  $$
create  procedure  过程名(形式参数列表)
BEGIN
	SQL语句
END$$
delimiter  ;
```

## 存储过程调用

​		`	call  存储过程名（实参列表）`

----

## 存储过程参数类型

1. NONE

```mysql
\d $$
create procedure  p1()
begin
	select count(*) from mysql.user;   //统计user用户数
end$$
\d ;
call p1();  //调用
```

```mysql
create table school.t1(id int ,cc varchar(100));
delimiter $$
create procedure autoinsert1()
BEGIN
declare i int default 1;
while (i<=20000)do
	insert into school.t1  values(i,md5(i));
	set i=i+1;
end while
END$$
delimiter ;
call autoinsert1();   //调用
```

2.IN    只能传入参数

```mysql
create table school.t2(id int ,cc varchar(100));
delimiter $$
create procedure autoinsert2(IN a int)	//传参a，参数类型为int
BEGIN
	declare i int default 1;
	while (i<=a)do
		insert into school.t2  values(i,md5(i));
		set i=i+1;
	end while
END$$
delimiter ;
call autoinsert2(20000);   //调用
```

或者使用变量传参进行调用：

```mysql
set  @num=20;			//定义mysql中的变量，@num为局部变量num，@@global.num为全局变量num，将20赋值给了num。
select  @num;			//查看num的值,相当于echo num
call  autoinsert2(@num);  //将变量作为参数a传进去
```

3.OUT    只能传出参数

```mysql
delimiter $$
create procedure p3(OUT a int)
begin
	select count(*) into a from mysql.user;
end$$
delimiter ;
call p3(@num);
select @num;
```

4.IN 和 OUT 

eg1.统计指定部门的员工数

```mysql
\d  $$
create procedure count_num(IN p1 varchar(50),OUT p2 int)
BEGIN
	select count(*) into p2 from company.employee  where post =p1;
END$$
\d ;
call count_num('hr',@num);
select @num;	//查看结果
```

eg2.统计指定部门工资超过5000的总人数

```mysql
create procedure count_num(IN p1 varchar(50),IN p2 float(10,2),OUT p3 int)
BEGIN
	select count(*) into p3 from company.employee where post=p1 and salary>=p2;
END$$
\d ;
call count_num('hr',5000,@num);
select @num;   //查看结果
```

5.INOUT

```mysql
\d $$
create procedure proce_param_inout(INOUT p1 int)
BEGIN
	if (p1 is not null)then
		set p1=p1+1;
	else
		select 100 into p1;		//或  set p1 =100;
	end if;
END$$
\d ;
call proce_param_inout(@h);
select @h;
```

## 函数创建

create function 函数名(参数列表)  returns  返回值类型   [特性..] 函数体

函数的参数形式：参数名 ，类型

```mysql
delimiter $$
create function 函数名（参数列表）  returns  返回值类型
BEGIN
	有效的sql语句
END$$
delimiter  ；

```

## 函数调用

select  函数名（实参列表）

-----

```mysql
create function my_hello(my_str char(20))  
returns  char(50)
	return  CONCAT('hello,',my_str,'!');

select my_hello('muzi');   //调用，结果为：hello，muzi！
```

```mysql
\d $$
create function name_from_emp(x int)
returns varchar(50)
BEGIN
	return (select name from employee where id=x);
END$$
\d ;
select name_from_emp(5);  //获得id为5的员工姓名
select * from employee where employee.name=	name_from_emp(5);  //获得id为5的员工的所有信息
```

## 存储过程与函数的维护

```mysql
show  create procedure p1\G;
show create function p2\G;
show {procedure |function }  status {like 'pattern'}
drop { procedure |function } {if exist} sp_name
```

## MySQL变量

1.用户变量

​		以@开头，形式为“@变量名”。用户变量跟mysql客户端是绑定的设置的变量支队当前用户使用的客户端生效，当用户断开连接时所有变量会自动释放。

2.全局变量

​		定义时有如下两种形式   `set  GLOBAL  变量名`    或者   `set  @@global.变量名`。对所有客户端生效，但只有具有super权限才可以设置全局变量。

3.会话变量

​		只对连接的客户端生效。

4.局部变量

​		设置并作用于begin...end语句块之间的变量。declare语句专门用于电仪局部变量，而set语句是设置不同类型的变量，包括会话变量和全局变量。

​		语法：`declare  变量名[...]  变量类型  [default 值]`

# MySQL安全机制 DCL

远程连接MySQL

默认情况MySQL不允许远端连接，默认用户信息里的host设置为localhost，只允许本机登陆。

`mysql  -h192.168.1.10 -uroot -p'mimashi123'`

如果此时显示的是error 2003，应该是防火墙没关闭：

```bash
systemctl stop firewalld    		//关闭防火墙
systemctl  disable firewalld  		//设置防火墙开机不启动
systemctl  mask firewalld  			//最狠的，将整个firewall服务屏蔽掉，要想重新开启只能systemctl  unmask firewalld
```

关闭之后，如果显示error 1130， 可能是端口没开，MySQL工作默认端口是3306。

## MySQL权限表

mysql.user

​		全局级别的授权。用户字段，权限字段，安全字段，资源控制字段。

mysql.db

​		数据库级别的授权。用户字段，权限字段。

mysql.tables_priv

​		表级别的授权。

mysql.columns_priv

​		列级别的授权

查看权限：`show  grants\G`

## MySQL用户管理

### 1.登陆和退出mysql

`mysql  -h192.168.5.24 -P 3306 -uroot -p'mimashi123' mysql -e'select user,host from user'`

-h		指定主机名，默认为localhost

-P		MySQL服务器端口，默认为3306

-u		指定用户名，默认为root

-p		指定登录密码，默认为空密码

mysql		此处mysql为指定登陆的数据库

-e		接SQL语句

### 2.创建用户

方法一：CREATE USER 语句创建

​		`create user user1@'localhost' identified by 'mimashi123';`

方法二：GRANT语句创建,常用

​		`grant all on *.* to'user2'@'localhost' identified by 'mimashi123';`

​		//`*.*`表示所有库所有表

### 3.删除用户

方法一：DROP USER 语句

​		`drop user 'user1'@'localhost';`

方法二：DELETE语句

​		`delete from mysql.user  where user='user1'  and host='localhost';`

​		`flush priviliges;`

### 4.修改用户密码

#### root用户修改自己密码

方法一：

​		`mysqladmin -uroot -p'oldpassword' password 'newpassword'`

方法二：

​		`update mysql.user set authentication_string=password('newpassword')  where user='root' and host='localhost';`

​		`flush priviliges;`

方法三：

​		`set  password=password('newpassword');`

#### root用户修改其他用户密码

方法一：

​		`set password for 'user1'@'localhost'=password('newpassword');`

​		`flush priviliges;`

方法二：

​		`update mysql.user set authentication_string=password('newpassword') where user='user1'  and host='localhost';`

​		`flush priviliges;`

#### 普通用户修改自己密码

​		`set password=password('newpassword');`

#### 丢失root用户密码

​		`vim  /etc/my.cnf`

​				`[mysqld]`

​				`skip-grant-tables`		

​		`service mysqld restart`

​		`mysql -uroot`

​		`mysql>  update mysql.user set authentication_string=password('newpassword') where user='root' and host='localhost';`

​		`mysql> flush priviliges;`

## MySQL权限管理

### 权限应用的顺序：

​		user  >   db   >  tables_priv  >  colums_priv

### 查看授权

`show  grant\G`

`show grants for admin1@'%'\G`     //管理员查看用户admin1授权的语句

### 语法格式：

`grant  权限列表  on  库名.表名  to  '用户名'@'客户端主机名'  [identified by ‘密码’  with option  参数]；`

权限列表		all   所有权限，但不包括授权权限

​					  select，update

数据库.表名：	`*.*`			所有库下的所有表								global level

​							web.*		web库下的所有表								database  level

​							web.t1		web库下的t1表										table  level

​							select (col1), insert(col1 ,col2) on mydb.mytb1		column level

客户端主机：		%								所有主机

​							192.168.2.%				192.168.2网段的所有主机

​							192.168.2.23				指定主机

​							localhost						指定主机

with_option参数：grant option		授权选项

​							MAX_QUERIES_PER_HOUR		定义每小时允许执行的查询数

​							MAX_UPDATE_PER_HOUR	定义每小时允许执行的更新数

​							MAX_CONNECTIONS_PER_HOUR	每小时可建立的连接数

​							MAX_USER_CONNECTIONS	单个用户同时可建立的连接数								

-----------

还有更多使用   `help  grant`  查看							

-----

### 授权GRANT示例

`grant  all  on *.*  to admin1@'%'  identified by 'mimashi123';` //全局授权，但无grant权限

`grant all on *.* to admin2@'%' identified by 'mimashi123' with grant option;`   //有grant权限的全局授权，与管理员root一样的权限

------

`grant all on bbs.* to admin3@'%' identified by 'mimashi123';`//库级授权

这条用的最多，但是实际生产中，一般是admin@‘192.168.12.12’，不会采用admin@'%'这种形式，一定是对生产环境里的前端具体用的某一台主机名进行授权，如Apache、Tomcat这些的服务器，而非所有主机 。‘%’是通配符。

-------

`grant all on bbs.user to admin4@'%' identified by 'mimashi123';`//表级授权

`grant select(col1),insert(col2,col3) on bbs.user to admin5@'%' identified by 'mimashi123';`//列级授权



全局授权的用户信息和权限信息都在user表中，而数据库授权的用户信息在user表中，授权信息在db表中。

### 回收权限REVOKE

语法：

​	REVOKE  权限列表  ON  数据库名  FROM  用户名@'客户端主机名'；

示例：

`revoke  delete on *.* from admin1@'%';` 	

`revoke all privileges on *.* from admin2@'%';`

`revoke all privileges,grant option on *.* from 'admin3'@'%';`

TIPS：

5.6版本之前，如果想删除用户，必须先撤掉用户的权限 revoke all privileges 之后才能删除用户 drop user。如果没撤权限就删除了用户，之后如果创建了同名的被删用户，则之前的权限还在，留下了隐患。

5.7就直接删除用户就行了   drop  user


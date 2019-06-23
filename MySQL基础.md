# 	SQL语言（结构化查询语言）

​		DDL语句	数据库定义语言：数据库、表、视图、索引、存储过程、函数、CREATE	DROP	ALERT		//开发人员

​		DML语句	数据库操纵语言：插入数据INSERT、删除数据DELETE、更新数据UPDATE		//开发人员

​		DQL语句	数据库查询语言：查询数据SELECT

​		DCL语句	数据库控制语言：例如控制用户访问权限GRANT、REVOKE

# mysql小技巧

`ctr+z`	从mysql界面暂停，转到系统界面，从系统界面输入`fg`，回到mysql界面。也可以在mysql控制台 使用system  后跟linux命令来执行linux命令

`lsblk`	linux查看硬盘

`select database（）`	查看当前使用的是哪个数据库

`ss -tnlp |grep :3306`	查看3306端口是否打开

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

### 修改字段名

 将test字段改为test1
		`ALTER TABLE 表名 CHANGE 原字段名 新字段名 字段类型 约束条件`
		`ALTER TABLE user10 CHANGE test test1 CHAR(32) NOT NULL DEFAULT '123';`

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

eg.全国每个城市有不同的分公司，查询出每个城市的某一个分公司的全部信息

```mysql
表名cityandcompany，字段cityname，compname，tel
create table cityandcompany2(cityname varchar(20),compname varchar(20),tel int primary key auto_increment);
insert into cityandcompany2 values ('nanjing','co1'),('nanjing','co2'),('nanjing','co3'),('shanghai','co1'),('shanghai','co2'),('shanghai','co3');
select * from cityandcompany2;
select * from cityandcompany2 where compname IN (select max(compname) from cityandcompany2 group by cityname);
```

```
这篇博客讲的比较好：https://www.cnblogs.com/zhuiluoyu/p/6862673.html
```

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

```mysql
1.ANY关键字
假设any内部的查询语句返回的结果个数是三个，如:result1,result2,result3,那么，
select ...from ... where a > any(...);
->
select ...from ... where a > result1 or a > result2 or a > result3;

2.ALL关键字
ALL关键字与any关键字类似，只不过上面的or改成and。即:
select ...from ... where a > all(...);
->
select ...from ... where a > result1 and a > result2 and a > result3;

3.SOME关键字
some关键字和any关键字是一样的功能。所以:
select ...from ... where a > some(...);
->
select ...from ... where a > result1 or a > result2 or a > result3;

4.IN关键字
IN运算符用于WHERE表达式中，以列表项的形式支持多个选择，语法如下：
　　WHERE column IN (value1,value2,...)
　　WHERE column NOT IN (value1,value2,...)
当 IN 前面加上 NOT运算符时，表示与 IN 相反的意思，即不在这些列表项内选择。代码如下:
查询
SELECT ID,NAME FROM A WHERE　ID IN (SELECT AID FROM B)             //查询B表中AID的记录
SELECT ID,NAME FROM A WHERE　ID　NOT IN (SELECT AID FROM B)        //意思和上面相反
删除
delete from articles where id in (1,2,3);                         //删除id=1,id=2,id=3的记录
delete from articles where id not in (1);                         //删除id!=1的记录
词语IN是"＝ANY"的别名。因此，这两个语句是一样的：
SELECT s1 FROM t1 WHERE s1 = ANY (SELECT s1 FROM t2);
SELECT s1 FROM t1 WHERE s1 IN    (SELECT s1 FROM t2);

5.EXISTS关键字
MySQL EXISTS 和 NOT EXISTS 子查询语法如下：
　　SELECT ... FROM table WHERE  EXISTS (subquery)
该语法可以理解为：将主查询的数据，放到子查询中做条件验证，根据验证结果（TRUE 或 FALSE）来决定主查询的数据结果是否得以保留。
mysql> SELECT * FROM employee
    -> WHERE EXISTS
    -> (SELECT d_name FROM department WHERE d_id=1004);
Empty set (0.00 sec)
此处内层循环并没有查询到满足条件的结果，因此返回false，外层查询不执行。
NOT EXISTS刚好与之相反
当然，EXISTS关键字可以与其他的查询条件一起使用，条件表达式与EXISTS关键字之间用AND或者OR来连接，如下：
mysql> SELECT * FROM employee
    -> WHERE age>24 AND EXISTS
    -> (SELECT d_name FROM department WHERE d_id=1003);
提示：
•EXISTS (subquery) 只返回 TRUE 或 FALSE，因此子查询中的 SELECT * 也可以是 SELECT 1 或其他，官方说法是实际执行时会忽略 SELECT 清单，因此没有区别。
•EXISTS 子查询的实际执行过程可能经过了优化而不是我们理解上的逐条对比，如果担忧效率问题，可进行实际检验以确定是否有效率问题。
•EXISTS 子查询往往也可以用条件表达式、其他子查询或者 JOIN 来替代，何种最优需要具体问题具体分析

6.UNION关键字
MySQL UNION 用于把来自多个 SELECT 语句的结果组合到一个结果集合中。语法为：
　　SELECT column,... FROM table1 
　　UNION [ALL]
　　SELECT column,... FROM table2
在多个 SELECT 语句中，对应的列应该具有相同的字段属性，且第一个 SELECT 语句中被使用的字段名称也被用于结果的字段名称。
UNION 与 UNION ALL 的区别
当使用 UNION 时，MySQL 会把结果集中重复的记录删掉，而使用 UNION ALL ，MySQL 会把所有的记录返回，且效率高于 UNION。
复制代码
mysql> SELECT d_id FROM employee
    -> UNION
    -> SELECT d_id FROM department;
+------+
| d_id |
+------+
| 1001 |
| 1002 |
| 1004 |
| 1003 |
+------+
合并比较好理解，也就是将多个查询的结果合并在一起，然后去除其中的重复记录，如果想保存重复记录可以使用UNION ALL语句。
```

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

# MySQL日志管理

主要以rpm包安装讲解：

 /etc/my.cnf		针对这个文件写入相关指令开启或关闭相关日志文件

error log   	错误日志   		 排错		/var/log/mysqld.log    	默认开启

bin  log		 二进制日志		备份		增量备份DDL	DML	DCL，主要存储数据库的变更日志，如增删改。不包括查。

relay log		中继日志		   复制		接收replication	master

slow log 		慢查询日志		调优		查询时间超过指定值

## ERROR   Log

log-error=/var/log/mysqld.log			向/etc/my.cnf中写入这条，开启error log

## Binary Log

log-bin=/var/log/mysql-bin/muzi		//向/etc/my.cnf中写入这条，开启bin log，加muzi是为了将日志文件命名前缀，生成的日志文件如：muzi-bin.0001	muzi-bin.0002等

sever-id=2										//5.7以后的版本还得加上这条，数字2随便取，一般取ip的最后数字，比如10

[root@muzi~]#  mkdir /var/lib/mysql-bin			//这是为了将数据库文件和日志文件分离，自建mysql-bin目录，将日志文件存入。默认日志文件是和mysql文件一起存放在/var/lib/mysql/中。

[root@muzi~]#  chown mysql.mysql /var/lib/mysql-bin/    //文件夹将权限改为mysql用户mysql组

[root@muzi~]#  systemctl reatart mysqld		//重启mysqld

注：

1.systemctl restart  mysqld		系统控制台命令，重启mysqld会截断日志，生成一个新的日志

2.flush logs		mysql控制台内命令，也会截断日志，生成新的日志。

3.reset master 		删除所有binlog，最好别用。这就是mysql的`rm -rf /`删库跑路~~

4.删除部分bin日志

​		PURGE BINARY LOGS TO 'mysql-bin.010';		删除指定日志文件mysql-bin.010

​		PURGE BINARY LOGS BEFORE '2019-4-02 22:46:23';	删除指定日期前的日志

 5.暂停写入bin日志   

SET	SQL_LOG_BIN=0;  //，mysql控制台输入。大小写没关系。关闭写入日志文件。

SET	SQL_LOG_BIN=1; //打开写入日志文件。要注意，这只是对当前mysql会话起作用，如果关了重进，或者另一个用户，其相应的日志文件还是会写入，只不过你自己的没有写入。

6.截取binlog

all：

#mysqlbinlog	mysql.00001

datatime:    //如果操作是发生在同一秒，则针对datetime就不如针对position了。

#mysqlbinlog	mysql.00001 --start-datetime='2019-4-02-23：01:23'

#mysqlbinlog	mysql.00001 --stop-datetime='2019-4-02-23：01:23'

#mysqlbinlog	mysql.00001 --start-datetime='2019-4-02-23：01:23'  --stop-datetime='2019-4-02-23：01:23'

position：   //常用。备份、恢复、主从同步等等。

#mysqlbinlog	mysql.00001 --start-position=200

#mysqlbinlog	mysql.00001 --stop-postiton=220

#mysqlbinlog	mysql.00001 --start-positon=270 --stop-positon=930

## Slow	Query	Log

slow_query_log=1	//向/etc/my.cnf文件中写入该句，打开慢查询日志功能。

slow_query_log_file=/var/log/mysql-slow/slow.log  //自定义慢查询日志文件

long_query_time=3			//定义慢查询时间，这里是3秒为一个慢查询，超过3秒的查询语句才会写入慢查询日志中。用于查询语句调优。

[root@muzi~]#   mkdir /var/log/mysql-slow

[root@muzi~]#   chown mysql.mysql /var/log/mysql-slow/

[root@muzi~]#   systemctl restart mysqld

查看慢查询日志

eg.：BENCHMARK(count,expr)

​			select benchmark(500000000,2*3);

# MySQL备份

databases	binlog	my.conf

所有备份数据都应放在非数据库本地，而且建议有多份副本。

测试环境中做日常恢复演练，恢复较备份更为重要。

数据的一致性		服务的一致性

**逻辑备份**：备份的是建表、建库、插入等操作所执行的SQL语句（DDL DML DCL），适用于中小型数据库，效率相对较低。使用的是mysql工具。

​				mysqldump			mydumper

**物理备份**：直接复制数据库文件，适用于大型数据库环境，不受存储引擎的影响，但不能恢复到不同的MySQL版本。使用的是系统工具。需要考虑数据的一致性、服务的可用性。

​				tar，cp	xtrabackup		inbackup		lvm snapshot

​				完全备份				增量备份				差异备份 

## 物理备份

### tar备份

备份期间，服务不可用

备份过程：完全物理备份

```bash
1.停止数据库
	systemctl  stop  mysqld
2.tar备份数据
	mkdir  /backup
	tar -cf /backup/`data+%F`_mysql_all.tar.gz /var/lib/mysql
	//当天日期加_mysql_all.tar.gz	
	ls /backup/			//会生成如2019-2-22_mysql_all.tar.gz
```

注：备份文件应该复制到其他服务器或存储上。

还原过程：

```bash
1.停止数据库
	systemctl stop mysqld
2.清理环境
	rm -rf /var/lib/mysql/*
3.导入备份数据
	tar -xf /backup/2019-2-22_mysql_all.tar.gz -C /
4.启动数据库
	chown -R mysql.mysql  /var/lib/mysql  //恢复过后如果权限不对则需要改一下权限
	systemctl start mysqld
5.binlog恢复
	
```

###  LVM  snapshot  逻辑卷快照备份    第43集  有点乱

数据一致，服务可用

注：MySQL数据lv 和 将要创建的snapshot 必须在同一VG，即快照卷和原始卷为同一VG。因此VG必须要有一定剩余空间。快照卷主要解决的是数据的一致性问题。

加全局读锁---->LVM mysql 快照----> 释放锁----> 挂载快照卷（只读方式）---->从快照卷中复制数据---->卸载并删除快照卷 

```bash
df -Th   查看挂载情况

lvscan	查看已建立快照
```

情况一：正常安装MySQL：

​		1.安装系统

​		2.准备LVM，例如 /dev/vg_muzi/lv-mysql, mount /var/lib/mysql  将mysql挂载到这个逻辑卷上

​		3.安装MySQL，默认datadir=/var/lib/mysql

情况二：MySQL已经运行一段时间，数据并没有存储在LVM上，而是在/var/lib/mysql中，所以需要新建逻辑卷并将mysql数据挂载到新建的逻辑卷。，将现在的数据迁移到LVM：

​		1.准备lvm及文件系统

```bash
[root@muzi~]#  lvcreate -n lv-mysql -L 2G datavg  //创建一个逻辑卷，大小为2G
[root@muzi~]# mkfs.xfs /dev/datavg/lv-mysql  //将其格式化为xfs系统
```

​		2.将数据迁移到LVM

```bash
[root@muzi~]# systemctl stop mysqld
[root@muzi~]# mount /dev/datavg/lv-mysql  /mnt/		//临时挂载点
[root@muzi~]# cp -a  /var/lib/mysql/*  /mnt		//将MySQL原数据拷贝到临时挂载点/mnt，此时/mnt里面数据就是mysql关闭服务时那一刻的数据了，拍了个照。
[root@muzi~]# umount   /mnt/			//删除临时挂载点
[root@muzi~]# vim /etc/fstab			//加入fstab开机挂载
	/dev/datavg/lv-mysql 	/var/lib/mysql	xfs	defaults	0  0
[root@muzi~]# mount  -a
[root@muzi~]# chown -R mysql.mysql  /var/lib/mysql
```

#### LVM备份流程

1.加全局读锁

```mysql
mysql>	flush	tables with read lock;
```

2.创建快照

```bash
[root@muzi~]# lvcreate -L 500M -s -n lv-mysql-snap  /dev/datavg/lv-mysql
[root@muzi~]# mysql -p'mimashi123' -e'show master status' > /backup/`date +%F`_position.txt  //-e的语句是查看创建快照时的position，整句话是将创建快照时的position存入txt文件中，最好记下来，当前备份到的位置，以后恢复的时候就从这个position位置处恢复。
```

3.释放锁

```mysql
mysql> unlock	tables;
```

1-3必须在同一会话中完成，第1步加锁之后，mysql会话不能关，一旦关闭，锁就自动失效。所以实际生产中不是1,2,3分步执行的，而是采用下面语句使得这三步语句一起执行，保证处于同一会话中：

```bash
[root@muzi~]# echo 'FLUSH TABLES WITH READ LOCK;SYSTEM lvcreate -L 500M -s -n lv-mysql-snap  /dev/datavg/lv-mysql; UNLOCK TABLES;'  |  mysql -p'mimashi123'   

[root@muzi~]# echo "FLUSH TABLES WITH READ LOCK;SYSTEM lvcreate -L 500M -s -n lv-mysql-snap  /dev/datavg/lv-mysql;"  | mysql -p'mimashi123'
//这两句的效果是一样的，因为语句执行完毕后，会话结束，锁自动失效，所以不需要手动解锁。
```

​		4.从快照中备份

```bash
[root@muzi~]# mount -o ro  /dev/datavg/lv-mysql-snap  /mnt/	 //xfs: -o ro,nouuid
[root@muzi~]# cd /mnt/
[root@muzi~]# tar -cf /backup/`date+%F`_mysql_all.tar.gz  ./*
```

​		5.移除快照

```bash
[root@muzi~]# cd;umount /mnt/
[root@muzi~]# lvremove -f /dev/vg_mizi/lv-mysql-snap
```

#### LVM恢复流程

```bash
1.停止数据库
	systemctl stop mysqld
2.清理环境
	rm -rf /var/lib/mysql/*
3.恢复数据
	tar xf /backup/2019-9-22_mysql_all.tar.gz -C /var/lib/mysql/
4.修改权限
	chown -R mysql.mysql	/var/lib/mysql/
5.启动数据库
	systemctl start mysqld
	echo "FLUSH TABLES WITH READ LOCK;SYSTEM lvcreate -L 500M -s -n lv-mysql-snap /dev/datavg/lv-mysql;" | mysql -p'mimashi123'
6.binlog恢复
	

```

#### 脚本+Cron  第44  45集，自动化部署，要再看。

```bash
#!/bin/bash
#LVM backmysql...
back_dir=/backup/`date+%F`
[ -d $back_dir ] || mkdir -p $back_dir
echo "FLUSH TABLES WITH READ LOCK;SYSTEM lvcreate -L 500M -s -n lv-mysql-snap /dev/datavg/lv-mysql;" | mysql -p'mimashi123'
mount -o ro,nouuid /dev/datavg/lv-mysql-snap  /mnt
rsync -a /mnt/ $back_dir
if[$? -eq 0 ];then
	umount /mnt/
	lvremove -f /dev/datavg/lv-mysql-sanp
fi
```

### percona--Xtrabackup  物理备份

它是开源免费的支持MySQL数据库热备份的软件，能对innodb和xtradb存储引擎的数据库非阻塞地备份，不暂停服务创建innodb热备份；

percona是一家老牌的mysql服务咨询公司，旗下mysql分支产品--percona server。

安装xtrabackup 

`yum install https://repo.percona.com/yum/percona-release-latest.noarch.rpm`

`yum install percona-xtrabackup-80`

rpm -qa |grep percona  //查看有没有装好

rpm -ql percona-xtrabackup-24-...    //查看软件

案例1：

完全备份的备份流程

```bash
mkdir  /xtrabackup/full  -p   //新建备份文件夹
innobackupex --user=root --password='mimashi123' /xtrabackup/full  //将整个mysql文件备份到/xtrabackup/full文件夹中  备份时长取决于数据库大小
cd /xtrabackup/full/    //切换到备份文件夹中，会发现mysql所有的文件都在这了
ls   //此时会以备份时间创建备份文件夹，如2019-12-12_23-22-10
ls 2019-12-12_23_22_10/		//该文件夹中有个xtrabackup_binlog_info文件，记录了备份的位置，当前的二进制文件binlog是几，以及position是几，恢复的时候可以依据此文件进行恢复。

```

完全备份的恢复流程

```bash
1.停止数据库
systemctl stop mysqld

2.清理环境
rm -rf /var/lib/mysql/*
rm -rf /var/lib/mysqld.log
rm -rf /var/log/mysql-slow/slow.log

3.重演回滚
innobackupex --apply-log /xtrabackup/full/2019-12-12_23-22-10/   //重演回滚--allpy-log

4.恢复数据
innobackupex --copy-back /xtrabackup/full/2019-12-12_23-22-10/   //恢复数据。将数据文件拷贝到/var/lib/mysql中去，这里是通过my.cnf文件中的'basedir=/var/lib/mysql'知道拷贝的目的地址的 

5.修改权限
chown -R mysql.mysql /var/lib/mysql

6.启动数据库
systemctl start mysqld
```

案例2 ：

增量备份的备份流程

```mysql
准备一个表t
create database t1;
use t1;
create table t(id int);
insert into t values(1);
```

```bash
1.周一：完整备份
mkdir /xtrabackup
innobackupex --user=root --password='mimashi123' /xtrabackup
ls /xtrabackup/	//此时问文件夹内会产生以备份时时间命名的完全备份文件夹，如2019-12-12_23-22-10

2.周二~周六：增量备份
// 周二：mysql> insert into t values(2);
innobackupex --user=root --password='mimashi123' --incremental /xtrabackup --incremental-basedir=/xtrabackup/2019-12-12_23-22-10/
ls /xtrabackup/     //除了第一个生成的第一个完全备份文件夹，这是会生成另一个增量备份文件夹，如2019-12-13_23-22-10,下一次的增量备份将以这个新的文件夹为basedir进行增量备份

//周三：mysql> insert into t values(3);
innobackupex --user=root --password='mimashi123' --incremental /xtrabackup --incremental-basedir=/xtrabackup/2019-12-13_23-22-10/
ls /xtrabackup/     //除了第一个生成的第一个完全备份文件夹，以及周二生成另一个增量备份文件夹，这次会生成周三的增量备份文件夹，如2019-12-14_23-22-10,同样的，下一次的增量备份将以这个新的文件夹为basedir进行增量备份
```

增量备份的恢复流程

```bash
1.停止数据库
systemctl stop mysqld

2.清理环境
rm -rf /var/lib/mysql/*
rm -rf /var/lib/mysqld.log
rm -rf /var/log/mysql-slow/slow.log

3.依次重演回滚 redo log ，想恢复到哪天的就回滚到哪天。其实就是将那天的增量文件夹加入到完全备份文件夹中去。
先回滚周一的：full  2019-12-12_23-22-10
innobackupex --apply-log --redo-only /xtrabackup/2019-12-12_23-22-10/
依次回滚周二： 2019-12-13_23-22-10  周三： 2019-12-14_23-22-10
innobackupex --apply-log --redo-only /xtrabackup/2019-12-12_23-22-10/ --incremental-dir=/xtrabackup/2019-12-13_23-22-10
innobackupex --apply-log --redo-only /xtrabackup/2019-12-12_23-22-10/ --incremental-dir=/xtrabackup/2019-12-14_23-22-10

4.恢复数据，拷贝的始终是完全备份的那个文件夹
三种方式：
cp： cp -rf /xtrabackup/2019-12-12_23-22-10/* /var/lib/mysql/ 

rsync

innobackupex copy-back (依据的是my.cnf的datadir)：
innobackupex --copy-back /xtrabackup/full/2019-12-12_23-22-10/   //恢复数据。将数据文件拷贝到/var/lib/mysql中去，这里是通过my.cnf文件中的'basedir=/var/lib/mysql'知道拷贝的目的地址的 

5.修改权限
chown -R mysql.mysql /var/lib/mysql

6.启动数据库
systemctl start mysqld

7.binlog恢复，从备份的那个点以后的那些操作，依靠二进制文件进行恢复。

```

案例3：

差异备份的备份流程

```mysql
准备一个表t
create database t1;
use t1;
create table t(id int);
insert into t values(1);
```

```bash
1.周一：完整备份
mkdir /xtrabcakup
innobackupex --user=root  --password='mimashi123' /xtrabackup/

2.周二~周六：差异备份
 //周二：mysql> insert into t values(2);
 innobackupex --user=root --password='mimashi123' --incremental /xtrabackup --incremental-basedir=/xtrabackup/完全备份目录（周一的）
 
周三：mysql> insert into t values(3);
innobackupex --user=root --password='mimashi123' --incremental /xtrabackup --incremental-basedir=/xtrabackup/完全备份目录（注意还是基于周一的哦）
```

差异备份的恢复流程

```bash
1.停止数据库
systemctl stop mysqld

2.清理环境
rm -rf /var/lib/mysql/*
rm -rf /var/lib/mysqld.log
rm -rf /var/log/mysql-slow/slow.log

3.重演回滚
第一步，先回滚完全备份文件夹：
innobackupex --apply-log /xtrabackup/周一的完全备份文件夹   
第二歩，想恢复到哪天，直接回滚那天的差异备份文件夹，而不用一次进行回滚，这里回滚到周三：
innobackupex --apply-log --redo-only /xtrabackup/完全备份文件夹（周一的）  --incremental-dir=/xtrabackup/周三的差异备份文件夹

4.恢复数据
三种方式：cp、rsync、innobackupex --copy-back
innobackupex --copy-back /xtrabackup/full/周一的完全备份文件夹

5.修改权限
chown -R mysql.mysql /var/lib/mysql

6.启动数据库
systemctl start mysqld

7.binlog恢复，从备份的那个点以后的那些操作，依靠二进制文件进行恢复

```

## 物理备份注意事项

任何的备份，物理或是逻辑，都只是备份到某个备份点，从这个备份点之后的所有操作，都得依靠binlog记录。binlog不是专门用来备份恢复的，但是binlog可以使你的恢复离你的崩溃点更接近。

所以my.cnf中的binlog得配置好。而且得在备份的时候要记住 备份点处的binlog的名称，以及对应的position。

所以脚本中最好加上下面这句，目的是将binlog及position记录下来。

`mysql -p'mimashi123' -e'show master status' > /backup/date +%F_position.txt  //-e的语句是查看创建快照时的position，整句话是将创建快照时的position存入txt文件中，最好记下来，当前备份到的位置，以后恢复的时候就从这个position位置处恢复。`

`mysql>show master status\G`  查看当前的binlog及position。

mysql重启、或对库有新的操作，都会使binlog及position截断生成新的。

### LVM

将MySQL的数据存放在逻辑卷上，借助于逻辑卷快照，可以将mysql数据瞬间定个保存下来，不用停数据库太久，保证了服务的可用性。先锁表，再快照，再释放锁，要注意这三个行为一定要在同一个会话当中，否则锁会随着会话的断开自动释放。之后挂载逻辑卷，这挂载也是一个临时性挂载，而且所有操作都是基于脚本加上计划任务来实现的，不会以手动实现。包括每天的备份是以每天的日期加上文件夹作为名称进行备份的 。另外数据库若比较大，备份比较多的情况下，要考虑删除，可以结合shell脚本当中利用AWK或FIND的方式去进行删除。

LVM的缺点就是过于麻烦，要掌握系统管理员权限才可以进行。

### percona工具

要注意，它会去读数据库配置文件当中的datadir，所以务必要先确定这个配置文件中的相关设置。

增量备份，都是基于上一次备份。

差异备份，都是基于完全备份。

完全备份恢复，只需--apply-log完全备份文件夹。

增量备份恢复，需先--apply-log完全备份文件夹，再依次--apply-log增量文件夹到需要恢复的那一天的增量备份文件夹。

差异备份恢复，需先--apply-log完全备份文件夹，再--apply-log需要恢复的那一天的差异备份文件夹。

## 逻辑备份

加`--master-data=1`之后，逻辑备份可以自动记录binlog和position。保存在mysqldump的sql文件中。

数据一致、服务可用性。 

### 语法

`mysqldump -h 服务器  -u用户名  -p密码  数据库名  >备份文件.sql`

关于数据库名：

​		-A，-all-databases						所有库

​		school											数据库名

​		 school t1 t2									school库的t1，t2表

​		-B，--database bbs test mysql		多个数据库

关于其他参数说明：

​		--single-transaction		innoDB一致性 服务可用性

​		-x，--lock-all-tables		myISAM一致性  服务可用性

​		-E，--events					备份事件调度器代码

​		--opt								 同时启动各种高级选项

​		-R，--routines				  备份存储过程和存储函数

​		-F，--flush-logs				备份之前截断日志，使备份点之后的操作重开一个binlog记录，就避免了binlog恢复时要使用start-position和stop-position了。

​		--triggers						  备份触发器

​		--master-data=1 | 2			  该选项将会记录binlog的日志位置与文件名并追加到文件中，建议=1.会自动记录。=2得手动指定。

### 备份流程

```bash
mysqldump -uroot -p'miamshi123'  --all-databases --single-transaction --routines --triggers --master-data=1 --flush-logs >/backup/`date+%F-%H`-mysql-all.sql  //date+%F是年月日，%H加上时。
ls /backup/    //此文件夹中会生成以备份时间命名的文件，如2019-12-22-22 
cat /backup/2019-12-22-22-mysql-all.sql
```

备份之后，业务正常推进...

```mysql
create database test；
create  table t1(id int);
insert  into t1 values (1);
...
还可能mysql崩溃....或者mysql被误删
这时候binlog就起作用了
```

```bash
mysql的binlog日志作用是用来记录mysql内部增删改等对mysql数据库有更新内容的记录（对数据库进行改动的操作），对数据库查询的语句如show，select开头的语句，不会被binlog日志记录，主要用于数据库的主从复制与及增量恢复。

binlog需要放在另外的存储位置，不能随着mysql或系统崩溃而消失。
binlog默认是存放在/var/lib/mysql中的。
my.cnf中的内容：log-bin=mysqlbin 这句，生成的binlog文件名就是以mysqlbin开头的。

在/etc/my.cnf中自定义配置binlog文件位置。
vim /etc/my.cnf  //找到log-bin的部分
	expire_logs_days=5  //配置定期清理
	log-bin=/home/logs/mysql-bin   //自定义的存放目录
	binlog_format=ROW  //记录的方式是行。还有另外两种statement level和mixed模式。参考https://www.cnblogs.com/rinack/p/9595370.html
systemctl restart mysqld   //重启mysql，去新建的目录下看看，已经有最新的日志了
mysqlbinlog工具的作用是解析mysql的二进制binlog日志内容，把二进制日志解析成可以在MySQL数据库里执行的SQL语句。
```

常用binlog命令

```mysql
show master logs;		查看所有binlog日志列表 
show variables like 'log_%';  		查看日志开启状态
 show master status;查看最新一个binlog日志的编号名称，及其最后一个操作事件结束点
flush logs;   刷新log日志，立刻产生一个新编号的binlog日志文件，跟重启一个效果
reset master;   清空所有binlog日志   //慎用！！
mysqlbinlog qfcloud-bin.00004   查看日志qfcloud-bin.00004
```

### 恢复流程（要在mysql运行下完成）

```bash

1.停止数据库(或找个新装的mysql)
systemctl stop mysqld

2.清理环境（新环境没必要）
rm -rf /var/lib/mysql/*
rm -rf /var/log/mysql-bin/*
rm -rf /var/log/mysql-slow/*
rm -rf /var/log/mysqld.log

3.启动数据库
systemctl start mysqld   //此时会重新初始化，产生新的密码
初始密码在/var/log/mysqld.log中
tips：shell脚本自动得到mysql临时密码语句：
mysql_pass=`grep 'temporary password' /var/log/mysqld.log | tail -1 |awk '{print $NF}'`    //echo $mysql_pass  就能得到临时密码了。
grep 'password' /var/log/mysqld.log

4.重置密码 
mysqladmin -uroot -p'初始密码' password 'mimashi456'
如果用脚本：mysqladmin -p"$mysql_pass"  password "mimashi456" //注意这里的-p后面一定是双引号，不可以用单引号，这涉及到shell脚本的变量使用规则。
5.导入备份数据
建议在恢复时，暂停binlog。因为恢复过程本身也会产生二进制日志。
方法一：
进到mysql控制台进行暂停binlog再source恢复
mysql> set sql_log_bin=0;
mysql> source /backup/2019-12-22-22-mysql-all.sql
方法二：
vim /backup/2019-12-22-22-mysql-all.sql  //编辑备份文件，在MASTER_LOG_FILE那句前面加上这句：
	set sql_log_bin=0;
mysql -p'mimashi456' < /backup/2019-12-22-22-mysql-all.sql
这里恢复以后，密码就变成备份数据库的密码了（mimashi123），而非刚刚设置的那个新密码（mimashi456）。 
mysql -p'mimashi456' -e'flush privileges'  //-e执行sql语句，刷新授权表，有利于生成binlog截断

6.重启mysql
systemctl restart mysqld    //密码就变回备份数据库的密码了（mimashi123）。

6.binlog日志恢复
mysqlbinlog qfcloud-bin.00003 --start-position=154 | mysql -uroot -p'mimashi123'  //备份点是 00003日志文件的 154位置，从这开始恢复
mysqlbinlog qfcloud-bin.00004 qfcloud-bin.00005 qfcloud-bin.00006 | mysql -uroot -p'mimashi123'  //接着恢复余下需要恢复的日志

假如在00007里有误删的操作，使用binlog恢复时，只需跳过相应position的语句执行就可以了，例如在00006的730到930是误删除的操作,220以后的是正确的操作：
mysqlbinlog  --start-position=232 qfcloud-bin.00003 qfcloud-bin.00004 qfcloud-bin.00005 | mysql -uroot -p'mimashi123'  
mysqlbinlog  --stop-position=730 qfcloud-bin.00006 | mysql -uroot -p'mimashi123' 
mysqlbinlog  --start-position=930 qfcloud-bin.00006 qfcloud-bin.00007 qfcloud-bin.00008 | mysql -uroot -p'mimashi123' 
```

##  表数据 的导入导出

### MySQL命令导出文本文件

```bash
mysql -uroot -p'mimashi123' -e 'select * from bbs.t1' > /table_save/t1.txt		//导出为txt
mysql -uroot -p'mimashi123' --xml -e 'select * from bbs.t1' > /table_save/t1.xml		//导出为xml
mysql -uroot -p'mimashi123' --html -e 'select * from bbs.t1' > /table_save/t1.html		//导出为html
```



### SELECT ... INTO OUTFILE  导出文本文件

有两个限制：

5.7以后有安全限制，需要在配置文件`my.cnf`中加入：

​		`secure-file-priv= /table_save`		//改完my.cnf记得重启mysql

另一个限制是导出表的存放目录，mysql得有权限：

​		`chown  mysql.mysql  /table_save/`

导出表的语法：  `select ... into outfile 导出地址/名称`

```mysql
select * from bbs.t1 into outfile '/table_save/t1';
system cat /table_save/t1 //此时就看到t1表内的数据了，字段间没有分隔符

select * from bbs.t1 into outfile 'table_save/t1_2' fields terminated by '---';  //使用‘---’作为导出文件内字段显示分隔符
```

表的导出导入只备份表的记录，不会备份表结构。因此需要通过mysqldump 备份表结构，恢复时先恢复表结构，再导入数据。

### LOAD  DATA INFILE  导入文本文件

```mysql
delete from bbs.t1;  //删除t1表内容

load data infile '/table_save/t1.txt' into table bbs.t1;  //将数据导入t1表

load data infile 'table_save/t1_2.txt' into table bbs.t1 fields terminated by '---';  //如果导出时定义了分隔符，导入时也要指定相应的分隔符。
```

# MySQL复制技术	AB replication

Primary-Secondary Replication	主从

Group Replication	集群

不等于备份

实时同步	机械故障	远程灾备	用于备份	高可用HA	负载均衡	读写分离	分布式数据库

M	M-S	M-S-S	M-M	M-M-S-S多源复制

复制原理

1.在主库上把数据更改（DDL	DML	DCL）的语句记录到二进制日志中（binlog）。

2.备库I/O线程将主库上的日志复制到自己的中继日志（relay log）中。

3.备库SQL线程读取中继日志中的事件，将其重放到备份数据库之上。

## M-S	一主一备

流程：

```mysql
master：
1. binlog开启  设置server-id=1  重启mysql	restart
2. grant replication  授权从机访问权限
3.初始化数据库	
mysqldump all databases(log_file,position) scp rsync ----->master2		//log_file和position记录了复制点。

slave:
1. 设置server-id=2，
2.初始化数据库	使用mysql控制台source方式导入master的数据，注意此时master的业务正常走，所以此时slave和master的数据是不一致的，但是借助于log_file和position，我们可以知道复制点的位置，即数据差异开始的地方。等到从机角色设置好后，从机会自动从复制点去复制新产生的数据。复制的时候设置--master-data=1可以自动记录这两项内容到复制的sql文件中。
3.设置从机角色  
		mysql > change master to
				master_host='master1',		//或master1的IP
				master_user='授权用户',
				master_password='授权密码'，
				master_log_file='xxx',		//log_file
				master_log_pos=xxx;			//position
4. mysql > start slave;			//启动slave角色
5. mysql > show slave status\G  //验证结果是否正常
```

示例：

master1（master）  	192.168.122.10		mysql密码'mimashi111'

master2（slave）		 192.168.122.20		mysql密码'mimashi222'

```bash
master1,master2添加host解析：使用主机名进行访问
[root@master1~]# vim /etc/hosts
					192.168.122.10  master1
					192.168.122.20  master2
[root@master1~]# scp -r /etc/hosts master2:/etc	//将这个host文件拷贝给master2
[root@master1~]# ping master2		//此时使用主机名ping就可以ping通了
```

第1步	master部署  [ master1 ]

在master1上模拟出日常环境：

```mysql
create database bbs；
create table bbs.t1(id int,name varchar(20));
insert into bbs.t1 values (1,'yang'),(2,'wang');
select * from bbs.t1;
```

```bash
[root@master1~]# vim  /etc/my.cnf
					log-bin
					server-id=1		//打开log-bin，设置server-id
[root@master1~]# systemctl restart mysqld
[root@master1~]# systemctl stop firewalld	//关闭防火墙
[root@master1~]# systemctl mask firewalld	//更狠关闭
[root@master1~]# mysql -uroot -p'mimashi111'
	mysql> grant replication slave,replication client on *.* to 'alice'@'192.168.122.20' identified by 'mimashi999';	//授权alice用户以'mimashi999'访问master1的mysql，注意这只是在mysql层面上的授权，而系统层面的授权也得做，如防火墙。因为从机是通过一个I/O线程从主机去拿日志，主机不能阻拦。 实际业务中，mysql都是部署在内网的，防火墙没必要开启。
	mysql> flush privileges;
[root@master1~]# mysqldump -p'mimashi111' --all-databases --single-transaction --master-data=1 --flush-logs > `date +%F`-mysql-all.sql  //复制主机数据文件，并以当天时间的年月日形式加-mysql-all.sql结尾生成文件。如2019-12-22-mysql-all.sql
[root@master1~]# sed -n '22p' 2019-12-22-mysql-all.sql //查看生成的复制文件，第22行记录了binlog和position。这行如果没有注释掉，从机就可以自动读取binlog和position位置。
[root@master1~]# scp -r 2019-12-22-mysql-all.sql master2:/backup //将复制的数据文件传给master2的/backup目录下
到此，master1的任务大致完成。
```

master1模拟增加数据，该数据并未在复制数据中

```mysql
insert into bbs.t1 values (3,'zhou'),(4,'wu');
select * from bbs.t1;
```

第2歩	slave部署   [ master2 ]

```bash
[root@master2~]# mysql -hmaster1 -ualice -p'mimashi999'	//测试授权的用户alice能否访问master1的mysql。如果拒绝访问要查看master1防火墙是否关闭，以及是否master1的mysql是否授权alice用户登录。
mysql> show grants;		//查看alice用户的权限
[root@master2~]# vim /etc/my.cnf   //配置server-id为2
					server-id=2
[root@master2~]# systemctl restart mysqld  //配置文件改完要重启mysqld
[root@master2~]# ls /backup/  //查看复制文件传过来了没
```

```mysql
接下来导入复制数据，这里最好采用mysql控制台导入，而非系统命令行导入
进到mysql控制台进行暂停binlog再source恢复
[root@master2~]# mysql -uroot -p'mimashi222'
mysql> set sql_log_bin=0;	//暂停binlog
mysql> source /backup/2019-12-22-mysql-all.sql
mysql> change master to master_host='master1',  //配置连接主库
					master_user='alice',
					master_password='mimashi999' 
					master_log_file=xxx	 //不写也行
					master_log_pos=xxx	//采用从mysql控制台导入数据这种方式时，这两句不写也行。从机会自动读复制文件的第22行内容。但是要注意，该技巧只适用于mysql控制台导入数据，且第22行数据没被注释，使用系统命令行方式导入时则必须加上这两句。另外，如果是新库，即主机mysql还没有数据时，master_log_file=使用show master status\G看到的那个binlog文件，master_log_pos=0；
mysql> start slave;		//启动从角色
mysql> show slave status\G	//查看从角色是否启动成功，查看slave_io_running和slave_sql_running是否为yes

这样设置过后，主机和从机的mysql已经完全同步了。包括mysql密码。主机的mysql有什么操作，从机就会有什么操作。但是不能反向操作，即从机进行mysql操作，主机不会随之改变。可以使用双主M-M实现反向操作。

mysql> select * from bbs.t1; //此时t1已经和master1处的t1一模一样了，都是4条数据。
```

## M-S GTID

流程：

```mysql
master：
1.log-bin	server-id=1		gtid_mode=ON	enfroce_gtid_consistency=1 	restart
2.grant replication
3.初始化数据库	
mysqldump all databases scp rsync ------> master2

slave:
1.server-id=2	
gtid_mode=ON	
enforce_gtid_consistency=1	
master-info-repository=TABLE 			//这两句使连接主库的主机名、
relay-log-info-repository=TABLE(slave)   //用户名、账号、密码等存储在一张表中，而非文件(/var/lib/mysql/master.info)中。特别是M-M-S-S时，建议写在表中。 
2.初始化数据库	导入数据
3.mysql> change master to
			master_host='master1',
			master_user='授权用户'，
			master_password='授权密码'，
			master_auto_position=1;  //与传统方式的差别
4.mysql> start slave;
5.mysql> show slave status\G
```

示例：

master1（master）  	192.168.122.10		mysql密码'mimashi111'

master2（slave）		 192.168.122.20		mysql密码'mimashi222'

```bash
master1,master2添加host解析：使用主机名进行访问
[root@master1~]# vim /etc/hosts
					192.168.122.10  master1
					192.168.122.20  master2
[root@master1~]# scp -r /etc/hosts master2:/etc	//将这个host文件拷贝给master2
[root@master1~]# cat /etc/hosts		//查看host解析文件
[root@master1~]# ping master2	 //此时使用主机名ping就可以ping通了
```

```bash
master:master1部署
[root@master1~]# mysql -uroot =p'mimashi111'
mysql> grant replication slave,replication client *.* to 'alice'@'192.168.122.%' identified by 'mimashi999';
mysql> flush peivileges;
[root@master1~]# mysqldump -p'mimashi111' --all-databases --single-transcation --master-data=1 --flush-logs > `date +%F`-mysql-all.sql
[root@master1~]# scp 2019-12-22-mysql-all.sql master2:/backup	
```

```bash
slave：master2部署
[root@master2~]# mysql -hmaster1 -ualice -p'mimashi999' //测试连接
[root@master2~]# vim /etc/my.cnf
				log-bin
				server-id=2
				gtid_mode=ON
				enforce_gtid_consistency=1
				master-info-repository=TABLE 
				relay-log-info-repository=TABLE
[root@master2~]# systemctl restart mysqld
[root@master2~]# mysql -uroot -p'mimashi222' < /backup/2019-12-22-mysql-all.sql  //GTID方式，就不用了考虑导入数据的方式了，都行。因为GTID会自动去协商binlog和position。
mysql> change master to 
	>	master_host='master1',
	>	master_user='alice',
	>	master_password='mimashi999',
	>	master_auto_position=1;
mysql> start slave;
mysql> show slave status\G
[root@master2~]# systemctl restart mysqld		//这是密码更正为主机mysql密码了。
```

## M-M-S-S 	多源复制

多源复制是5.7才兴起的。之前的是MHA技术，解决多主的问题。

最好是在数据库安装的时候就操作，避免了数据的操作。

M-M	GTID  双主模式，两个都可以对库进行操作，同时更新

M-M-S-S	GTID	多源复制 ，使用两个频道或多个频道，一个频道坏了就用另外的。

### M-M

（master1<--->master2）：续上一实验

```bash
Master2：主机服务设置
1.log-bin 	server-id=2		gtid_mode=ON	enforce_gtid_consistency=1	restart  //上一示例已设置

2.master1和master2数据已经一致		//上一示例已设置

3.给master1授权，由于之前针对用户alice授权是网段授权'alice'@'192.168.122.%'，所以也不用更改设置。
如果之前针对的具体IP授权，那么master2对master1授权也要在master1上进行授权，注意！也是在master1上授权。因为此时master1是作为主机，master2是从机，要想同步，还得从主机上设置，然后从机master2才能同步授权数据。因为主从机模式是单向的，只能从主同步到从，而不能反过来。
```

```mysql
master1:从机服务设置
mysql> change master to
	> master_host='master2',
	> master_user='alice',
	> master_password='mimashi999',
	> master_auto_position=1;
mysql> start slave
mysql> show slave status\G
```

### M-M-S-S

续上一实验

master1（master）  	192.168.122.10		    mysql密码'mimashi111'

master2（master）		 192.168.122.20		  mysql密码'mimashi222'

slave1（slave）			192.168.122.30			mysql密码'mimashi333'

slave2（slave）			192.168.122.40			mysql密码'mimashi444'

```bash
master1,master2,slave1,slave2添加host解析：使用主机名进行访问
[root@master1~]# vim /etc/hosts
					192.168.122.10  master1
					192.168.122.20  master2
					192.168.122.30	slave1
					192.168.122.40	slave2
[root@master1~]# scp -r /etc/hosts master2:/etc	//将这个host文件拷贝给master2
[root@master1~]# scp -r /etc/hosts slave2:/etc	//将这个host文件拷贝给slave1
[root@master1~]# scp -r /etc/hosts slave2:/etc	//将这个host文件拷贝给slave2
[root@master1~]# cat /etc/hosts		//查看host解析文件
[root@master1~]# ping master2	 //此时使用主机名ping就可以ping通了
```

```bash
master1/master2:
初始化数据库，保证m1，m2，s1，s2数据一致，此时m1和m2数据已经一致，都是主机。把数据库整个备份，分别拷给s1和s2。该操作也可在m2上做。
[root@master1~]# mysqldump -p'mimashi111' --all-databases --single-transcation --master-data=2 --flush-logs >`data +%F`-mysql-all.sql
[root@master1~]# scp -r 2019-12-22-mysql-all-sql slave1:/root
[root@master1~]# scp -r 2019-12-22-mysql-all-sql slave2:/root
[root@master1~]# mysql -uroot -p'mimashi111'
	mysql> reset master;  	//删除binlog，否则日志会和slave1和slave2的数据冲突,导致GTID协商不了，slave1和slave2的I/O服务打不开。

slave1:
[root@slave1~]# mysql -p'mimashi333' < 2019-12-22-mysql-all.mysql
[root@slave1~]# vim /etc/my.cnf
				server-id=3
				gtid_mode=ON
				enforce_gtid_consistency=1
				master-info-repository=TABLE
				relay-log-info-repository=TABLE
[root@slave1~]# systemctl restart mysqld //此时继承master1的mysql密码了
[root@slave1~]# mysql -uroot -p'miamshi111'
	mysql> reset master;		//清除二进制日志binlog
	mysql> change master to  //如果一开始在主机处是对单个IP授权，那么要在主机处授四次权。如果是对网段授权，只需要授一次权。
		> master_host='master1',
		> master_user='alice',
		> master_password='mimashi999'
		> master_auto_position=1 for channel 'alice-master1';//名为alice-master1的频道
	mysql> change master to 
		> master_host='master2',
		> master_user='alice',
		> master_password='mimashi999'
		> master_auto_position=1 for channel 'alice-master2';
	mysql> start slave;
	mysql> show slave status\G
	
slave2:
[root@slave2~]# mysql -p'mimashi444' < 2019-12-22-mysql-all.mysql
[root@slave2~]# vim /etc/my.cnf
				server-id=4
				gtid_mode=ON
				enforce_gtid_consistency=1
				master-info-repository=TABLE
				relay-log-info-repository=TABLE
[root@slave2~]# systemctl restart mysqld  //此时继承master1的mysql密码了
[root@slave2~]# mysql -uroot -p'miamshi111'
	mysql> reset master;		//清除二进制日志binlog
	mysql> change master to  
		> master_host='master1',
		> master_user='alice',
		> master_password='mimashi999'
		> master_auto_position=1 for channel 'alice-master1';
	mysql> change master to 
		> master_host='master2',
		> master_user='alice',
		> master_password='mimashi999'
		> master_auto_position=1 for channel 'alice-master2';
	mysql> start slave;
	mysql> show slave status\G
```

# MySQL中间件

MySQL Proxy	MySQL官方

Atlas					奇虎360

DBProxy				美团

Amoeba				早期阿里

cober					阿里

Mycat					阿里

mycat 基于java开发的，要先配置java环境。

## JAVA环境配置

```bash
搜索jdk，选择bin.tar.gz安装，不需要编译，只需要下载解压就ok，一般下载解压后都需要配置环境变量，而rpm包安装不用配置。
[root@mycat~] wget https://download.oracle.com/otn-pub/java/jdk/12.0.1+12/69cfe15208a647278a19ef0990eea691/jdk-12.0.1_linux-x64_bin.tar.gz?AuthParam=1561163766_f9ccf5b744e09a80056c96ef543c26de
[root@mycat~] tar -xf jdk-12.0.1_linux-x64_bin.tar.gz?AuthParam=1561163766_f9ccf5b744e09a80056c96ef543c26de -C /usr/local/
[root@mycat~] ls /usr/local   	//会出现jdk12.0.1的目录，目录名太长影响使用，做个链接
[root@mycat~] ln -s /usr/local/jdk12.0.1   /usr/local/java //将解压目录做个链接，相当于java目录装在/usr/local/java
[root@mycat~] vim /root/.bash_profile		//配置java_home环境变量,有些是针对对应用户下的.bash_profile或.profile进行更改，或者/etc/profile。这里我用的是RHEL7，修改/etc/profile之后，linux的命令全都不能用了，更改/root/.bash_profile则没有问题
				JAVA_HOME=/usr/local/java
				PATH=$JAVA_HOME/bin:$PATH  //这里JAVA_HOME在前，意思是我安装的这个java应该被优先选择。系统中安装的java版本可能会低
				export JAVA_HOME PATH
[root@mycat~] source /etc/profile	
[root@mycat~] echo $JAVA_HOME		//查看当前环境变量中有没有JAVA
		JAVA_HOME=/usr/local/java
[root@mycat~] java -version		//查看java版本是否为需要的版本
```

## Mycat配置

```bash
下载下来后同样不用编译，解压直接使用
[root@mycat~] wget http://dl.mycat.io/1.6.6.1/Mycat-server-1.6.6.1-release-20181031195535-linux.tar.gz
[root@mycat~] tar -xf Mycat-server-1.6.6.1-release-20181031195535-linux.tar.gz -C /usr/local/
[root@mycat~] ls /usr/local/mycat/
	bin catlet	lib	logs	verdion.txt
[root@mycat~] ls /usr/local/mycat/bin/	//mycat的启动相关命令
[root@mycat~] ls /usr/local/mycat/conf/		//配置文件位置
		server.xml	schema.xml	主要用这两个文件
[root@mycat~] ls /usr/local/mycat/logs/		//日志文件，正常运行以后会产生相应日志
```

``[root@mycat~] vim /usr/local/macat/conf/server.xml		//打开server.xml文件，进行配置：`

```xml
<user name='root'>	这个是前端应用程序去连接中间件Mycat的账户，是中间件Mycat设置的
		<poperty name='password'>123456</property>	//密码是123456
		<poperty name='schema'>muzi_1</property> //定义能访问的数据库名称，任意取名，但是要和schema.xml中配置的名称对应，建议取数据库名称。
    在前端应用使用root这个用户访问时，Mycat会去找这个名为muzi的schema在/schema.xml中配置的各项内容。
<user name='root1'>	//另一个账户
		<poperty name='password'>123456</property>	
		<poperty name='schema'>muzi_2</property> //Mycat会去找muzi_2这个schema的配置。
```

`[root@mycat~] vim /usr/local/macat/conf/schema.xml		//打开schema.xml文件，进行配置`：

```xml
<?xml version="1.0"?>
<!DOCTYPE mycat:schema  SYSTEM "schema.dtd">
<mycat:schema xmlns:mycat="http://io.macat/">
	<schema name="muzi_1"  
		checkSQLschema="false" 		//检测数据库schema。
		sqlMaxLimit="100" 			//sqlMaxLimit最大连接数。
		dataNode="dn1" //dataNode数据节点，名称任取，下一段进行配置。
	>
	</schema>
	<dataNode name="dn1" 
		dataHost="host_pool" //	可连接的主机池,名称任取，下一段进行配置。
		database="my_true_database" //database--连接的真实数据库名。
	>
	<dataHost name="host_pool"
		maxCon="1000"			//最大连接数
		minCon="10" 			//最小连接数
		balance="0" 			//负载均衡设置
		writeType="0" dbType="mysql" dbDriver="native" 	switchType="1" 	slaveThreshold="100"	
	>	
			<heartbeat>select user()</heartbeat>	//心跳检测。检测连接的数据库主机是否能正常工作，任意语句都可以。
			<writeHost host="master1" url="master1:3306" user="jack" password="mimashijack123"> //写主机设置。hostM1名称任取，建议真机名。url是真正的地址，建议不要用ip地址，使用DHCP名称解析。writeHost是写操作主机，所以user字段设置为可以对数据库进行写操作的用户，尽量配置一个专门的用户授相应的权。这里使用jack用户，对其进行写授权。注意这里的用户，是中间件Mycat访问数据库的用户，是MySQL设置的。
			<readHost host="slave1" url="192.168.1.200:3306" user="jack" password="mimashijack123" /> //读主机设置。slave1名称任取，建议真机名。url是真正的地址，建议不要用ip地址，使用DHCP名称解析。这里同样使用jack用户进行访问控制，要对jack这个用户进行读授权。
			<readHost host="slave2" url="slave2:3306" user="jack" password="mimashijack123" /> //第二个读主机。
			</writeHost>
        
			<writeHost host="master2" url="master2:3306" user="jack" password="mimashijack123"> //定义第二个写主机。
			<readHost host="slave1" url="192.168.1.200:3306" user="jack" password="mimashijack123" /> //第二个写主机的第一个读主机。
			<readHost host="slave2" url="slave2:3306" user="jack" password="mimashijack123" /> //第二个写主机的第二个读主机。
			</writeHost>
	</dataHost>
	
	<schema name="muzi_2" checkSQLschema="false" sqlMaxLimit="100" dataNode="dn1"></schema>  //名为muzi_2的schema，定义了要去访问的数据节点，以及一些访问规则。
	<dataNode name="dn1" dataHost="host_pool" database="my_true_database"> //数据节点，定义了要去访问的主机池，以及一些访问规则。
	<dataHost name="host_pool" maxCon="1000" minCon="10" balance="0" writeType="0" dbType="mysql" dbDriver="native"  switchType="1" slaveThreshold="100">	//主机池，定义了可去访问的各项主机，以及一些访问规则。
		<heartbeat>select user()</heartbeat>//定义了检测主机健康状态的方法。
		<writeHost host="master1" url="master1:3306" user="jack" password="mimashijack123"> 
		<readHost host="slave1" url="192.168.1.200:3306" user="jack" password="mimashijack123" /> 
		<readHost host="slave2" url="slave2:3306" user="jack" password="mimashijack123" />
		</writeHost>
        
		<writeHost host="master2" url="master2:3306" user="jack" password="mimashijack123"> 
		<readHost host="slave1" url="192.168.1.200:3306" user="jack" password="mimashijack123" /> 
		<readHost host="slave2" url="slave2:3306" user="jack" password="mimashijack123" />
		</writeHost>
	</dataHost>        
</mycat:schema>
```

`<dataHost>标签内的一些属性规则：`

balance属性：负载均衡类型。

​				="0"，不开启读写分离机制，所有操作都发送到当前可用的writeHost上。

​				="1"，全部的readHost 和 stand by writeHost 参与select 语句的负载均衡，简单的说，当双主机从模式（M1->S1，M2->S2，并且M1,M2互为主备），正常情况下，M2，S1，S2都参与select语句的负载均衡。开启读写分离机制，所有读操作都发送到当前可用的writeHost上。

​				="2"，所有读操作都随机地分发到writeHost、readHost上。

​				="3"，所有读请求随机地分发到writerHost对应的readHost执行，writeHost不负担读压力，注意balance=3只在1.4及以后版本有。

writeType属性：负载均衡类型。

​				="0"，所有写操作发送到配置的第一个writeHost，第一个挂了切到还生存的第二个writeHost，重新启动后以切换后的为准。

​				="1"，所有写操作都随机地发送到配置的writeHost，1.5以后已废弃，不推荐。

switchType属性：

​				="3"，基于MySQL Galera Cluster的切换机制，心跳语句为show status like 'wsrep%'

**结论：**

1.所有节点都正常，writeHost负责写操作，备writeHost负责读操作。

2.当第一个writeHost失效时，其中一个备的writeHost负责写操作，其他备的writeHost负责读操作。

3.当只有一个writeHost时，同时负责读写。

**M-M-S-S	准备Mycat连接的用户及权限**

192.168.122.234	为Mycat主机IP

```bash
将DHCP解析文件拷给Mycat：
[root@master1 ~] scp -r /etc/hosts 192.168.122.234:/etc
```

```mysql
先在mysql上授权Mycat能使用的用户，随便哪个master主机都行,这里在master1上进行授权:
mysql> grant all on bbs.* to 'jack'@'192.168.122.234' identified by 'mimashijack123';  //最好是针对特定业务所用到的库授权，比如bbs，而非*.*
mysql> flush privileges;
```

```bash
在Mycat上测试能不能连接到master、slave的mysql（Mycat上装个mysql-client或mariaDB）：
[root@mycat ~] mysql -hmaster1 -ujack -p'mimashijack123'
mysql> show grants;		//查看授权
[root@mycat ~] mysql -hmaster2 -ujack -p'mimashijack123'
[root@mycat ~] mysql -hslave1 -ujack -p'mimashijack123'
[root@mycat ~] mysql -hslave2 -ujack -p'mimashijack123'

接下来启动Mycat：
[root@mycat ~] /usr/local/mycat/bin/mycat start
[root@mycat ~] ss -tnlp |grep java	//查看有没有java进程，有，mycat才启动成功
[root@mycat ~] jps	//或使用java的jps查看java进程
[root@mycat ~] ps aux |grep mycat	//或直接查看有没有mycat进程
Mycat启动没成功基本上是配置文件的语法问题，这时候先查看生成的日志文件进行排错：
[root@mycat ~] ls /usr/local/mycat/logs/
	mycat.log	wrapper.log
[root@mycat ~] tail /usr/local/mycat/logs/mycat.log 
[root@mycat ~] tail /usr/local/mycat/logs/wrapper.log
第62集末尾几分钟排错。
```






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

### 避免重复distinct

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

### 关键字  between and

`select name ,salary from t1 where salary between 5000 and 6000;`

`select name ,salary from t1 where salary not between 5000 and 6000;`

### 关键字  is  null

`select name ,salsry from t1 where salary is null;`

`select name ,salsry from t1 where salary is not null;`

### 关键字  in  集合查询

`select name ,salary from t1  where salary=1000  or  salary=2000  or  salary =3000;`

`select name ,salary from t1 where salary in (1000,2000,3000);`

`select name ,salary from t1 where salary not in (1000,2000,3000);`

### 关键字  like  模糊查询

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

## 限制查询的记录数  limit

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

 使用正则查询

`select  * from t1  where  name REGEXP  '^ali';`

`select * from t1  where  name REGEXP 'yun$';`

`select  * from t1  where name REGEXP 'm{2}';`

## 对字符串匹配的方式

`where  name = 'tom';`			精确匹配

`where  name like  'to%';`		模糊匹配

`where  name like 'to_';`   模糊匹配

`where name REGEXP 'yun$';`  正则匹配

## 多表查询

  




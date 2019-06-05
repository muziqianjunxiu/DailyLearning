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
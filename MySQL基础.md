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


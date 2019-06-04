2019.6.1-----------

安装部署		**备份恢复		主备复制		HA架构		分布式数据库**		压力测试		性能优化		**自动化运维**

# SQL语言（结构化查询语言）

​		DDL语句	数据库定义语言：数据库、表、视图、索引、存储过程、函数、CREATE	DROP	ALERT		//开发人员

​		DML语句	数据库操纵语言：插入数据INSERT、删除数据DELETE、更新数据UPDATE		//开发人员

​		DQL语句	数据库查询语言：查询数据SELECT

​		DCL语句	数据库控制语言：例如控制用户访问权限GRANT、REVOKE

# CentOS下安装

## 不同安装方式相关目录位置

1.RPM or  Yum：`datadir：/var/lib/mysql`

2.源码包：`basedir：/usr/local/mysql8.0.10	datadir：/usr/local/mysql8.0.10/data`

3.预编译：`basedir：/usr/local/mysql8.0.10	datadir：/usr/local/mysql8.0.10/data`

## 1.	二进制预编译   rpm		

`Yum Repository`		`mysql80-community-release-el7-3.noarch.rpm`	

yum安装		二进制rpm包		这个包里面有一个规则文件，这个文件决定了mysql各个文件目录安放的位置 

官网  	----->		`mysql community server` ----->		`Redhat enterprise Linux/Oracle Linux`	----->		`mysql yum repository`点进去  提供的是yum仓库	

`yum  repolist`		查看yum仓库

`su  root`		切换为root用户

`cd 	/root`		进入/root，即root用户家目录			

`ls`		查看/root下文件

`wget	https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm`	下载mysql仓库rpm包到/root目录下

`md5sum	mysql80-community-release-el7-3.noarch.rpm`			使用md5sum计算下载的文件md5值，与官网下载链接给出的MD5值进行比对，一致才可以说明文件在下载过程中未损坏

`rpm -ivh  mysql80-community-release-el7-3.noarch.rpm`			使用rpm安装mysql仓库

`ls	/etc/yum.repos.d/`			查看仓库是否安装成功

`yum	makecache`				更新仓库		

``yum list |  grep mysql-community-server`			用grep过滤查看mysql-community-server安装源，结果是`mysql-community-server.x86_64` 

`yum -y install mysql-community-server.x86_64` 		执行安装

`ls	 /var/lib/mysql`		数据库文件位置（rpm包安装方式，`mysql`的各个文件夹位置是固定的），此时无文件

`systemctl	start 	mysqld`	第一次启动`mysql`，会初始化，生成数据库文件，再查看`/var/lib/mysql`

`ls	/var/lib/mysql`		再查看，此时生成了数据库文件

`systemctl	enable  mysqld`		设置`mysql`为开机自启

###`sed	-ri	'/^SELINUX=/cSELINUX=disabled'	/etc/selinux/config`		针对云服务器的配置，关闭selinux，seLinux 主要作用就是最大限度地减小系统中服务进程可访问的资源（最小权限原则）###

`ps  aux  | grep  mysqld`	或  `pidof  mysqld`   或 `service mysqld status`  或`chkconfig --list mysql`   **查看MySQL是否启动**

`systemctl	stop	firewalld`		关闭防火墙 

`systemctl	disable	firewalld`	关闭防火墙

`mysql	-uroot`		`mysql5.7`以后，第一次进`mysql`，没有密码进不去了，此时需要查看root临时密码：`在/var/log/mysqld.log`中

`grep 'password' /var/log/mysqld.log`			查看root临时密码 ，如`'OIDSHO23OI'`

`mysql	-uroot 	-p'OIDSHO23OI'`		初次登陆，命令提示符变成：`mysql>`

​		`ALTER	USER	root@'localhost'	IDENTIFIED	BY	'mimaSHI123!'；`			第一次要修改密码

**另一种改密方式**：不用进mysql

`mysqladmin  -uroot   -p'OIDSHO23OI'  password  "mimaSHI123!"`

## 2.	二进制预编译			Generic			

官网  	----->		`mysql community server ----->		Linux Generic	----->	download`	

`rpm -q glibc`		查看系统glibc版本，从官网下载对应glibc版本的包  ---->/root目录下   `wget  https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.16-linux-glibc2.12-x86_64.tar.xz`

纯粹的预编译版本，与rpm包的区别，rpm包已经自动完成了很多操作，例如创建用户组，规定每个文件的位置等等，使用Generic安装，这些动作都需要自己完成，但也增加了可控性。实际生产环境中，这些都是由自动化脚本来完成。

`groupadd	mysql`		增加用户组

`useradd	-r -g mysql  -s /bin/false mysql`

`cd /usr/local`				安装到这个目录下

`tar xvf /root/mysql-8.0.16-linux-glibc2.12-x86_64.tar.xz`	解压

`ln  -s  mysql-8.0.16-linux-glibc2.12-x86_64   mysql`			做个链接mysql，便于管理

**mysql初始化**

没有编译安装的过程

`cd  mysql`

`mkdir  mysql-files`

`chmod 750 mysql-files`

`chown -R mysql`

`chgrp -R mysql`

`bin/mysql --initialize --user=mysql  --basedir=/usr/local/mysql  --datadir=/usr/local/mysql/data`				初始化用户、安装目录、数据目录，这个过程会有root临时密码出现，记录下来

`bin/mysql_ssl_rsa_setup  --datadir=/usr/local/mysql/data`		建立公钥私钥目录

`chown -R root`		

`chown -R mysql data mysql-files`	

**建立Mysql配置文件my.cnf**		

`vim /etc/my.cnf`					

​		`[mysqld]`

​		`basedir=/usr/local/mysql`

​		`datadir=/usr/local/mysql/data`

###或者`\cp -rf support-files/my-default.cnf  /etc/my.cnf`   	替换配置文件###

**启动mysql**

方法一，手动启动

`bin/mysqld_safe  --user=mysql&`			

方法二，使用centos6 mysql.server脚本（system V）启动

`cp support-files/mysql.server  /etc/init.d/mysqld` 

`chkconfig  -add mysqld`			添加到开机启动

`chkconfig  mysqld on`				设置为开机启动，相当于systemctl  enable 

`service  mysqld start`			启动

查看启动状态

ps aux |grep mysqld

**添加PATH【可选项】**

默认是不能使用mysql命令登陆的，因为环境变量没设置

`/usr/local/mysql/bin/mysql`

`echo "export PATH=$PATH:/usr/local/mysql/bin" >> /etc/profile`

`source /etc/profile`

`mysql -uroot -p'xxxxx'`		此时就可以使用mysql命令进行登录

**如果需要重新初始化,比如密码忘了**

`killall 	mysqld`

`rm -rf  /usr/local/mysql/data`

`chown -R mysql`

`chgrp -R  mysql`

`/usr/local/mysql/bin/mysqld --initilize  --user=mysql  --basedir=/usr/local/mysql  --datadir=/usr/local/mysql/data`		初始化过程中，会产生新的root初始密码

`bin/mysql_ssl_rsa_setup`					ssl初始化

`chown -R  root`

`chown -R  mysql  data  mysql-files`

`service mysqld  start`					启动mysql

## 3.源码   SourceCode

多一个编译的过程，初始化过程和上面都是一样的。



# 忘记Mysql密码

## MySQL5.7.5  and   earlier

root用户：

`vim  /etc/my.cnf`				跳过授权表启动

​		`[mysqld]`

​		`skip-grant-tables` 

`service  mysql  restart`

`mysql`

​		`update  mysql.user set password=password("mimashi123") where user="root" and host="localhost";`

​		`flush privileges;`

​		`\q`

`vim  /etc.my.cnf`

​		`[mysqld]`

​		`#skip-grant-tables`

`service mysqld restart`

## MySQL5.7.6 and later

root用户：

`vim /etc/my.cnf`

​		`[mysqld]`

​		`skip-grant-tables`

`systemctl  restart mysqld`

`mysql`

​		`select user,host,authentication_string from mysql.user;`

​		`update mysql.user set authentication_string=password('mimashi123!') where user='root';`

​		`flush privileges;`

​		`\q`

`vim  /etc.my.cnf`

​		`[mysqld]`

​		`#skip-grant-tables`

`service restart mysqld`

`mysql	-uroot  -p"mimashi123!"`

## MySQL8

root用户：

`vim /etc/my.cnf`							编辑配置文件

​		`[mysqld]`									在`[mysqld]`模块中添加

​		`skip-grant-tables`					**跳过授权表启动**，即无密码启动

`systemctl  restart mysqld`				重启mysql服务

`mysql`												此时可以无密码进入

​		`use  mysql`								使用mysql表

​		`update user set authentication_string =' ' where user ='root';`		将密码置空

​		`\q`												退出mysql

`vim  /etc/my.cnf`							编辑配置文件

​		`[mysqld]`										在模块`[mysqld]`中

​		`#skip-grant-tables`						将免登陆语句注释掉

`service restart mysqld`						重启mysql服务

`mysql  -uroot -p`				提示输入密码直接回车，因为已经将密码置空了

​		`ALTER USER 'root'@'localhost' IDENTIFIED BY 'mimashi123!';`		修改密码为`'mimashi123!`'

​		`\q`

`mysql -uroot -p'mimashi123!'`			此时用新密码登陆		

## 区别

区别就在于5.7.6之前的版本字段名是password，且可以使用简单密码。而5.7.6之后的字段名是authentication_string，且不可以使用简单密码。`mysql8`中又废弃了password（）方法。

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


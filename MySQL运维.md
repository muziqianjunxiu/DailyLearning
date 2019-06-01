2019.6.1-----------

安装部署		**备份恢复		主备复制		HA架构		分布式数据库**		压力测试		性能优化		**自动化运维**

SQL语言（结构化查询语言）

​		DDL语句	数据库定义语言：数据库、表、视图、索引、存储过程、函数、CREATE	DROP	ALERT		//开发人员

​		DML语句	数据库操纵语言：插入数据INSERT、删除数据DELETE、更新数据UPDATE		//开发人员

​		DQL语句	数据库查询语言：查询数据SELECT

​		DCL语句	数据库控制语言：例如控制用户访问权限GRANT、REVOKE

# CentOS下安装

## 1.	二进制rpm		`Yum Repository`		`mysql80-community-release-el7-3.noarch.rpm`	

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

`yum list |  grep mysql-community`-server			用grep过滤查看mysql-community-server安装源，结果是`mysql-community-server.x86_64` 

`yum -y install mysql-community-server.x86_64` 		执行安装

`ls	 /var/lib/mysql`		数据库文件位置（rpm包安装方式，`mysql`的各个文件夹位置是固定的），此时无文件

`systemctl	start 	mysqld`	第一次启动`mysql`，会初始化，生成数据库文件，再查看`/var/lib/mysql`

`ls	/var/lib/mysql`		再查看，此时生成了数据库文件

`systemctl	enable`		设置`mysql`为开机自启

`sed	-ri	'/^SELINUX=/cSELINUX=disabled'	/etc/selinux/config`		

`systemctl	stop	firewalld`		关闭防火墙 

`systemctl	disable	firewalld`	关闭防火墙

`mysql	-uroot`		`mysql5.7`以后，第一次进`mysql`，没有密码进不去了，此时需要查看root临时密码：`在/var/log/mysqld.log`中

`grep 'password' /var/log/mysql.log`			查看root临时密码 ，如`'OIDSHO23OI'`

`mysql	-uroot 	-p'OIDSHO23OI'`		初次登陆，命令提示符变成：`mysql>`

​		`ALERT	USER	'root'@'localhost'	IDENTIFIED	BY	'woshimima123'`			第一次要修改密码



## 2.	二进制预编译			Generic			

官网  	----->		`mysql community server ----->		Linux Generic	----->	download`	

`rpm -q glibc`		查看系统glibc版本，从官网下载对应glibc版本的包




































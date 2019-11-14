mkdir GP1

cd GP1

mkdir Day01

cd Day01

mkvirtualenv py3 -p /usr/bin/python3     创建虚拟环境，解释器使用python3

workon py3    切换到py3虚拟环境中

pip freeze  查看环境中已安装包

pip install django==1.11.7  安装某一特定版本，需两个等号

django-admin startproject  HelloWorld   创建一个项目

tree 

cd HelloWorld 

python manage.py startapp App   创建一个应用

tree

python manage.py runserver   启动django

python manage.py makemigrate

--------------------------

# pycharm连接mysql数据库

### 1.apt源安装MySQL   Server

​		`sudo  apt-get update`			更新源

​		`sudo apt-get install mysql-server`		安装数据库

### 2.配置MySQL	Server

​		`sudo	mysql_secure_installation`		安装配置		此过程中要设置root密码

​		`sudo	sysytemctl 	status	 mysql.service`		检查MySQL服务状态	

​		`sudo systemctl enable mysql`				启动MySQL服务

### 3.创建及配置用户

​		`sudo	mysql	-uroot	-p`			root用户进入，输入刚刚创建的root密码

​		`CREATE DATABASE GP1HelloDjango charset=utf8;`			创建数据库，utf8支持中文存储

​		`GRANT ALL PRIVILEGES ON GP1HelloDjango.* TO muzi@localhost IDENTIFIED BY "mimaSHI123!";`		创建用户muzi(密码mimaSHI123!) 并赋予其以本机localhost登陆GP1HelloDjango数据库的所有权限。

​		`FLUSH PRIVILIGES`		修改生效

之后就可以在pycharm中以"muzi__mimaSHI123!"连接了。

### 4.连接mysql驱动：

常用的mysql驱动有三种：

1.MySQLclient：

​		Python2，3都能用，但是有个致命缺陷，它对mysql安装有要求，必须在指定位置存在配置文件。

2.python-mysql：

​		python2支持很好，python3不支持

3.pymysql：

​		python2,3都支持，而且它还可以伪装成前两个库。

​				`pip install pymysql`  安装pymysql

​				在项目主包 `__init__.py`中输入：

```python
import pymsql
pymysql.install_as_MySQLdb()
```

​				伪装成MySQLclient，再执行迁移：

​				`python manage.py migrate`
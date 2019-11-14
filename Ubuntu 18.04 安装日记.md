# 为root用户添加密码

​		`sudo 	passwd	root`		

--------------------------

# 		更新apt源为阿里云

​		`lsb_release	 -a`			查看自己的`ubuntu`系统的`codename`代号，需要选择阿里给出的对应你机器`codename`的源（一般有`bionic`和`trusty`等等）

```html
Ubuntu 12.04 (LTS)代号为precise。
Ubuntu 14.04 (LTS)代号为trusty。
Ubuntu 15.04 代号为vivid。
Ubuntu 15.10 代号为wily。
Ubuntu 16.04 (LTS)代号为xenial
Ubuntu 18.04 (LTS)代号为bionic
```

​						`su	 root`		切换到root用户

​						`cp	 /etc/apt/sources.list 	/etc/apt/sources.list.bak`		备份系统原来的源

​						`vi	sources.list`		进入vi，编辑sources.list

​									命令模式下，按方向键移动光标

​									`dd`	删除光标所在行，将原有文件内容删除

​									粘贴复制来的阿里源，`ubuntu 18.04(bionic)` 配置如下

```
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse 
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse 
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse 
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse 
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse 
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse 
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse 
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse 
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse 
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

​									**vi 换行：**命令模式下，移动方向键到需要换行的位置，按`‘a’`，再按`‘enter`’键，再按`‘ESC’`键，再移动方向键到下一个需要换行的地方，重复操作。

​									按`‘ESC'`键，进入命令行模式，按`“：wq！ ”`	保存退出

​						`sudo 	apt-get		update`		更新源文件

​						`sudo apt-get upgrade`    升级

​						此时就可以利用阿里源进行`sudo apt install***`安装各种软件了，例如：

​						`sudo 	apt 	install		vim`		安装vim

---------------------

# 防火墙设置	端口开闭	  

`sudo	ufw	status`							查看防火墙状态

`sudo ufw	enable`							打开防火墙

`sudo 	ufw	disable`							关闭防火墙

`sudo 	ufw	reload`							重启防火墙

`ufw default allow/deny`			外来访问默认允许/拒绝

`ufw allow/deny 20`					允许/拒绝 访问20端口,20后可跟/tcp或/udp，表示tcp或udp封包。

`ufw allow/deny servicename`		ufw从/etc/services中找到对应service的端口，进行过滤。

`ufw allow proto tcp from 10.0.1.0/10 to 本机ip port 25`			允许自10.0.1.0/10的tcp封包访-问本机的25端口。

`ufw delete allow/deny 20`			删除以前定义的"允许/拒绝访问20端口"的规则

----------------------------

# 安装pycharm

1. `sudo snap install pycharm-community --classic`

2. extract解压到~/Download目录中

3. `cd /Downloads/pycharm-edu-2019.2.2/bin`

4. `sh pycharm.sh`  运行pycharm

5. vim ~/.bashrc  将pycharm路径添加到$PATH，并设置别名方便启动

   ​		`export PATH=$PATH:~/Downloads/pycharm-edu-2019.2.2/bin`在/.bashrc末尾添加这句

   ​		`alias charm='pycharm.sh'`   设置别名

   这样设置后，在terminal中输入charm即可启动pycharm，期间terminal不可关闭。

---------------------------

# 安装teamviewer

 `sudo apt install gdebi-core`

`wget https://download.teamviewer.com/download/linux/teamviewer_amd64.deb`即可下载最新的Ubuntu系统TeamViewer软件包

`sudo gdebi teamviewer_amd64.deb`  使用gdebi命令安装软件包

---------------

# 安装virtualenv/virtualenvwrapper

## virtualenv、virtualenvwrapper安装

1.`sudo apt-get install pip3`  或者`sudo apt-get install python3-pip`  安装pip3，使用pip3安装virtualenv以及virtualenvwrapper，若通过apt安装，须使用python2安装，造成其安装目录及后续配置出现问题，不如使用pip3安装。

2.`pip3 install virtualenv virtualenvwrapper` 安装虚拟环境以及虚拟环境管理工具

安装完virtualenvwrapper后，需要配置：

1.创建目录用来存放虚拟环境：  `mkdir $HOME/.virtualenvs`   

2.在~./bashrc中添加行：`export WORKON_HOME=$HOME/.virtualenvs`

​										`export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3`

​										`source ~/.local/bin/virtualenvwrapper.sh`    **注意ubuntu18.04中，通过pip安装virtualenvwrapper得到的virtualenvwrapper.sh被安装在~/.local/bin/目录下，别的版本可能在/usr/local/bin/virtualenvwrapper.sh目录下**

3.运行：`source ~/.bashrc`

------

## virtualenvwrapper 命令

- 创建虚拟环境：`mkvirtualenv new_env`
- 使用虚拟环境：`workon new_env`
- 退出虚拟环境：`deactivate`
- 删除虚拟环境: `rmvirtualenv new_env`
- 查看所有虚拟环境：`lsvirtualenv`

------------------

## mkvirtualenv

 ` mkvirtualenv py1` 该语句创建的虚拟环境py1中，既含有python2解释器，也含有python3解释器（pip --version为python2，pip3 --version为python3，pip2 --version为python2）

`mkvirtualenv py2 -p /usr/bin/python2` 该语句创建的虚拟环境py2中，既含有python2解释器，也含有python3解释器（pip --version为python2，pip3 --version为python3，pip2 --version为python2）

`mkvirtualenv py3 -p /usr/bin/python3`  该语句创建的虚拟环境py3中，只含有python3解释器（pip --version为python3，pip3 --version为python3，pip2 --version无显示内容）

--------------------

## 情况1：ubuntu中只有python3

1.安装pip3

`sudo apt install python3-pip`

2.使用pip3安装`virtualenv`及`virtualenvwrapper`

`pip3 install virtualenv virtualenvwrapper`

3.配置`virtualenvwrapper`

​	①`mkdir $HOME/.virtualenvs`   

​	② 在~./bashrc中添加行：`export WORKON_HOME=$HOME/.virtualenvs`

​						 			  	`export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3`

​										  `source ~/.local/bin/virtualenvwrapper.sh`

4.apt安装`virtualenv`及`virtualenvwrapper`

`sudo apt install virtualenv virtualenvwrapper`   `apt`安装的版本低于`pip3`安装的版本

---------------

## 情况2：:创建虚拟环境时提示bash: /usr/local/bin/virtualenvwrapper.sh: 没有那个文件或目录 的解决办法

Ubuntu安装了2.7和3.x两个版本的python,在安装时使用的是`sudo pip3 install virtualenvwrapper`
运行的时候默认使用的是python2.x,但在python2.x中不存在对应的模块。
(virtualenvwrapper.sh文件内容如下:)：

```bash
if [ "$VIRTUALENVWRAPPER_PYTHON" = "" ] then
VIRTUALENVWRAPPER_PYTHON="$(command \which python)"
fi
```

解决方法：

修改`virtualenvwrapper.sh`文件

`which virtualenvwrapper.sh`    找到文件路径
在文件路径下  `/usr/local/bin/virtualenvwrapper.sh`  

​						`sudo vim virtualenvwrapper.sh`

修改：

```bash
if [ "$VIRTUALENVWRAPPER_PYTHON" = "" ] then
VIRTUALENVWRAPPER_PYTHON="$(command \which python3)"
fi
```

------

# 安装MySQL

```
https://www.linuxidc.com/Linux/2018-11/155408.htm			如何在Ubuntu 18.04中安装MySQL 8.0数据库服务器
```

### 1.apt源安装MySQL   Server

​		`sudo  apt-get update`			更新源

​		`sudo apt-get install mysql-server`		安装数据库

### 2.配置MySQL	Server

​		`sudo	mysql_secure_installation`		安装配置		此过程中要设置root密码

​		`sudo	sysytemctl 	status	 mysql.service`		检查MySQL服务状态	

​		`sudo systemctl enable mysql`				启动MySQL服务

#### 创建及配置用户

​		`sudo	mysql	-uroot	-p`			root用户进入

​		`GRANT ALL PRIVILEGES ON *.* TO root@localhost IDENTIFIED BY "123456"	`		以root进入mysql后也可用命令给root设置密码

​		`CREATE DATABASE weixx`			创建数据库weixx

​		`GRANT ALL PRIVILEGES ON weixx.* TO wxx@localhost IDENTIFIED BY "654321"`		创建用户wxx(密码654321) 并赋予其weixx数据库的所有权限

​		`FLUSH PRIVILIGES`		修改生效

#### 进行远程访问或控制配置

​		`GRANT ALL PRIVILEGES ON weixx.* TO wxx@"%" IDENTIFIED BY "654321"`		允许wxx用户可以从任意机器上登入mysql

​		`sudo gedit /etc/mysql/my.cnf`			打开此文件，并在最后一行添加：`>skip-networking => # skip-networking`，允许其他机器访问MySQL








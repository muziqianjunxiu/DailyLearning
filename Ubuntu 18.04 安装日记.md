# Ubuntu 18.04 安装日记

​		虚拟机内安装

​		`sudo 	passwd	root`		为root用户添加密码

## 		更新源为阿里云：

​						`lsb_release	 -a`			查看自己的`ubuntu`系统的`codename`代号，需要选择阿里给出的对应你机器`codename`的源（一般有`bionic`和`trusty`等等）

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

​									按`‘ESC'`键，进入命令行模式，按`“：wq ”`	保存退出

​						`sudo 	apt-get		update`		更新源文件

​						此时就可以利用阿里源进行`sudo apt install***`安装各种软件了，例如：

​						`sudo 	apt 	install		vim`		安装vim
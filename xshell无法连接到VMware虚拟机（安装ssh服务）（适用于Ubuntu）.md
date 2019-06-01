## 使用Xshell连接不上解决

ip没问题，可以ping外网，可以pingNAT模式下宿主机和其他虚拟机，但是就是不能XShell远程登录。试了防火墙以及端口，都没用。最后发现，问题出在ubuntu18.04默认未安装ssh服务，所以使用XShell连接不上。而RHEL则没有这个问题，ip配置好了后XShell瞬间连上。

`sudo	apt-get	install	openssh-server`			安装ssh服务

`vim	/etc/ssh/sshd_condig`		修改ssh服务的配置文件：

​			将	'`#Port   22`'											改为  '`Port	22`'

​			将	'`#LoginGraceTime	2m`								改为		 '	`LoginGraceTime	2m`

​					`#PermitRootLogin Prohibit-password`						`#PermitRootLogin Prohibit-password`

​																											`PermitRootLogin	yes`

​					`#StrictModes	yes`													`StrictModes	yes`

​					`#MaxAuthTries	6`														`MaxAuthTries	6`

​					`#MaxSessions	10`'												  	`MaxSessions	10`'

`sudo	service	sshd	restart`		重启ssh服务

`sudo	ps -e  |  grep  ssh`		查看ssh是否启动

`ifconfig`		查看ip地址，具体ubuntu配置ip详见下文

`ping	baidu.com`			查看能否连接外网

接下来就可以在XShell中远程连接了
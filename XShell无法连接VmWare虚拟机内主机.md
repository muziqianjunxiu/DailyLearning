# xshell无法连接到VMware虚拟机（安装ssh服务）（适用于Ubuntu）

ip没问题，可以ping外网，可以pingNAT模式下宿主机和其他虚拟机，但是就是不能XShell远程登录。试了防火墙以及端口，都没用。最后发现，问题出在ubuntu18.04默认未安装ssh服务，所以使用XShell连接不上。而RHEL则没有这个问题，ip配置好了后XShell瞬间连上。

`sudo	apt-get	install	openssh-server`			安装ssh服务

`vim	/etc/ssh/sshd_condig`		修改ssh服务的配置文件：

​			将	'`#Port   22`'											改为  '`Port	22`'

​			将	`#LoginGraceTime	2m`								

​					`#PermitRootLogin Prohibit-password`						

​					`#StrictModes	yes`													

​					`#MaxAuthTries	6`														

​					`#MaxSessions	10`'												  	

改为		 	`LoginGraceTime	2m`

​					`#PermitRootLogin Prohibit-password`

​					`PermitRootLogin	yes`

​					`StrictModes	yes`

​					MaxAuthTries	6`

​					`MaxSessions	10`

`sudo	service	sshd	restart`		重启ssh服务

`sudo	ps -e  |  grep  ssh`		查看ssh是否启动

`ifconfig`		查看ip地址，具体ubuntu配置ip详见下文

`ping	baidu.com`			查看能否连接外网

接下来就可以在XShell中远程连接了

# xshell无法连接到VMware虚拟机（ip地址配置）（适用于RHEL，不适用于ubuntu，此时Vmware设置的是NAT模式）

`ip 	addr`，查看本机IP， 检查是否有192.168.这类的ip地址

 `ls    /etc/sysconfig/network-scripts/`    查看网卡列表，一般默认第一个就是你电脑的网卡

`cat 	/etc/sysconfig/network-scripts/ifcfg-ens33` 查看网卡信息。（ifcfg-ens33是我的网卡，着这里大家要输入自己的网卡，下面操作同样是输入自己的网卡）。

`vi  /etc/sysconfig/network-scripts/ifcfg-ens33`    编辑配置文件，把ONBOOT=no的no改成yes

`service network restart` 		重启网络。当右下角出现【ok】，就成功了。

`ip  addr`  		这时就会在你的网卡下出现192.168.这类的IP地址。这时打开xshell输入上图配置的IP地址即可（没有/24)。

原文：https://blog.csdn.net/weixin_42272901/article/details/80574198 


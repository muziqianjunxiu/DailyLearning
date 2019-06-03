# xshell无法连接到VMware虚拟机（ip地址配置）（适用于RHEL，不适用于ubuntu，此时Vmware设置的是NAT模式）

`ip 	addr`，查看本机IP， 检查是否有192.168.这类的ip地址

 `ls    /etc/sysconfig/network-scripts/`    查看网卡列表，一般默认第一个就是你电脑的网卡

`cat 	/etc/sysconfig/network-scripts/ifcfg-ens33` 查看网卡信息。（ifcfg-ens33是我的网卡，着这里大家要输入自己的网卡，下面操作同样是输入自己的网卡）。

`vi  /etc/sysconfig/network-scripts/ifcfg-ens33`    编辑配置文件，把ONBOOT=no的no改成yes

`service network restart` 		重启网络。当右下角出现【ok】，就成功了。

`ip  addr`  		这时就会在你的网卡下出现192.168.这类的IP地址。这时打开xshell输入上图配置的IP地址即可（没有/24)。


原文：https://blog.csdn.net/weixin_42272901/article/details/80574198 


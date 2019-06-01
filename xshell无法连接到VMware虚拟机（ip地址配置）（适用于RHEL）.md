# xshell无法连接到VMware虚拟机（ip地址配置）（适用于RHEL，不适用于ubuntu，此时Vmware设置的是NAT模式）

本人亲身经历，遇到这 类问题头大，查了大量资料，结果发现IP地址没配置。

接下来教大家如何配置虚拟机的IP地址：

1：打开虚拟机在终端输入`ip 	addr`，查看本机IP，

 检查是否有192.168.这类的ip地址

2：输入命令： `ls    /etc/sysconfig/network-scripts/`    查看网卡列表，一般默认第一个就是你电脑的网卡，下图中做标记的是我的网卡。

3：输入命令 `cat 	/etc/sysconfig/network-scripts/ifcfg-ens33` 查看有线网卡信息。（ifcfg-ens33是我的网卡，着这里大家要输入自己的网卡，下面操作同样是输入自己的网卡）。


当然我这是配置成功的.......如果大家划线的等于号后面是no，那么就对了，接下就是把no改成yes。

4：把no修改成yes。

 输入命令： `vi  /etc/sysconfig/network-scripts/ifcfg-ens33`    进入下图界面。


（1）键盘按i 键 就可以编辑网卡信息了，把ONBOOT=no的no改成yes，修改后按一下ESC键退出编辑。接下来输入：wq   

出现下图界面是时，再按一下enter键保存并退出此界面。

5：输入命令： `cat  /etc/sysconfig/network-scripts/ifcfg-enp6s0` 查看是否修改成功。

如果界面和第3的图片界面一样，就修改成功了（就是修改成ONBOOT=yes）。

6：输入命令： `ip  addr` 查看ip。

估计大家还是没有找到192.168.这类的IP地址，这时需要重启网络。

7：输入命令: `service network restart` 重启网络。当右下角出现【ok】，就成功了。


这时输入ip  addr  就会在你的网卡下出现192.168.这类的IP地址。如图：


这时打开xshell输入上图配置的IP地址即可（没有/24)。

8：xshell创建会话：文件—>新建会话，在按图输入端口号，主机（刚刚配置的ip）协议SSH，名称可以自己设置（我设置的131）。

点确认，就会在xshell中出现你自己设定的名称的会话，见连接，出现下面界面就代表成功了


大功告成，打完收工，快凌晨12点了，
--------------------- 
作者：跳舞_小丑 
来源：CSDN 
原文：https://blog.csdn.net/weixin_42272901/article/details/80574198 
版权声明：本文为博主原创文章，转载请附上博文链接！
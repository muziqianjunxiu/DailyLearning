# Ubuntu软件包安装、卸载和删除

```txt
一、Ubuntu中软件安装方法
1、APT方式
（1）普通安装：apt-get install softname1 softname2 …;
（2）修复安装：apt-get -f install softname1 softname2... ;(-f Atemp to correct broken dependencies)
（3）重新安装：apt-get --reinstall install softname1 softname2...;
2、Dpkg方式
（1）普通安装：dpkg -i package_name.deb
3、源码安装（.tar、tar.gz、tar.bz2、tar.Z）
首先解压缩源码压缩包然后通过tar命令来完成
a．解xx.tar.gz：tar zxf xx.tar.gz 
b．解xx.tar.Z：tar zxf xx.tar.Z 
c．解xx.tgz：tar zxf xx.tgz 
d．解xx.bz2：bunzip2 xx.bz2 
e．解xx.tar：tar xf xx.tar
然后进入到解压出的目录中，建议先读一下README之类的说明文件，因为此时不同源代码包或者预编译包可能存在差异，然后建议使用ls -F --color或者ls -F命令（实际上我的只需要 l 命令即可）查看一下可执行文件，可执行文件会以*号的尾部标志。
一般依次执行	   ./configure
            	make
                sudo make install
即可完成安装。

二、Ubuntu中软件包的卸载方法
1、APT方式
（1）移除式卸载：apt-get remove softname1 softname2 …;（移除软件包，当包尾部有+时，意为安装）
（2）清除式卸载 ：apt-get --purge remove softname1 softname2...;(同时清除配置)
    清除式卸载：apt-get purge sofname1 softname2...;(同上，也清除配置文件)
2、Dpkg方式
（1）移除式卸载：dpkg -r pkg1 pkg2 ...;
（2）清除式卸载：dpkg -P pkg1 pkg2...;

三、Ubuntu中软件包的查询方法
Dpkg 使用文本文件来作为数据库.通称在 /var/lib/dpkg 目录下. 通称在 status 文件中存储软件状态,和控制信息. 在 info/ 目录下备份控制文件, 并在其下的 .list 文件中记录安装文件清单, 其下的 .mdasums 保存文件的 MD5 编码.
体验使用数据库的时刻到了:
    $ dpkg -l Desired=Unknown/Install/Remove/Purge/Hold | Status=Not/Installed/Config-files/Unpacked/Failed-config/Half-installed |/ Err?=(none)/Hold/Reinst-required/X=both-problems (Status,Err: uppercase=bad) ||/ Name Version Description +++-===========-================-======================================== ii aalib1 1.4p5-28 ascii art library - transitional package ii adduser 3.85 Add and remove users and groups ii alien .63 install non-native packages with dpkg ... ... 
每条记录对应一个软件包, 注意每条记录的第一, 二, 三个字符. 这就是软件包的状态标识, 后边依此是软件包名称, 版本号, 和简单描述.
    第一字符为期望值,它包括:
        u 状态未知,这意味着软件包未安装,并且用户也未发出安装请求.
        i 用户请求安装软件包.
        r 用户请求卸载软件包.
        p 用户请求清除软件包.
        h 用户请求保持软件包版本锁定.
    第二列,是软件包的当前状态.此列包括软件包的六种状态.
        n 软件包未安装.
        i 软件包安装并完成配置.
        c 软件包以前安装过,现在删除了,但是它的配置文件还留在系统中.
        u 软件包被解包,但还未配置.
        f 试图配置软件包,但是失败了.
        h 软件包安装,但是但是没有成功.
    第三列标识错误状态,可以总结为四种状态. 第一种状态标识没有问题,为空. 其它三种符号则标识相应问题.
        h 软件包被强制保持,因为有其它软件包依赖需求,无法升级.
        r 软件包被破坏,可能需要重新安装才能正常使用(包括删除).
        x 软包件被破坏,并且被强制保持.
也可以以统配符模式进行模糊查询, 比如我要查找以nano字符开始的所有软件包:
    $ dpkg -l nano* Desired=Unknown/Install/Remove/Purge/Hold | Status=Not/Installed/Config-files/Unpacked/Failed-config/Half-installed |/ Err?=(none)/Hold/Reinst-required/X=both-problems (Status,Err: uppercase=bad) ||/ Name Version Description +++-==============-==============-============================================ ii nano 1.3.10-2 free Pico clone with some new features pn nano-tiny <none> (no description available) un nanoblogger <none> (no description available) 
以上状态说明: 系统中安装了 nano 版本为 1.3.10-2 ;安装过 nano-tiny , 后来又清除了; 从未安装过nanoblogger 
如果觉得 dpkg 的参数过多, 不利于记忆的话, 完全可以使用 dpkg-query 进行 dpkg 数据库查询.
应用范例:
    查询系统中属于nano的文件:
        $ dpkg --listfiles nano
    or
        $ dpkg-query -L nano
    查看软件nano的详细信息:
        $ dpkg -s nano
    or
        $ dpkg-query -s nano
    查看系统中软件包状态, 支持模糊查询:
        $ dpkg -l
    or
        $dpkg-query -l
    查看某个文件的归属包:
        $ dpkg-query -S nano
    or
        $ dpkg -S nano

四、其他应用总结
apt-cache search # ------(package 搜索包)
apt-cache show #------(package 获取包的相关信息，如说明、大小、版本等)
apt-get install # ------(package 安装包)
apt-get install # -----(package --reinstall 重新安装包)
apt-get -f install # -----(强制安装, "-f = --fix-missing"当是修复安装吧...)
apt-get remove #-----(package 删除包)
apt-get remove --purge # ------(package 删除包，包括删除配置文件等)
apt-get autoremove --purge # ----(package 删除包及其依赖的软件包+配置文件等（只对6.10有效，强烈推荐）)
apt-get update #------更新源
apt-get upgrade #------更新已安装的包
apt-get dist-upgrade # ---------升级系统
apt-get dselect-upgrade #------使用 dselect 升级
apt-cache depends #-------(package 了解使用依赖)
apt-cache rdepends # ------(package 了解某个具体的依赖,当是查看该包被哪些包依赖吧...)
apt-get build-dep # ------(package 安装相关的编译环境)
apt-get source #------(package 下载该包的源代码)
apt-get clean && apt-get autoclean # --------清理下载文件的存档 && 只清理过时的包
apt-get check #-------检查是否有损坏的依赖
dpkg -S filename -----查找filename属于哪个软件包
apt-file search filename -----查找filename属于哪个软件包
apt-file list packagename -----列出软件包的内容
apt-file update --更新apt-file的数据库

dpkg --info "软件包名" --列出软件包解包后的包名称.
dpkg -l --列出当前系统中所有的包.可以和参数less一起使用在分屏查看. (类似于rpm -qa)
dpkg -l |grep -i "软件包名" --查看系统中与"软件包名"相关联的包.
dpkg -s 查询已安装的包的详细信息.
dpkg -L 查询系统中已安装的软件包所安装的位置. (类似于rpm -ql)
dpkg -S 查询系统中某个文件属于哪个软件包. (类似于rpm -qf)
dpkg -I 查询deb包的详细信息,在一个软件包下载到本地之后看看用不用安装(看一下呗).
dpkg -i 手动安装软件包(这个命令并不能解决软件包之前的依赖性问题),如果在安装某一个软件包的时候遇到了软件依赖的问题,可以用apt-get -f install在解决信赖性这个问题.
dpkg -r 卸载软件包.不是完全的卸载,它的配置文件还存在.
dpkg -P 全部卸载(但是还是不能解决软件包的依赖性的问题)
dpkg -reconfigure 重新配置

apt-get install
下载软件包，以及所有依赖的包，同时进行包的安装或升级。如果某个包被设置了 hold (停止标志，就会被搁在一边(即不会被升级)。更多 hold 细节请看下面。
apt-get remove [--purge]
移除 以及任何依赖这个包的其它包。
--purge 指明这个包应该被完全清除 (purged) ，更多信息请看 dpkg -P。

apt-get update
升级来自 Debian 镜像的包列表，如果你想安装当天的任何软件，至少每天运行一次，而且每次修改了
/etc/apt/sources.list 後，必须执行。

apt-get upgrade [-u]
升 级所有已经安装的包为最新可用版本。不会安装新的或移除老的包。如果一个包改变了依赖关系而需要安装一个新的包，那么它将不会被升级，而是标志为 hold。apt-get update 不会升级被标志为 hold 的包 (这个也就是 hold 的意思)。请看下文如何手动设置包为 hold。我建议同时使用 '-u' 选项，因为这样你就能看到哪些包将会被升级。

apt-get dist-upgrade [-u]
和 apt-get upgrade 类似，除了 dist-upgrade 会安装和移除包来满足依赖关系。因此具有一定的危险性。

apt-cache search
在软件包名称和描述中，搜索包含xxx的软件包。

apt-cache show
显示某个软件包的完整的描述。

apt-cache showpkg
显示软件包更多细节，以及和其它包的关系。

dselect
console-apt
aptitude
gnome-apt
APT 的几个图形前端(其中一些在使用前得先安装)。这里 dselect 无疑是最强大的，也是最古老，最难驾驭。

普通 Dpkg 用法
dpkg -i
安装一个 Debian 包文件，如你手动下载的文件。
dpkg -c
列出 的内容。
dpkg -I
从 中提取包信息。
dpkg -r
移除一个已安装的包。
dpkg -P
完全清除一个已安装的包。和 remove 不同的是，remove 只是删掉数据和可执行文件，purge 另外还删除所有的配制文件。
dpkg -L
列出 安装的所有文件清单。同时请看 dpkg -c 来检查一个 .deb 文件的内容。
dpkg -s
显示已安装包的信息。同时请看 apt-cache 显示 Debian 存档中的包信息，以及 dpkg -I 来显示从一个 .deb 文件中提取的包信息。
dpkg-reconfigure
重 新配制一个已经安装的包，如果它使用的是 debconf (debconf 为包安装提供了一个统一的配制界面)。你能够重新配制 debconf 它本身，如你想改变它的前端或提问的优先权。例如，重新配制 debconf，使用一个 dialog 前端，简单运行：
dpkg-reconfigure --frontend=dialog debconf (如果你安装时选错了，这里可以改回来哟：)

echo " hold" | dpkg --set-selections
设置 的状态为 hlod (命令行方式)

dpkg --get-selections ""
取的 的当前状态 (命令行方式)

支持通配符，如：
Debian:~# dpkg --get-selections *wine*
libwine hold
libwine-alsa hold
libwine-arts hold
libwine-dev hold
libwine-nas hold
libwine-print hold
libwine-twain hold
wine hold
wine+ hold
wine-doc hold
wine-utils hold

例如：
大家现在用的都是 gaim-0.58 + QQ-plugin，为了防止 gaim 被升级，我们可以采用如下方法：

方法一：
Debian:~# echo "gaim hold" | dpkg --set-selections
然後用下面命令检查一下：
Debian:~# dpkg --get-selections "gaim"
gaim hold
现在的状态标志是 hold，就不能被升级了。

如果想恢复怎么办呢?
Debian:~# echo "gaim install" | dpkg --set-selections
Debian:~# dpkg --get-selections "gaim"
gaim install
这时状态标志又被重置为 install，可以继续升级了。

同志们会问，哪个这些状态标志都写在哪个文件中呢?
在 /var/lib/dpkg/status 里，你也可以通过修改这个文件实现 hold。

有时你会发现有的软件状态标志是 purge，不要奇怪。
如：事先已经安装了 amsn，然後把它卸了。
apt-get remove --purge amsn
那么状态标志就从 install 变成 purge。

方法二：
在/etc/apt 下手动建一个 preferences 文件
内容：
Package: gaim
Pin: version 0.58*
保存

dpkg -S
在包数据库中查找 ，并告诉你哪个包包含了这个文件。(注：查找的是事先已经安装的包)

--------------------------------------------
Debian的软件包管理工具命令不完全列表
--------------------------------------------
Debian系统中所有的包信息都在/var/lib/dpkg下.其中/var/lib/dpkg/info目录中保存了各个软件包的信息及管理文件.每个文件的作用如下:
以 ".conffiles"     结尾的文件记录软件包的配置列表.
以 ".list"          结尾的文件记录了软件包的文件列表,用户可在文件当中找到软件包文件的具体安装位置.
以 ".md5sums"       结尾的文件记录了md5信息,用来进行包的验证的.
以 ".config"        结尾的文件是软件包的安装配置角本.
以 ".postinst"      角本是完成Debian包解开之后的配置工作,通常用来执行所安装软件包相关的命令和服务的重新启动.
以 ".preinst"       角本在Debain解包之前运行,主要作用是是停止作用于即将升级的软件包服务直到软件包安装或和升级完成.
以 ".prerm"         脚本负责停止与软件包关联的daemon服务,在删除软件包关联文件之前执行.
以 ".postrm"        脚本负责修改软件包链接或文件关联,或删除由它创建的文件.

/var/lib/dpkg/available是软件包的描述信息.
包括当前系统中所有使用的Debian安装源中所有的软件包,还包括当前系统中已经安装和未安装的软件包.
          
1.dpkg包管理工具
dpkg -r 卸载软件包.不是完全的卸载,它的配置文件还存在.
dpkg --info "软件包名" --列出软件包解包后的包名称.
dpkg -l     --列出当前系统中所有的包.可以和参数less一起使用在分屏查看.
dpkg -l |grep -i "软件包名" --查看系统中与"软件包名"相关联的包.
dpkg -s   查询已安装的包的详细信息. dpkg -L   查询系统中已安装的软件包所安装的位置.
dpkg -S   查询系统中某个文件属于哪个软件包.
dpkg -I   查询deb包的详细信息,在一个软件包下载到本地之后看看用不用安装(看一下呗).
dpkg -i 手动安装软件包(这个命令并不能解决软件包之前的依赖性问题),如果在安装某一个软件包的时候遇到了软件依赖的问题,可以用apt-get -f install在解决信赖性这个问题.
dpkg -reconfigure 重新配置 
dpkg -P 全部卸载(但是还是不能解决软件包的依赖性的问题)

2. apt高级包管理工具
   (1)GTK图形的"synaptic",这是APT的前端工具.
   (2)"aptitude",这也是APT的前端工具.
   用APT管理工具进行包的管理,可以有以下几种方法做源:
   (1)拿安装盘做源,方法如下:
        apt-cdrom ident        扫描光盘的信息
        apt-cdrom add          添加光盘源
   (2)这也是最常用的方法就是把源添加到/etc/apt/source.list中,之后更新列apt-get update
   
APT管理工具常用命令
apt-cache 加上不同的子命令和参数的使用可以实现查找,显示软件,包信息及包信赖关系等功能.
apt-cache stats 显示当前系统所有使用的Debain数据源的统计信息.
apt-cache search +"包名",可以查找相关的软件包.
apt-cache show   +"包名",可以显示指定软件包的详细信息.
apt-cache depends +"包名",可以查找软件包的依赖关系.
apt-get upgrade   更新系统中所有的包到最新版
apt-get install   安装软件包
apt-get --reindtall install 重新安装软件包
apt-get remove 卸载软件包
apt-get --purge remove 完全卸载软件包
apt-get clean 清除无用的软件包
在用命令apt-get install之前,是先将软件包下载到/var/cache/apt/archives中,之后再进行安装的.所以我们可以用apt-get clean清除/var/cache/apt/archives目录中的软件包.

源码包安装
   apt-cache showsrc 查找看源码包的文件信息(在下载之前)
   apt-get source 下载源码包.
   apt-get build-dep +"包名" 构建源码包的编译环境.

清除处于rc状态的软件包
dpkg -l |grep ^rc|awk '{print $2}' |tr ["\n"] [" "] | sudo xargs dpkg -P -

转载自： http://www.cnblogs.com/forward/archive/2012/01/10/2318483.html
```

```txt
dpkg -l					查看所有安装包		
dpkg -l nano		 	查看软件包nano的状态
dpkg -s nano			查看软件包nano的详细信息
dpkg-query	-L nano		查看系统中属于nano的文件
dpkg --info "软件包名"	 --列出软件包解包后的包名称.
dpkg -l 				--列出当前系统中所有的包.可以和参数less一起使用在分屏查看. (类似于rpm -qa)
dpkg -l |grep -i "软件包名" 	--查看系统中与"软件包名"相关联的包.
dpkg -s 				查询已安装的包的详细信息.
dpkg -L 				查询系统中已安装的软件包所安装的位置. (类似于rpm -ql)
dpkg -S 				查询系统中某个文件属于哪个软件包. (类似于rpm -qf)
dpkg -I 				查询deb包的详细信息,在一个软件包下载到本地之后看看用不用安装(看一下呗).
dpkg -i 				手动安装软件包(这个命令并不能解决软件包之前的依赖性问题),如果在安装某一个软件包的时候遇到了软件依赖的问题,可以用apt-get -f install在解决信赖性这个问题.
dpkg -r 				卸载软件包.不是完全的卸载,它的配置文件还存在.
dpkg -P 				全部卸载包括配置文件(但是还是不能解决软件包的依赖性的问题)
dpkg -reconfigure 		重新配置

```

# rpm包和deb包区别

```txt
一般来说著名的linux系统基本上分两大类：
1.RedHat系列：Redhat、Centos、Fedora等
rpm是redhat公司的一种软件包管理机制(redhat package managment)，直接通过rpm命令进行安装删除等操作，最大的优点是自己内部自动处理了各种软件包可能的依赖关系。 使用yum或dnf管理。
2.Debian系列：Debian、Ubuntu等
deb包可以把一个应用的文件包在一起，大体就如同Windows上的安装文件。apt是Debian Linux发行版中的APT软件包管理工具。所有基于Debian的发行都使用这个包管理系统。使用apt或dpkg管理。

最初只有.tar.gz的打包文件，用户必须编译每个他想在GNU/Linux上运行的软件。用户们普遍认为系统很有必要提供一种方法来管理这些安装在机器上的软件包，当Debian诞生时，这样一个管理工具也就应运而生，它被命名为dpkg。从而著名的“package”概念第一次出现在GNU/Linux系统中，稍后RedHat才决定开发自己的“rpm”包管理系统。
很快一个新的问题难倒了GNU/Linux制作者，他们需要一个快速、实用、高效的方法来安装软件包，当软件包更新时，这个工具应该能自动管理关联文件和维护已有配置文件，再次，Debian率先解决了这个难题，APT（Advanced Packaging Tool）诞生了。APT后来还被Conectiva改造用来管理rpm，并被其它Linux发行版本采用为它们的软件包管理工具。
也就是说先有apt,才有的rpm。
debian率先解决的软件包管理这个问题。rpm这些都是后起的。 
```

# Linux 包管理基础：apt、yum、dnf 和 pkg

```
https://linux.cn/article-8782-1.html			Linux 包管理基础：apt、yum、dnf 和 pkg
包管理系统：简要概述
大多数包系统都是围绕包文件的集合构建的。包文件通常是一个存档文件，它包含已编译的二进制文件和软件的其他资源，以及安装脚本。包文件同时也包含有价值的元数据，包括它们的依赖项，以及安装和运行它们所需的其他包的列表。
虽然这些包管理系统的功能和优点大致相同，但打包格式和工具却因平台而异：
操作系统 			格式 					工具
Debian 				.deb 				apt, apt-cache, apt-get, dpkg
Ubuntu 				.deb 				apt, apt-cache, apt-get, dpkg
CentOS 				.rpm 				yum
Fedora 				.rpm 				dnf
FreeBSD 			Ports, .txz 		make, pkg

Debian 及其衍生版，如 Ubuntu、Linux Mint 和 Raspbian，它们的包格式是 .deb。APT 这款先进的包管理工具提供了大多数常见的操作命令：搜索存储库、安装软件包及其依赖项，并管理升级。在本地系统中，我们还可以使用 dpkg 程序来安装单个的 deb 文件，APT 命令作为底层 dpkg 的前端，有时也会直接调用它。
最近发布的 debian 衍生版大多数都包含了 apt 命令，它提供了一个简洁统一的接口，可用于通常由 apt-get 和 apt-cache 命令处理的常见操作。这个命令是可选的，但使用它可以简化一些任务。
CentOS、Fedora 和其它 Red Hat 家族成员使用 RPM 文件。在 CentOS 中，通过 yum 来与单独的包文件和存储库进行交互。
在最近的 Fedora 版本中，yum 已经被 dnf 取代，dnf 是它的一个现代化的分支，它保留了大部分 yum的接口。
FreeBSD 的二进制包系统由 pkg 命令管理。FreeBSD 还提供了 Ports 集合，这是一个存在于本地的目录结构和工具，它允许用户获取源码后使用 Makefile 直接从源码编译和安装包。

更新包列表
大多数系统在本地都会有一个和远程存储库对应的包数据库，在安装或升级包之前最好更新一下这个数据库。另外，yum 和 dnf 在执行一些操作之前也会自动检查更新。当然你可以在任何时候对系统进行更新。
系统 								命令
Debian / Ubuntu 				sudo apt-get update
								sudo apt update
CentOS 							yum check-update
Fedora 							dnf check-update
FreeBSD Packages 				sudo pkg update
FreeBSD Ports 					sudo portsnap fetch update

更新已安装的包
在没有包系统的情况下，想确保机器上所有已安装的软件都保持在最新的状态是一个很艰巨的任务。你将不得不跟踪数百个不同包的上游更改和安全警报。虽然包管理器并不能解决升级软件时遇到的所有问题，但它确实使你能够使用一些命令来维护大多数系统组件。
在 FreeBSD 上，升级已安装的 ports 可能会引入破坏性的改变，有些步骤还需要进行手动配置，所以在通过 portmaster 更新之前最好阅读下 /usr/ports/UPDATING 的内容。
系统 						命令 								说明
Debian / Ubuntu 		 sudo apt-get upgrade 			只更新已安装的包
						sudo apt-get dist-upgrade 		可能会增加或删除包以满足新的依赖项
						sudo apt upgrade 				和 apt-get upgrade 类似
						sudo apt full-upgrade 			和 apt-get dist-upgrade 类似
CentOS 					sudo yum update 	
Fedora 					sudo dnf upgrade 	
FreeBSD Packages 			sudo pkg upgrade 	
FreeBSD Ports 			less /usr/ports/UPDATING 	使用 less 来查看 ports 的更新提示（使用上下光标键滚动，按 q 退出）。
					cd /usr/ports/ports-mgmt/portmaster && sudo make install && sudo portmaster -a 			安装 portmaster 然后使用它更新已安装的 ports

搜索某个包
大多数发行版都提供针对包集合的图形化或菜单驱动的工具，我们可以分类浏览软件，这也是一个发现新软件的好方法。然而，查找包最快和最有效的方法是使用命令行工具进行搜索。
系统 						命令 									说明
Debian / Ubuntu 	apt-cache search search_string 	
					apt search search_string 	
CentOS 					yum search search_string 	
					yum search all search_string 			搜索所有的字段，包括描述
Fedora 					dnf search search_string 	
					dnf search all search_string 			搜索所有的字段，包括描述
FreeBSD Packages 	pkg search search_string 				通过名字进行搜索
					pkg search -f search_string 		通过名字进行搜索并返回完整的描述
					pkg search -D search_string 				搜索描述
FreeBSD Ports 		cd /usr/ports && make search name=package 		通过名字进行搜索
				cd /usr/ports && make search key=search_string 		搜索评论、描述和依赖

查看某个软件包的信息
在安装软件包之前，我们可以通过仔细阅读包的描述来获得很多有用的信息。除了人类可读的文本之外，这些内容通常包括像版本号这样的元数据和包的依赖项列表。
系统 							命令 						说明
Debian / Ubuntu 	apt-cache show package 			显示有关包的本地缓存信息
					apt show package 	
					dpkg -s package 				显示包的当前安装状态
CentOS 				yum info package 	
					yum deplist package 			列出包的依赖
Fedora 				dnf info package 	
					dnf repoquery --requires package 		列出包的依赖
FreeBSD Packages 		pkg info package 				显示已安装的包的信息
FreeBSD Ports 			cd /usr/ports/category/port && cat pkg-descr 	

从存储库安装包
知道包名后，通常可以用一个命令来安装它及其依赖。你也可以一次性安装多个包，只需将它们全部列出来即可。
系统 							命令 						说明
Debian / Ubuntu 	sudo apt-get install package 	
					sudo apt-get install package1 package2 ... 	安装所有列出来的包
					sudo apt-get install -y package 	在 apt 提示是否继续的地方直接默认 yes
					sudo apt install package 	显示一个彩色的进度条
CentOS 				sudo yum install package 	
					sudo yum install package1 package2 ... 	安装所有列出来的包
					sudo yum install -y package 	在 yum 提示是否继续的地方直接默认 yes
Fedora 				sudo dnf install package 	
					sudo dnf install package1 package2 ... 	安装所有列出来的包
					sudo dnf install -y package 	在 dnf 提示是否继续的地方直接默认 yes
FreeBSD Packages 		sudo pkg install package 	
					sudo pkg install package1 package2 ... 	安装所有列出来的包
FreeBSD Ports 		cd /usr/ports/category/port && sudo make install 	从源码构建安装一个 port

从本地文件系统安装一个包
对于一个给定的操作系统，有时有些软件官方并没有提供相应的包，那么开发人员或供应商将需要提供包文件的下载。你通常可以通过 web 浏览器检索这些包，或者通过命令行 curl 来检索这些信息。将包下载到目标系统后，我们通常可以通过单个命令来安装它。
在 Debian 派生的系统上，dpkg 用来处理单个的包文件。如果一个包有未满足的依赖项，那么我们可以使用 gdebi 从官方存储库中检索它们。
在 CentOS 和 Fedora 系统上，yum 和 dnf 用于安装单个的文件，并且会处理需要的依赖。
系统 						命令 						说明
Debian / Ubuntu 		sudo dpkg -i package.deb 	
						sudo apt-get install -y gdebi && sudo gdebi package.deb 	安装 gdebi，然后使用 gdebi 安装 package.deb 并处理缺失的依赖
CentOS 					sudo yum install package.rpm 	
Fedora 					sudo dnf install package.rpm 	
FreeBSD Packages 			sudo pkg add package.txz 	
						sudo pkg add -f package.txz 	即使已经安装的包也会重新安装

删除一个或多个已安装的包
由于包管理器知道给定的软件包提供了哪些文件，因此如果某个软件不再需要了，它通常可以干净利落地从系统中清除这些文件。
系统 							命令 							说明
Debian / Ubuntu 	sudo apt-get remove package 	
					sudo apt remove package 	
					sudo apt-get autoremove 				删除不需要的包
CentOS 				sudo yum remove package 	
Fedora 				sudo dnf erase package 	
FreeBSD Packages 		sudo pkg delete package 	
					sudo pkg autoremove 					删除不需要的包
FreeBSD Ports 		sudo pkg delete package 	
				cd /usr/ports/path_to_port && make deinstall 	卸载 port

apt 命令
Debian 家族发行版的管理员通常熟悉 apt-get 和 apt-cache。较少为人所知的是简化的 apt 接口，它是专为交互式使用而设计的。
传统命令 						等价的 apt 命令
apt-get update 					apt update
apt-get dist-upgrade 			apt full-upgrade
apt-cache search string 		apt search string
apt-get install package 		apt install package
apt-get remove package 			apt remove package
apt-get purge package 			apt purge package
虽然 apt 通常是一个特定操作的快捷方式，但它并不能完全替代传统的工具，它的接口可能会随着版本的不同而发生变化，以提高可用性。如果你在脚本或 shell 管道中使用包管理命令，那么最好还是坚持使用 apt-get 和 apt-cache。
```

# 
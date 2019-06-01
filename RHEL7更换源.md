第一步：

`wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo`	下载CentOS-Base.repo文件 该文件会下载到 /etc/yum.repos.d 这个目录下面

第二歩：

`vim /etc/yum.repos.d/CentOS-Base.repo` 

`:%s/$releasever/7/g`								修改Centos-7.repo文件将所有$releasever替换为7

第三步：

 `yum clean all` 										清除原来的缓存

第四步：

 `yum makecache`										建立源数据缓存

整理自：    https://blog.csdn.net/zkuncn/article/details/78430601


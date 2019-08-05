Anaconda是一个用于科学计算的Python发行版，支持 Linux, Mac, Windows系统，提供了包管理与环境管理的功能，可以很方便地解决多版本python并存、切换以及各种第三方包安装问题。Anaconda利用工具/命令conda来进行package和environment的管理，并且已经包含了Python和相关的配套工具。
这里先解释下conda、anaconda这些概念的差别。conda可以理解为一个工具，也是一个可执行命令，其核心功能是包管理与环境管理。包管理与pip的使用类似，环境管理则允许用户方便地安装不同版本的python并可以快速切换。Anaconda则是一个打包的集合，里面预装好了conda、某个版本的python、众多packages、科学计算工具等等，所以也称为Python的一种发行版。其实还有Miniconda，顾名思义，它只包含最基本的内容——python与conda，以及相关的必须依赖项，对于空间要求严格的用户，Miniconda是一种选择。

conda的设计理念——conda将几乎所有的工具、第三方包都当做package对待，甚至包括python和conda自身！因此，conda打破了包管理与环境管理的约束，能非常方便地安装各种版本python、各种package并方便地切换。

 它提供了两个关键功能：

- ：提供包管理，功能类似于 pip，Windows 平台安装第三方包经常失败的场景得以解决。
- ：提供虚拟环境管理，功能类似于 virtualenv，解决了多版本Python并存问题。



Anaconda作为Python的一个发行版，下载安装简单，[点击此处](https://www.anaconda.com/download/)进入官网下载相应的版本，安装即可。
Anaconda提供了一个强大的conda工具，用以包管理和环境管理，包管理与pip类似；环境管理则与许多第三方虚拟环境管理包工具类似

virtualenv是一款轻量级第三方虚拟环境管理工具，通过pip就可以轻松安装。下面介绍virtualenv的安装使用。，一旦成功安装 virtualenv，运行 shell 创建自己的环境。我们通常会创建一个项目文件夹myproject，其下创建 env 文件夹，该文件夹就是一个虚拟的 Python 环境，同样的，我们可以使用 -p 参数来改变 python 的版本，默认情况下，virtualenv 会优先选取它的宿主 python 环境。

## Python虚拟环境之Anaconda&virtualenv

https://blog.csdn.net/hohaizx/article/details/78375238





### anaconda包管理：

安装包	`conda install package_name`

​				`conda install pandas`

卸载包：`conda remove package_name`

更新包：`conda update package_name`

​				`conda update --all`

列出已安装包：`conda list`



### anaconda环境管理：

安装nb_conda用于notebook自动关联nb_conda的环境：`conda install nb_conda`

创建环境：`conda create -n env_name package_name`		env_name 是设置环境的名称（-n 是指该命令后面的env_name是你要创建环境的名称），package_names 是你要安装在创建环境中的包名称

​				例如，要创建环境名称为 **py3** 的环境并在其中安装 numpy，在终端中输入 `conda create -n py3 pandas`。

创建环境版本：

`conda create -n py3 python=3` 	创建环境名称为py3，并安装最新版本的Python3

`conda create -n py2 python=2`	创建环境名称为py2，并安装最新版本的Python2

进入环境：

`windows：activate my_env`

`mac/linux:source activate my_env`

进入环境后，我可以用conda list 查看环境中默认安装的几个包：`conda list`

离开环境：

`windows:deactivate`

`mac/linux:conda deactivate`

导出环境：

你可以在你当前的环境中终端中使用 conda env export > environment.yaml 将你当前的环境保存到文件中包保存为YAML文件（包括Pyhton版本和所有包的名称）。

`conda env export >environment.yaml`

使用导出环境文件：

首先在conda中进入你的环境，比如activate py3

`conda env update -f=/path/to/environment.yaml`	`中-f表示你要导出文件在本地的路径，所以/path/to/environment.yml要换成你本地的实际路径`

对应于pip管理中：`pip freeze > environment.txt`,	

进入python命令环境，然后运行以下命令就可以安装该项目需要的包：`pip install -r /path/to/environment.txt`

列出环境：

`conda env list`

删除环境：

`conda env remove -n env_name`
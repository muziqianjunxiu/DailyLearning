# Anaconda

## conda工具介绍

conda是Anaconda用于包管理和环境管理的工具，功能上类似pip和virtualenv的组合。conda的包管理功能和pip是一样的，conda的环境管理和virtualenv是一样的，在conda中anything is a package 。conda本身可看做是一个包，python环境可以看做是一个包，anaconda也可以看作是一个包，所以除了普通的第三方包支持更新之外，这三个“包”也支持更新：

`conda update conda`更新conda本身

`conda update anaconda` 更新anaconda

`conda update python` 更新python

## 更改源为清华源

`conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/`

`conda config --set show_channel_urls yes`

## 包管理

### **列出已安装包**

`conda list` 

### **更新所有包**

`conda update --all`

### **更新特定包**

`conda update package_name`

### **安装特定包**

`conda install package_name`

### **删除特定包**

`conda remove package_name`

### **停用特定包**

`jupyter serverextension disable  package_name`

### **启用特定包**

`jupyter serverextension enable package_name`

### **搜索特定包**

`conda serach package_name`

## 环境管理

### **创建环境+包**

`conda create -n env_name python=版本 package_name`

### **无附带包新建环境**

`conda create -n env_with_nothing python=3` 

默认会安装以下包：

`ca-certificates	certifi		openssl		pip		python		setuptools		sqlite		vc		vs2015_runtime		wheel		wincreatstore`

### **克隆创建环境**

`conda create -n new_env_name --clone old_env_name`

### **进入环境**

`windows:activate env_name` 	

`OSX/Linux:source activate env_name`

### **退出环境**

`windows：deactivate`     

`OSX/Linux：conda deactivate`

### **列出环境**

`conda env list`

`conda info -e`

### **删除环境**

`conda env remove -n env_name`

### **查看环境细节**

`conda info`

### **导出环境文件**

`conda env export> environment.yaml`

### **导入环境文件**

`conda env update -f= /path/to/environment.yaml`

# Jupyter Notebook

## **应用相关**

### **修改notebook工作文件夹**

`jupyter notebook --generate-config`   	查看jupyter配置文件位置，修改配置文件`c.NotebookApp.notebook_dir` 这一行，如`c.NotebookApp.notebook_dir=D:\\Jupyter Notebook File Location` 。windows双斜杠\\  linux反斜杠/。记住将注释符号#去掉。

### **Anaconda Prompt控制台进入Jupyter Notebook**

`jupyter notebook`

 终端输入 `jupyter notebook`  浏览器自动打开页面地址：`http://localhost:8888`。新开服务器会在端口 8889 上运行，依次累加端口。

### **从特定端口进入**

`jupyter notebook --port 8889`

### 后台开启

`jupyter notebook --no-browser`

### **关闭服务**

终端中按两次Ctrl+C，关闭整个服务器

## **优化相关**

### **安装代码自动补全库**

`conda install pyreadline`	

### **更改notebook主题**

`pip install jupyterthemes`	安装主题

`jt -l`	加载主题列表

`jt -t <name_of_theme>`	确定主题

`jt -r`	还原初始主题

### **安装页面扩展功能**

`conda install -c conda-forge jupyter_contrib_nbextensions`

`nbextensions`是一种`JavaScript`模块，加载在`notebook`前端页面上，大大提升用户体验。执行上述命令后，启动`Jupyter Notebook`，导航栏多了“`Nbextensions`”的类目，点击“`Nbextensions`”，勾选相应扩展功能。

或者直接进`localhost:8888/nbextensions/`进行配置。

`https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/index.html`这是`nbextensions`官方文档。

# kernel相关

**创建虚拟环境，并将环境添加为kernel**

`conda create -n env_name python=版本 ipykernel`	

创建新的虚拟环境时，最好顺带安装`ipykernel`包，`ipykernel`包的作用是将当前所在`python`环境添加为`kernel`，方便jupyter notebook使用。

**查看已有内核列表**

`jupyter kernelspec list` 

**删除已有内核**

`jupyter kernelspec remove kernel_name`    

## **kernel是什么**

`A kernel provides programming language support in Jupyter. IPython is the default kernel. Additional kernels include R, Julia, and many more.`   

`kernel`就是为`jupyter notebook`提供编程语言环境支持的东西。说白了就是将语言环境（如`python`）“打包”供`jupyter notebook`使用。

环境中安装`ipykernel`即可“打包”`python`语言编辑环境作为`kernel`，安装`irkernel`则可“打包”`R`语言编辑环境。

## **kernel和notebook/spyder关系**

notebook和spyder都是编辑器，kernel是编辑器中python代码或其他代码运行的支持环境。

## **kernel和虚拟环境关系**

通过配置Jupyter下的kernel.json文件，就可以将虚拟环境映射为kernel。

## **虚拟环境如何映射为kernel**

创建环境的时候**最好**安装`ipykernel`包（如果是`R`语言，要安装`irkernel`），`ipykernel`包的作用是将当前所在环境添加为`kernel`，配置文件为`Anaconda`安装目录里的`kernel.json`。

```json
kernel.json			D:\Anaconda\share\jupyter\kernels\python3\kernel.json这是我的kernel.json文件所在位置。我的Anaconda是安装在D盘下的。
{
 "argv": [
  "D:\\Anaconda\\python.exe",
  "-m",
  "ipykernel_launcher",
  "-f",
  "{connection_file}"
 ],
 "display_name": "Python 3",
 "language": "python"
}
```

# 虚拟环境    和  Jupyter Notebook

## jupyter notebook依赖包

`attrs  	 backcall 	 bleach	 colorama 	 decorator	defusedxml 	 entrypoints	 icu 	ipykernel 	 ipython 	ipython_genutils 	ipywidgets	  jedi 	 jinja2 	 jpeg 	jsonschema 	 jupyter	jupyter_client	jupyter_console	 jupyter_core  	 libpng	libsodium 	m2w64-gcc-libgfor~	m2w64-gcc-libs	 m2w64-gcc-libs-co~	m2w64-gmp 	m2w64-libwinpthre~ 	markupsafe 	mistune 	msys2-conda-epoch	nbconvert	 nbformat 	 notebook	 pandoc	pandocfilters 	 parso 	 pickleshare	prometheus_client 	 prompt_toolkit	 pygments	  pyqt 	 pyrsistent  	 python-dateutil	pywinpty	pyzmq 	  qt 	qtconsole	 send2trash 	sip	 six 	 terminado 	testpath	tornado 	traitlets	wcwidth            pkgs/main/win-64::wcwidth-0.1.7-py37_0`	 `webencodings 	widgetsnbextension 	 winpty	zeromq	zlib` 	

可以发现：`ipykernel`是`jupyter notebook`的依赖包

## **ipykernel依赖包**

`backcall 	ca-certificates	certifi	colorama	decorama	ipykernel	ipython	ipython_genutils	jedi	jupyter_client	jupyter_core	libsodium	openssl	parso	pickleshare	pip	prompt_toolkit	pygments	python	python-dateutil	pyzmq	setuptools	six	sqlite	tornado	traitlets	vc	vs2015_runtime	wcwidth	wheel	wincertstore	zeromq`

## **nb_conda_kernels依赖包**

`attrs 	  backcall	 bleach  	ca-certificates	certifi   	colorama   	decorator     	defusedxml 	 entrypoints 	 ipykernel 	  ipython	 ipython_genutils 	jedi 	  jinja2  	 jsonschema 	  jupyter_client  	  jupyter_core	libsodium  	m2w64-gcc-libgfor~ 	 m2w64-gcc-libs 	 m2w64-gcc-libs-co~	m2w64-gmp  	  m2w64-libwinpthre~	  markupsafe	  mistune    	 msys2-conda-epoch	  nb_conda_kernels 	nbconvert 	nbformat 	 notebook	 openssl	pandoc  	  pandocfilters	 parso	pickleshare 	 pip  	  prometheus_client	 prompt_toolkit 	  pygments 	pyrsistent	 python	 python-dateutil	 pywinpty 	pyzmq	 send2trash	setuptools	 six 	 sqlite	terminado 	 testpath 	tornado 	traitlets 	vc 	vs2015_runtime	 wcwidth  	 webencodings 	 wheel	 wincertstore 	 winpty 	zeromq`

可以发现：`ipykernel`是`nb_conda_kernels`的依赖包。

## **nb_conda依赖包**

`attrs 	  backcall	 bleach  	ca-certificates	certifi   	colorama   	decorator     	defusedxml 	 entrypoints 	 ipykernel 	  ipython	 ipython_genutils 	jedi 	  jinja2  	 jsonschema 	  jupyter_client  	  jupyter_core	libsodium  	m2w64-gcc-libgfor~ 	 m2w64-gcc-libs 	 m2w64-gcc-libs-co~	m2w64-gmp  	  m2w64-libwinpthre~	  markupsafe	  mistune    	 msys2-conda-epoch	 nb_conda  	  nb_conda_kernels 	nbconvert 	nbformat 	 notebook	 openssl	pandoc  	  pandocfilters	 parso	pickleshare 	 pip  	  prometheus_client	 prompt_toolkit 	  pygments 	pyrsistent	 python	 python-dateutil	 pywinpty 	pyzmq	 send2trash	setuptools	 six 	 sqlite	terminado 	 testpath 	tornado 	traitlets 	vc 	vs2015_runtime	 wcwidth  	 webencodings 	 wheel	 wincertstore 	 winpty 	zeromq`           

可以发现：  `ipykernel和nb_conda_kernels`是`nb_conda`的依赖包

## 依赖关系

### `jupyter notebook` 

`ipykernel`是`jupyter notebook`的依赖包，所以某个环境要想使用`jupyter notebook`，在安装`jupyter notebook`的同时，已经依赖安装了`ipykernel`，效果就是将该环境映射为`kernel`，供`jupyter notebook`使用。

### `nb_conda`

`nb_conda_kernels`，`ipykernel`是`nb_conda`的依赖包，安装`nb_conda`会自动安装`nb_conda_kernels`和`ipykernel`。

`nb_conda`的作用：① 将已建环境映射为`kernel`，并添加至`jupyter notebook`页面的`kernel`列表中，供`notebook`切换使用。（通过调用`nb_conda_kernels`，`nb_conda_kernels`再调用`ipykernel`实现）；②在`jupyter notebook`页面中建立`conda`标签，方便管理`kernel`。

### `nb_conda_kernels`

`nb_conda_kernels`的作用：①将已建环境映射为`kernel`，并添加至`jupyter notebook`页面的`kernel`列表中，供`notebook`切换使用。（通过调用`ipykernel`实现）；②读取全部已映射`kernel`。

### `ipykernel`

`ipykernel`是`jupyter notebook`，`nb_conda_kernels`，`nb_conda`的依赖包，安装这三个包时会关联安装`ipykernel`。

`ipykernel`的作用：①将本环境映射为`kernel`，并添加至`jupyter notebook页`面的`kernel`列表中。②但是不能读取全部kernel，只能读取本环境kernel。

### 总结

①只要某个虚拟环境安装了`ipykernel`包，该环境则被映射为`kernel`。

②进一步，若该环境安装`nb_conda_kernels`包，则该环境下的`jupyter notebook`页面可读取所有已安装`ipykernel`包的环境`kernel`。即`kernel`列表有显示。

③再进一步，若该环境安装了`nb_conda`，不仅可实现上述①②功能，还可在`jupyter notebook`页面上添加`conda`标签。

### 调用关系

1)	`nb_conda`---------->`nb_conda_kernels`---------->`ipykernel`

2)	`jupyter notebook`----------------------------------->`ipykernel`

## Base环境

`Base`环境使用时，安装`nb_conda`包，从`base`环境进入`jupyter notebook`时，可方便使用并管理所有环境下的`kernel`。


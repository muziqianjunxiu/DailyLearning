# Linux下配置virtualenv

## python虚拟环境virtualenv安装及配置虚拟环境管理工具virtualenvwrapper

`sudo apt-get pip3`  安装pip3，使用pip3安装virtualenv以及virtualenvwrapper，若通过apt安装，须使用python2安装，造成其安装目录及后续配置出现问题，不如使用pip3安装。

`pip3 install virtualenv virtualenvwrapper` 安装虚拟环境以及虚拟环境管理工具

安装完virtualenvwrapper后，需要配置：

1.创建目录用来存放虚拟环境：  `mkdir $HOME/.virtualenvs`   

2.在~./bashrc中添加行：`export WORKON_HOME/.virtualenvs`

​										`export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3`

​										`source ~/.local/bin/virtualenvwrapper.sh`    **注意ubuntu18.04中，通过pip安装virtualenvwrapper得到的virtualenvwrapper.sh被安装在~/.local/bin/目录下，别的版本可能在/usr/local/bin/virtualenvwrapper.sh目录下**

3.运行：`source ~/.bashrc`

---------------------------------------

## ubuntu18.04创建虚拟环境时提示bash: /usr/local/bin/virtualenvwrapper.sh: 没有那个文件或目录 的解决办法

Ubuntu安装了2.7和3.x两个版本的python,在安装时使用的是sudo pip3 install virtualenvwrapper
在我运行的时候默认使用的是python2.x,但在python2.x中不存在对应的模块。
(virtualenvwrapper.sh文件内容如下:)：

```bash
if [ "$VIRTUALENVWRAPPER_PYTHON" = "" ] then
VIRTUALENVWRAPPER_PYTHON="$(command \which python)"
fi
```

解决方法：

修改virtualenvwrapper.sh文件

which virtualenvwrapper.sh    找到文件路径
在文件路径下  /usr/local/bin/virtualenvwrapper.sh  

​						sudo vim virtualenvwrapper.sh

修改：

```bash
if [ "$VIRTUALENVWRAPPER_PYTHON" = "" ] then
VIRTUALENVWRAPPER_PYTHON="$(command \which python3)"
fi
```

----------------------

### virtualenvwrapper 命令

- 创建虚拟环境：`mkvirtualenv new_env`
- 使用虚拟环境：`workon new_env`
- 退出虚拟环境：`deactivate`
- 删除虚拟环境: `rmvirtualenv new_env`
- 查看所有虚拟环境：`lsvirtualenv`
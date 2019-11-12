mkdir GP1

cd GP1

mkdir Day01

cd Day01

mkvirtualenv py3 -p /usr/bin/python3     创建虚拟环境，解释器使用python3

workon py3    切换到py3虚拟环境中

pip freeze  查看环境中已安装包

pip install django==1.11.7  安装某一特定版本，需两个等号

django-admin startproject  HelloWorld   创建一个项目

tree 

cd HelloWorld 

python manage.py startapp App   创建一个应用

tree

python manage.py runserver   启动django


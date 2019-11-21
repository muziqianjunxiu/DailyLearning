`mkdir GP1`

`cd GP1`

`mkdir Day01`

`cd Day01`

`mkvirtualenv py3 -p /usr/bin/python3`     创建虚拟环境，解释器使用`python3`

`workon py3`    切换到py3虚拟环境中

`pip freeze`  查看环境中已安装包

`pip install django==1.11.7`  安装某一特定版本，需两个等号

--------------

# python manage.py XXX

`django-admin startproject  HelloWorld`   创建一个项目

`python manage.py startapp App`   创建一个应用

`python manage.py runserver`   启动django服务

`python manage.py makemigrations`   制造ORM迁移文件

`python manage.py migrate`  应用迁移文件

`python manage.py createsuperuser`	创建admin管理员

`python manage.py changepassword username`   修改admin页面中username的登陆密码

`python manage.py flush`	清空数据库内容，只留下空表。注意该操作会将admin页面登陆用户也清除掉。

`python manage.py shell`   进入带当前python环境的shell环境，调试用

`python manage.py dbshell`	进入在setting.py中设置的数据库。shell环境。

`python manage.py dumpdata appname > appname.json`	导出数据

`python manage.py loaddata appname.json`	导入数据

-------

# models.py

```python 
from django.db import models
class Person(models.Model):
	name = models.CharField(max_length=20)
	age = models.IntegerField()
```

逻辑删除：对于重要数据，不做物理删除，做逻辑删除，实现方法是定义isDelete属性，类型为BooleanField，默认值为False

## 字段类型：

### AutoField

​		一个根据实际ID自动增长的IntegerField，通常不指定，如果不指定，一个主键字段将自动添加到模型中

### CharField（max_length=字符长度）

​		字符串，默认的表单样式是TextInput

### TextField

​		大文本字段，一般超过4000使用，默认表单控件是Textarea

### IntegerField

​		整数

### DecimalField(max_digits=None,decimal_palces=None)

​		使用python的Decimal实例表示的十进制浮点数，高精度的计算用

​		参数说明：DecimalField.max_digits  位数总数			

​						  DecimalField.decimal_places    小数点后的数字位数

### FloatField

​		用python的float实例来表示的浮点数

### BooleanField

​		true/false字段，此字段的默认表单控制是CheckboxInput

### NullBooleanField

​		支持null、true、false三中值

### DateField([auto_now=False,auto_now_add=False])

​		使用python的datetime.date实例表示的日期

​		参数说明：

​				DateField.auto_now：每次保存对象时，自动设置该字段为当前时间，用于“最后一次修改”的时间戳，它总是使用当前日期，默认为false

​				DateField.auto_now_add：当对象第一次被创建时自动设置为当前时间，用于创建的时间戳，总是使用当前日期，默认为false

### TimeField

​		使用python的datetime.time实例表示的时间，参数同DateField

### DateTimeField

​		使用python的datetime.time实例表示的日期和时间，参数同DateField

### FileField

​		一个上传文件的字段

### ImageField

​		继承了FileField的所有属性和方法，但对上传的对象进行校验，确保是个有效的image







-----

# views.py

```python
from django.http import HttpResponse

def index(request):
    return HttpResponse("Welcome!")
```



## 新建对象的方法：

  1.  `Person.objects.create(name = anyname,age = anyage)`

  2.  `Person.objects.get_or_create(name = "anyname", age = 23 )`

		3.  `p = Person(name = 'anyname')`

      `p.age = 23`

      `p.save()`

		4.  `p =Person(name = 'anyname',age = 23)`

     `p.save()`

## 获取对象的方法：

	1.  `Person.objects.all ()`
 	2. `Peron.objects.all()[:10]`   切片操作，获取10个人，**不支持负索引**，切片可节约内存
 	3.  `Person.objects.get(name=anyname)`
 	4. `Person.objects.filter(name='anyname')`
 	5. `Person.objects.filter(name__iexact="abc")`  名称为abc且不区分大小写
 	6.  `Person.objects.filter(name__contains="abc")` 名称包含abc的对象
 	7.  `Person.objects.filter(name__icontains="abc")` 名称包含abc，且abc不区分大小写的对象
 	8.  `Person.objects.filter(name__regex="^abc")`  正则表达式查询
 	9.  `Person.objects.filter(name__iregex="^abc")`  正则表达式不区分大小写
 	10.  `Person.objects.exclude(name__contains="abc")`  排除name中包含abc的对象
 	11.  `Person.objects.filter(name__contains="abc").exclude(age=23)`  找出name含abc的但排除年龄是23的对象

## 删除对象的方法：

 1.  `Person.objects.filter(name__contains="abc").delete()`

 2.  `p=Person.objects.filter(name__contains="abc")`

     `p.delete()`

## 更新对象的方法：

1.  `Person.objects.filter(name__contains="abc").update(name='cba')`

2.  `p = Person.objects.filter(name__contains="abc")`

    `p.name = "cba"`

    `p.save()`

## 查询对象数量：

​	`Person.objects.count()`

## 链式筛选对象：

​	`Person.objects.filter(name__contains="abc").filter(age=23)`

​	`Person.objects.filter(name__contains="abc").exclude(age=23)`检查对象是否存在：

​	`Person.objects.all().exists()`

## 获取对象后排序显示：

​	`Person.objects.all().order_by('name')`	按name进行正排序

​	`Person.objects.all().order_by('-name')`  按name进行负排序

## 切片不支持负索引解决方法：

​	`Person.objects.all().[:10]`	切片操作，前10条

​	`Person.objects.all().[-10:]`	不支持负索引，会报错

1.使用reverse（）解决

​		`Person.objects.all().reverse()[:2]`		最后两条

​		`Person.objects.all().reverse()[0]`		 最后一条

2.使用order_by,在栏目名（column name）前加一个负号

​		`Person.objects.order_by('-age')[:20]`		age最大的20条

## 查询结果合并后重复的解决方法：

一般的情况下，QuerySet 中不会出来重复的，重复是很罕见的，但是当跨越多张表进行检索后，结果并到一起，可能会出来重复的值

```python
qs1 = Pathway.objects.filter(label__name='x')
qs2 = Pathway.objects.filter(reaction__name='A + B >> C')
qs3 = Pathway.objects.filter(inputer__name='WeizhongTu')
# 合并到一起
qs = qs1 | qs2 | qs3
这个时候就有可能出现重复的
# 去重方法
qs = qs.distinct()
```

----------------

# urls.py

urls.py

```python
from django.contrib import admin
from django.conf.urls import url,include

urlpatterns=[
    url(r'^admin/',admin.site.urls),
    url(r'^App/',include('App.urls')),
]
```

App/urls.py

```python
from django.conf.urls import url
from App import views

urlpatterns=[
    url(r'^home/',views.home),
]
```



-----------------



# settings.py设置数据库为MySQL

### 1.apt源安装MySQL   Server

​		`sudo  apt-get update`			更新源

​		`sudo apt-get install mysql-server`		安装数据库

### 2.配置MySQL	Server

​		`sudo	mysql_secure_installation`		安装配置		此过程中要设置root密码

​		`sudo	sysytemctl 	status	 mysql.service`		检查MySQL服务状态	

​		`sudo systemctl enable mysql`				启动MySQL服务

### 3.创建及配置MySQL用户

​		`sudo	mysql	-uroot	-p`			root用户进入，输入刚刚创建的root密码

​		`CREATE DATABASE GP1HelloDjango charset=utf8;`			创建数据库，utf8支持中文存储

​		`GRANT ALL PRIVILEGES ON GP1HelloDjango.* TO muzi@localhost IDENTIFIED BY "mimaSHI123!";`		创建用户muzi(密码mimaSHI123!) 并赋予其以本机localhost登陆GP1HelloDjango数据库的所有权限。

​		`FLUSH PRIVILIGES`		修改生效

之后就可以在pycharm中以"muzi__mimaSHI123!"连接了。

### 4.连接mysql驱动：

常用的mysql驱动有三种：

1.MySQLclient：

​		Python2，3都能用，但是有个致命缺陷，它对mysql安装有要求，必须在指定位置存在配置文件。

2.python-mysql：

​		python2支持很好，python3不支持

3.pymysql：

​		python2,3都支持，而且它还可以伪装成前两个库。

​				`pip install pymysql`  安装pymysql

​				在项目主包 `__init__.py`中输入：

```python
import pymsql
pymysql.install_as_MySQLdb()
```

​				伪装成MySQLclient，再执行迁移：

​				`python manage.py migrate`

### 5.配置数据库连接信息

在setting.py中配置数据库连接信息

```python
'ENGINE':'django.db.backends.mysql',
'NAME':'GP1HelloDjango',
'USER':'muzi',
'PASSWORD':'mimaSHI123!',
'HOST':'localhost',
'PORT':'3306',
```


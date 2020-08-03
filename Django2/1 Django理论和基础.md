# 1 环境搭建

## 1.1 使用virtualenv创建虚拟环境



### 1.1 安装virtualenv

```
pip install virtualenv
```



### 1.2 创建虚拟环境

```
命令：virtualenv 虚拟环境名

例子：virtualenv learn_django

会在当前目录创建一个文件夹，就是创建虚拟环境。
```



### 1.3 激活虚拟环境

打开cmd，进入虚拟环境目录下，或复制路径如下：

![1 django activate](D:\Me\Way\Django2\imags\1 django activate.png)



退出虚拟环境就输入：deactivate，回车。





## 1.2 使用virtualenvwrapper创建虚拟环境

virtualenvwrapper更方，可以在任何地方激活性能环境，前提是你记得名字



### 1.2.1 安装virtualenvwrapper



```
pip install virtualenvwrapper-win
```



### 1.2.2 创建虚拟环境



```
mkvirtualenv learn_django
```



默认会把虚拟环境放到当前用户的Envs文件夹下

修改存放路径的步骤（环境变量这块你还在行的吧🌶）：

- 在系统环境变量创建WORKON_HOME变量
- 变量的值就是你想把虚拟环境放到哪里，把路径粘贴上去就成了

![2 django env path](D:\Me\Way\Django2\imags\2 django env path.png)





### 1.2.3 激活虚拟环境



```
workon learn_django
```

退出也是deactivate。



### 1.2.4 删除虚拟环境



```
rmvirtualenv learn_django
```



### 1.2.5 查看系统所有已创建的虚拟环境

```
lsvirtualenv
```



**更多操作自己查一下某咯。🐕🐱**





------

**下面的操作，都是在虚拟环境下进行的。不要乱来，好吧，开车得打灯😄。**





## 1.3 安装MySQL及一些库

### 1.3.1 安装MySQL

具体步骤，[点这里](D:\Me\Way\MySQL\1 MySQL5.7的安装.md)。



### 1.3.2 安装pymysql

```
pip install pymysql
```



### 1.3.3 安装mysqlclient

```
pip install mysqlclient
```



### 1.3.4 安装django

```
pip install django==2.1
```





#  2 搞Django前的一些前戏



## 2.1 web服务器、应用服务器和web应用框架



- web服务器：处理HTTP请求，响应静态文件。如apache，nginx，IIS。
- 应用服务器：处理逻辑的服务器。如uwsgi，tomcat。
- web应用框架：使用某种语言封装了一些常用的web功能的框架。如flask，django，spring。



## 2.2 django和MVC

django的设计模式遵循MVC，MVC即Model、View和Controller，代表模型、视图和控制器。









# 3 URL和视图



## 3.1 按规矩来，整一个hello world

提醒一下，要在虚拟环境搞。



### 3.1.1 创建一个django项目，并跑起来



- **方式一：命令行的方式**

```
1. 在cmd进入到你要放项目的地方
2. django-admin startproject first_django
3. cd first_django
4. django-admin startapp user
5. python manage.py runserver


命令步骤基本都这样，跑几次就老练了。
```

<img src="D:\Me\Way\Django2\imags\3 django start.png" alt="3 django start" style="zoom: 80%;" />



出现这界面，那恭喜咯，成了~🐖

<img src="D:\Me\Way\Django2\imags\4 django start.png" alt="4 django start" style="zoom: 60%;" />







- 方式二：使用pycharm

pycharm创建很方便，跟着提示就成了，这里就不再说明。



### 3.1.2 django的目录介绍

---- first_django

-------- first_django       :项目名称

------------ settings.py    :配置文件，项目所有的配置都搁这里

------------ urls.py            :配置路由用的

------------ wsgi.py           :部署项目用的，web服务器入口文件

-------- user                     :app名称  

------------ migrations     : 数据库迁移文件都放这里

------------ admin.py        :配置后台管理系统用的

------------ apps.py           :项目加载时需要该文件才能加载app

------------ models.py       :app中的模型类存放用的

------------ test.py              :测试用来用的

------------ views.py           :写视图处理的地方

-------- db.sqlite3              :自带的数据库

-------- manage.py            :项目入口文件，操作项目都得通过它










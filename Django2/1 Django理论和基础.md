

:notebook:

**1. 本文档只是根据django2的学习过程，总结了django2的一些知识点，适合有基础的man:basketball_man:快速复习用。**

**2.  大佬的话应该用不到的:artificial_satellite:...**

**3. 要是哪里有错，请指出:apple:...**



[django2.0官网>>>](https://docs.djangoproject.com/en/2.0/)



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



## 3.1 按规矩来，整第一个项目

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





## 3.2 视图函数



### 3.2.1 视图函数

```
1. 视图函数的第一个参数必须为request
2. 视图函数的返回值必须是HttpResponseBase()或其子类


def index(request, *args, **kwargs):
	
	return HttpResponseBase("hello world")
```



## 3.3 在URL中添加参数

### **方式一：<arg>**

例子:seat:：

```
127.0.0.1:8000/index/1
127.0.0.1:8000/index/2
```



- 视图函数

```
def index(request, id):
	print(id)
	return HttpResponse("ok")
	
def something(request, name, category):
	print(name)
	print(category)
	return HttpResponse("ok")
```

- 路由

```
urlpatterns = [
    url('index/<id>/', index),
    url('something/<name>/<category>/', something),
]
```



注意：

```
1. 你在url上面放了多少个变量，映射到的视图函数就得有多个参数，用来接收，否则要炸。
2. 要参数太多了（额...一般都不会太多）,你就用万能两兄弟something(*args, **kwargs)



```



### 方式二：查询字符串(?key=value&name=ydc...)



**例子:expressionless::**

假设请求路由是：127.0.0.1:8000/index?id=1



- 视图函数

```
def index(request):

	# 这样就可以获取id参数的值了
	id = request.GET.get('id')
	
	return HttpResponse("ok")
```

- 路由

```
urlpatterns = [
    url('index/', index),
]
```



注意：

```
1. 一个推荐使用request.GET.get('id')，而不是request.GET['id']，如果id不存在，后面的方式则会报错，而前面则只返回None。


```





## 3.4 url命名和反转

为什么取名：因为路由是经常变化的，。



- 路由

```
urlpatterns = [
	# 给路由取名：name='login'
    url('index/', index, name='login'),
    url('something/', index, name='something'),
]
```



- 视图函数

```
def something(request):

	id = request.GET.get('id')
	
	if id:
		return HttpResponse("ok")
	else:
		# 跳转到路由：/index/'
		return redirect('/index/')
		
def something_reverse(request):

	id = request.GET.get('id')
	
	if id:
		return HttpResponse("ok")
	else:
		# 反转：login为路由url的名字
		return redirect(reverse('login'))
```



注意：

```
1. 因为一个路由时在多个地方使用的，如果都使用路由本体，以后修改就得改很多地方。
2. 所以一般都给路由取个名字，在项目中使用反转了得到对应的路由。即使路由改了也没事。
3. 

```



## 3.5 应用（app）命名空间

命名空间，即给每一个app创建一个命名，以免多个app之间的路由冲突，



### 3.5.1 创建命名空间方式一

在每个app的urls.py文件写上：

```
app_name = 'your_app_name'
```



### 3.5.2 创建命名空间方式而

在将app的urls.py接入到项目主urls.py文件时取名：

```
urlpatterns = [
	url('admin/', admin.site.urls),
    url('user/', include(('user.urls', 'user'), namespace='user'))
]

说明：
	url('app路由入口', include(('app的urls.py', 'app名称'), namespace='命名空间名称'))

注意参数是谁的，不要穿错了。
```





给app起来命名空间后，在反转时就可以这样写：

​	**redirect(reverse('命名空间:路由名称'))**

```
def something_reverse(request):

	id = request.GET.get('id')
	
	if id:
		return HttpResponse("ok")
	else:
		return redirect(reverse('user:login'))
```



# 4 Django模板

说到模板，就肯定会在模板说明渲染图片，导入.css等文件，根据规范，这些文件我们都是放堆static文件夹下面的。

为了模板能加载到静态文件，现在settings.py文件加入：

```
STATICFILES_DIRS = [
	os.path.join(BASE_DIR,"static")
]
```

注意：

​	这个配置默认是在项目根目录下面找static文件夹的，所有不要把static文件夹放一些奇怪的地方:dog: 。





## 4.1 DTL和Jinja2

1. DTL即Django Template Language，是Django自带的模板语言。

2. 当然也可以配置Django支持Jinja2等其他模板引擎，但是作为Django内置的模板语言，和Django可以达到无缝衔接而不会产生一些不兼容的情况。



## 4.2 DTL和HTML

```
1. DTL模板是一种带有特殊语法的HTML文件，这个HTML文件可以被Django编译，可以传递参数进去，实现数据动态化。
2. 在编译完成后，生成一个普通的HTML文件，然后发送给客户端。
3. DTL比HTML功能强大。


```



## 4.3 模板渲染



### 4.3.1 将模板渲染成字符串：render_to_string()

```
# 先读取html文件再整成字符串返回到页面，浏览器再解析
html = render_to_string("detail.html")
return HttpResponse(html)

```





### 4.3.2 直接渲染：render()

```

return render(request, "detail.html")
```





## 4.4 模板查找路径配置

在项目的settings.py文件：

```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        # 先找根目录下的templates文件夹
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        # 找不到，再去app里面找，再找不到？报错呗...
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```



注意：

```
1. TEMPLATES下的DIRS是一个列表，即可以放多个templates的路径，不管放哪里，把找得到的理解丢进去就成了，但一般不这么干。
2. 
```



## 4.5 模板变量



**模板变量：可以将后端的数据传递到前端页面**

在模板里面，使用变量，对象等用法跟后端的应用。如列表的切片，索引，对象的.属性等。:ok_man:



注意：

```
1. 在模板中不能使用[]，得用点来代替。如，user_list.0, user.name
2. 
```





### 4.5.1 普通变量

- 视图函数

```
def something_reverse(request):

	context = {
        'name': 'ydc',
        'age': 2
	}
	
	return render(request, 'index.html', context=context)
```



- index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
    {{ name }}
    <br>
    {{ age }}
</body>
</html>
```



## 4.5.2 对象

- 视图函数

```
def something_reverse(request):

	context = {
        'obj': [
        	{
        		'name': 'ydc',
        		'age': 5
        	}
        ]
	}
	
	# context是一个键值对对象
	return render(request, 'index.html', context=context)
```



- index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
    {{ obj.0.name }}
    <br>
    {{ obj.0.age }}
</body>
</html>
```



## 4.5 模板标签

常用的模板标签有：

```
1. if...：判断
2. for...in...：遍历
3. for...in...empty：在遍历的对象没有元素，会执行 empty 中的内容。
4. with...：在模版中定义变量。
5. url...：反转url
6. spaceless...：移除html标签中的空白字符。包括空格、tab键、换行等。
7. autoescape...：开启和关闭这个标签内元素的自动转义功能。
8. verbatim...：不想使用 DTL 的解析引擎。那么你可以把这个代码片段放在 verbatim 标签中。
```



### 4.5.1 if 标签

1. if标签相当于Python中的 if 语句，有 elif 和 else 相对应，所有的标签都放在标签符号（ {% %} ）。
2.  if 标签中可以使用 ==、!=、<、<=、>、>=、in、not in、is、is not 等判断运算符。

3. 实例:

```
{% if id%2 == 0 %}
	<p>ok</p>
{% else %}
	<p>no ok</p>
{% endif %}
```



### 4.5.2 for 标签

1. 遍历列表、元组、字符串、字典等一切可以遍历的对象。

```
{% for i in userlist %}
	<p>{{ i.name }}</p>
{% endfor %}
```

2. 反向遍历

```
{% for i in userlist reversed %}
	<p>{{ i.name }}</p>
{% endfor %}
```

3. 遍历字典

```
{% for k,v in my_dict.items %}
    <p>key：{{ k }}</p>
    <p>value：{{ v }}</p>
{% endfor %}
```

4. DTL提供的其他的变量：

```
forloop.counter ：当前循环的下标。以1作为起始值。
forloop.counter0 ：当前循环的下标。以0作为起始值。
forloop.revcounter ：当前循环的反向下标值。比如列表有5个元素，那么第一次遍历这个属性是等于5，并且是以1作为最后一个元素的下标。
forloop.revcounter0 ：类似于forloop.revcounter。不同的是最后一个元素的下标是从0开始。
forloop.first ：是否是第一次遍历。
forloop.last ：是否是最后一次遍历。
forloop.parentloop ：上一级的for循环。
```



### 4.5.3 for empty

1. 这个标签使用跟 for...in... 是一样的，只不过是在遍历的对象如果没有元素的情况下，会执行 empty 中的内容。

```
{% for data in datas %}
	<li>{{ data }}</li>
{% empty %}
	暂无数据
{% endfor %}
```



### 4.5.4 with

1. 当访问一个变量比较复杂时，可以把一个变量缓存到一个变量，方便以后在该模板访问变量。

```
{% with user=userlist.2 %}
	<p>{{ user }}</p>
{% endwith %}
```



注意：

```
1. with在模板定义变量时，=作用两边是不能有空格的
2. 
```



### 4.5.5 url标签

1. 前面也说过，在页面中写死url很难维护，所以需要所以url进行反转

```
<a href="{% url 'user:detail' %}">用户详情</a>

说明：
	{% url '命名空间:路由名称' %}
```



2. 如果 url 反转时需要传递参数，可以在后面传递。但参数有位置参数和关键字参数。位置参数和关键字参数不能同时使用。

```
1. 位置参数
url('detail/<urse_id>/',views.user_detail,name='user_detail')
<a href="{% url 'user:detail' 1 %}">用户详情</a>

2. 关键字参数
<a href="{% url 'user:detail' user_id=1 %}">用户详情</a>
<a href="{% url 'user:list' page=1 size=10 %}">用户列表</a>

3. 查询参数
<a href="{% url 'user:list' page=1 %}?size=10">用户列表</a>
```



### 4.5.6 spaceless标签

1. 移除html标签中的空白字符。包括空格、tab键、换行等。

```
{% spaceless %}
    <p>
    	<a href="home/">home</a>
    </p>
{% endspaceless %}

结果：
	<p><a href="home/">home</a></p>
```



2. spaceless 只会移除html标签之间的空白字符。而不会移除标签与文本之间的空白字符。

```
{% spaceless %}
    <p>
    	<a href="home/">
    		<span>
    			home
    		</span>
    	</a>
    </p>
{% endspaceless %}

结果：
	<p><a href="home/"><span>
			home
	</span></a></p>
```



### 4.5.7 autoescape标签

1. 开启和关闭这个标签内元素的自动转义功能。
2. 模板中默认是已经开启了自动转义的。

```
# 传递的上下文信息
context = {
	"info":"<a href='www.baidu.com'>百度</a>"
}

# 模板中关闭自动转义
{% autoescape on %}
	{{ info }}
{% endautoescape %}

结果：
	百度(可以点击的)
```



### 4.5.8 verbatim标签

1. 默认在 DTL 模板中是会去解析那些特殊字符的。
2. 不想使用 DTL 的解析引擎，可以把这个代码片段放在 verbatim 标签中。

```
{% verbatim %}
	{{ if dying }}
		哈哈哈
	{{ /if }}
{% endverbatim %}
```



## 4.6 过滤器

过滤器用法跟函数差不多，就是传参有点奇怪:palm_tree: 。



### 4.6.1 add

将传进来的参数添加到原来的值上面。

```
{{ v|add:5 }}

说明：
	1. 将5累加到v
	2. {{ v|add:5 }}  ===> add(v, 5)
```





### 4.6.2 cut

移除值中所有指定的字符串。

```
{{ v|cut:" " }}

说明：
	 {{ v|cut:" " }} ===> replace(' ','')
```



### 4.6.3 date

将一个日期按照指定的格式，格式化成字符串。

```
context = {
"bir": datetime.now()
}

{{ bir|date:"Y/m/d" }}

说明：
	{{ bir|date:"Y/m/d" }} ===> 2020/08/05
```

| 字符 | 描述                                 | 样例  |
| ---- | ------------------------------------ | ----- |
| Y    | 四位数年份                           | 2020  |
| m    | 两位数月份                           | 01-12 |
| n    | 月份，1-9前面没有0前缀               | 1-12  |
| d    | 两位数字的天                         | 01-31 |
| j    | 天，但是1-9前面没有0前缀             | 1-31  |
| g    | 小时，12小时格式的，1-9前面没有0前缀 | 1-12  |
| h    | 小时，12小时格式的，1-9前面有0前缀   | 01-12 |
| G    | 小时，24小时格式的，1-9前面没有0前缀 | 1-23  |
| H    | 小时，24小时格式的，1-9前面有0前缀   | 01-23 |
| i    | 分钟，1-9前面有0前缀                 | 00-59 |
| s    | 秒，1-9前面有0前缀                   | 00-59 |



### 4.6.4 defualt

如果值为 False 。比如 [] ， "" ， None ， {} 等，都会使用 default 过滤器提供的默认值。

```
{{ v|default:"nothing" }}

说明：
	1. 如果v为False，则渲染nothing
```





### 4.6.5 default_if_none

如果值是 None ，那么将会使用 default_if_none 提供的默认值。

```
{{ v|default_if_none:"nothing" }}

说明：
	当v的值为Node时，输出nothing。
```



### 4.6.6 first

返回列表/元组/字符串中的第一个元素。

```
{{ v|first }}
```



### 4.6.7 last

返回列表/元组/字符串中的最后一个元素。

```
{{ v|last }}
```



### 4.6.8 floatformat

使用四舍五入的方式格式化一个浮点类型。

```
{{ v|floatformat:3 }}
```





### 4.6.9 join

将列表/元组/字符串用指定的字符进行拼接。

```
{{ v|join:"/" }}

说明：
	v=['1', '2', '3'],则结果为：1/2/3
```



### 4.6.10 length

获取一个列表/元组/字符串/字典的长度。

```
{{ v|length }}
```



### 4.6.11 lower

将值中所有的字符全部转换成小写。

```
{{ v|lower }}
```



### 4.6.12 upper

将指定的字符串全部转换成大写。

```
{{ v|upper }}
```



### 4.6.13 random

在列表/字符串/元组中随机的选择一个值。

```
{{ v|rando }}
```



### 4.6.14 safe

标记一个字符串是安全的。也会关掉这个字符串的自动转义。

```
{{ string|safe }}

说明：
	如果 string 是一个普通的字符串，如abc，则直接在页面渲染。如果string 是 html 代码，也会直接当字符串渲染，不进行转换。
```



### 4.6.15 slice

切片操作。

```
{{ user_list|slice:":5" }}
```



### 4.6.16 stringtags

删除字符串中所有的 html 标签。

```
{{ string|slice:":5" }}

说明：
	<p>hello</p> ===> hello
```



#### 4.6.17 truncatechars

字符串长度大于过滤器指定长度，则进行切割，并拼接三个点来作为省略号。

```
{{ string|truncatechars:5 }}

注意：
	显示结果三个点的长度也包含。
	123456 ===> 12...
```



### 4.6.18 truncatechars_html

切割 html 标签。

```
{{ string|truncatechars_html:5 }}

注意：
	显示结果三个点的长度也包含。
	<p>123456</p> ===> <p>12...</p>
```







## 4.7 模板继承



### 4.7.1 引入模板



**引入模板使用：{% include '模板名称' %}**



在index.html引入header.html文件

- index.html文件

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Index</title>
</head>
<body>
	{% include 'header.html' %}
	
	<div class="main"></div>
	
	<footer></footer>
</body>
</html>
```



- header.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
	header>我是头部</header>
</body>
</html>
```



注意：

```
1. 注意文件的路径，写不对就找不到。

```



(今天才8月5号，但是已经下了七天的雨了:cloud_with_lightning_and_rain: ...)



### 4.7.2 模板继承



**模板的引入和继承，目的都是为了代码的重用，避免写一些重复的代码。**

所以，在web设计时，划分好模块，把公共的抽出来，在用到的地方引入，或继承就成了🚗。

模板继承，大概就是在一个模板挖洞，在另一个模板填坑。



**场景：所有网页都包含一个头部导航。**



- 父模版（母模板）：base.html

```
{% load static %} <!- 这里是加载静态文件 ->

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    
    <!- 留给子模板写标题 ->
    <title>{% block title %}base{% endblock %}</title>
    <!- 留给子模板引入文件 ->
    {% block head %}{% endblock %}
    
    <!- 留给子模板写css样式 ->
    {% block css %}{% endblock %}
</head>
<body>
    <header>
        <ul>
            <li>首页</li>
            <li>帮助</li>
            <li>合作</li>
        </ul>
    </header>
	<!- 留给子模板写内容 ->
    {% block content %}{% endblock %}
	
	<!- 留给子模板写js ->
    {% block js %}{% endblock %}
</body>
</html>
```



- 子模板：index.html

```
{% extends 'base.html' %}  <!- 继承模板，必须写在最前面 ->

{% load static %}  <!- 加载静态文件 ->

<!- 子模板在这里写标题 ->
{% block title %}index{% endblock %}

<!- 子模板在这里引入文件 ->
{% block head %}{% endblock %}

<!- 子模板在这里写css样式 ->
{% block css %}
	<style>
        *{
            padding: 0px;
            margin: 0px;
        }
    </style>
{% endblock %}

<!- 子模板在这里写html内容 ->
{% block contetn %}
	<div class="main">
        <h1>我是儿子</h1>
    </div>
{% endblock %}

<!- 子模板在这里写js内容 ->
{% block js %}
	<script>
        function hello() {
            alert('hello **')
        }
    </script>
{% endblock %}
```



注意：

	1. {% extends 'base.html' %}必须在子模板的最前面（你的第一行可能留空，所有没敢直接说第一行:goat: ）。
 	2. 



### 4.7.3 补充：处理{% load static %}



如果每次都在模板上面写{% load static %}，那就有点low了，为此，可以在settings.py文件配置。

将{% load static %}，设置成内置的标签：

```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
            # 在这里加载，则
            'builtins': ['django.templatetags.static']
        },
    },
]
```

则后面可以直接使用static了：

```
<link rel="stylesheet" href="{% static 'plugin/bootstrap-3.3.7/css/bootstrap.min.css' %}">
```

而不需要在页面前面再load一下：

```
{% load static %}
```





# 5 Django数据库



## 5.1 数据库配置



在这里提一下MySQL和SQLserver的配置，其他的可以直接查一下咯:partly_sunny:。



### 5.1.1 mysql

- 安装第三方库

```
pip install pymysql
pip install mysqlclient
```



- settings.py

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': '数据库名称',
        'HOST': '127.0.0.1',
        'PORT': 3306,
        'USER': '数据库用户名',
        'PASSWORD': '密码',
    }
}
```



### 5.1.2 sql server

- 安装第三方库

```
pip install django-sqlserver django-pytds pyodbc django-pyodbc pypiwin32
pip install django-pyodbc-azure
```



- settings.py

```
DATABASES = {
    'default': {
        'ENGINE': 'sql_server.pyodbc',
        'NAME': '数据库名',
        'HOST': '127.0.0.1',
        'PORT': '1433',
        'USER': '用户名',
        'PASSWORD': '密码',
        'OPTIONS': {
            'DRIVER': 'SQL Server Native Client 11.0',
            'MARS_Connection': True,  # 支持异步
        },
    }
}
```





## 5.2 ORM：对象关系映射



ORM ，全称 Object Relational Mapping ，即对象关系映射，因为ORM的存在，我们可以使用类的方式去操作数据库，而不是使用原生的SQL语句:sushi: 。



### 5.2.1 数据库迁移

数据库迁移的步骤：

1. **你的应用注册了**

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    
    'your_app',
]
```



2. **数据库连接整好了**

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': '数据库名称',
        'HOST': '127.0.0.1',
        'PORT': 3306,
        'USER': '数据库用户名',
        'PASSWORD': '密码',
    }
}


注意：
	1. 数据库得先创建好，如果数据库没创建，运行项目就报错。
	2. 
```



3. **执行命令**

```
# 生成迁移文件
python manage.py makemigrations

# 迁移到数据库
python manage.py migrate
```





### 5.2.2 ORM的优点

- 易用性高
- 性能消耗小
- 设计灵活
- 可移植



### 5.2.3 常用的字段及属性



首先，模型对象长这样的，当然，真正开发肯定得复杂很多:crying_cat_face:：

```
class Personnel(models.Model):

    gender_choices = ((0, _(u'man')), (1, _('woman')))
    
    id = models.AutoField(primary_key=True)
    name = models.CharField(max_length=24, blank=True, null=True)
    gender = models.SmallIntegerField(choices=gender_choices, blank=True, null=True)
    age = models.IntegerField()
    
```



**常用的字段有:smiling_imp:：**



| 自增字段                           | 说明                                              |      |
| ---------------------------------- | ------------------------------------------------- | ---- |
| models.BigAutoField()              |                                                   |      |
| models.AutoField()                 | 这两个都是自增的，默认从1开始                     |      |
| **二进制字段**                     |                                                   |      |
| models.BinaryField()               | 可以把图片内容读，再存到数据库...应该挺少人这么干 |      |
| **布尔型**                         |                                                   |      |
| models.BooleanField()              |                                                   |      |
| models.NullBooleanField()          |                                                   |      |
| **整型**                           |                                                   |      |
| models.PositiveSmallIntegerField() | 5个字节                                           |      |
| models.SmallIntegerField()         | 6个字节                                           |      |
| models.PositiveIntegerField()      | 10个字节                                          |      |
| models.IntegerField()              | 11个字节                                          |      |
| models.BigIntegerField()           | 20个字节                                          |      |
| **字符串类型**                     |                                                   |      |
| models.CharField()                 | varchar，可变长字符串                             |      |
| model.TextField()                  | longtext，文本类型                                |      |
| **时间日期类型**                   |                                                   |      |
| models.DateField()                 | 日期类型                                          |      |
| models.DateTimeField()             | 时间日期类型                                      |      |
| models.DurationField()             | int, Python timedelta实现                         |      |
| **浮点型**                         |                                                   |      |
| models.FloatField()                |                                                   |      |
| models.DecimalField()              |                                                   |      |
| **其他字段**                       |                                                   |      |
| models.EmailField()                | 邮箱字段                                          |      |
| models.ImageField()                | 图片字段                                          |      |
| models.FileField()                 | 文件字段                                          |      |
| models.FilePathField()             | 文件路径字段                                      |      |
| models.URLField()                  | url字段                                           |      |
| models.UUIDField()                 | 唯一字段                                          |      |



**字段的参数**

| 所有字段都有的参数            | 说明                                                         |      |
| ----------------------------- | ------------------------------------------------------------ | ---- |
| db_column="xxx"               | 字段在数据库中的名字，不给就默认是字段名                     |      |
| primary_key=True              | 设置字段为主键，默认为False                                  |      |
| verbose_name="xxx"            | 字段的在django的后台系统显示的备注                           |      |
| unique=True                   | 该字段的数据在表中唯一                                       |      |
| null=True                     | 在数据库中允许为空                                           |      |
| blank=True                    | 在django的后台系统的form表达中可以为空                       |      |
| db_index=True                 | 设置索引                                                     |      |
| help_text="xxx"               | 字段的帮助信息                                               |      |
| editable=False                | 不能对字段内容进行编辑，默认是True，即可以编辑               |      |
| **字段独有的参数**            |                                                              |      |
| max_length=100                | models.CharField()字段独有，即指定字段长度为100个utf-8格式的字符串 |      |
| auto_now=True                 | models.DateField()和models.DateTimeField()独有，字段的值为该条记录**编辑**的那个时间日期，编辑一次，就更新一次 |      |
| auto_now_add=True             | models.DateField()和models.DateTimeField()独有，字段的值为该条记录**创建时候**的那个时间日期 |      |
| unique_for_month=True         | models.DateField()和models.DateTimeField()独有，             |      |
| max_digits=4,decimal_places=2 | models.DecimalField()独有，共有4位，小数点后2位              |      |
| **关系型字段的参数**          |                                                              |      |
| related_name="xxx"            | models.ForeignKey()独有，xxx用于外键关联中的反向查询，通过父表查询子表 |      |
| on_delete=models.CASCADE      | models.ForeignKey()独有，有6种操作：<br />on_delete = models.CASCADE            删除关联数据的时候，与之的关联也删除 <br />on_delete = models.DO_NOTHING   删除关联数据的时候，什么操作也不做 <br />on_delete = models.PROTRCT           删除关联数据的时候，引发报错 <br />on_delete = models.SET_NULL          删除关联数据的时候，与之关联的只设置为空 <br />on_delete = models.SET_DEFAULT    删除关联数据的时候，与之关联的只设置为默认值 <br />on_delete = models.SET                      删除关联数据 |      |
| to="User"                     | models.ForeignKey()独有，关联的对象                          |      |



### 5.2.3 外键



在 MySQL 中，表有两种引擎，InnoDB和myisam 。InnoDB 引擎是支持外键约束的。外键的存在使得 ORM 框架在处理表关系的时候异常的强大。



类定义

```
class ForeignKey(to,on_delete,**options) 

说明：	
	to：是要关联的模型
	on_delete：在使用外键引用的模型数据被删除了，这个字段该如何处理，参考5.2.3 常用的字段及属性的说明。
	这两个参数是必须的。
	
	author = models.ForeignKey("User",on_delete=models.CASCADE)
	或
	author = models.ForeignKey(User,on_delete=models.CASCADE)
```



在数据库底层都是存外键的，表面看起来是存对象🐏。





### 5.2.4 表关系



有时候真的分不清表之间的关系，怎么想怎么有道理。

但是，表关系很重要，也很好用。



表自己的关系可以自己维护，自己维护的好处就是便利，知道发生了什么

也可以给django维护，django维护的好处就是便利



#### 一对一



场景

```
一个用户有一个用户详细信息
```



**实现：django维护关系**

```python
class User(models.Model):
    username = models.CharField(max_length=20)
    password = models.CharField(max_length=100)

class UserInfo(models.Model):
    birthday = models.DateTimeField(blank=True,null=True)
    school = models.CharField(blank=True,max_length=50)
    # 映射到数据库是，就成了user_id
    user = models.OneToOneField("User", on_delete=models.CASCADE)
```



**实现：自己维护**

```python
class User(models.Model):
    username = models.CharField(max_length=20)
    password = models.CharField(max_length=100)

    
class UserInfo(models.Model):
    birthday = models.DateTimeField(blank=True,null=True)
    school = models.CharField(blank=True,max_length=50)
    # 自己定义
    user_id = models.IntegerField(blank=True,null=True)
```



#### 一对多



场景

```
一个作者有多篇文章
```



**实现**

```python
class Author(models.Model):
    username = models.CharField(max_length=20)
    password = models.CharField(max_length=100)


class Article(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    author = models.ForeignKey("User",on_delete=models.CASCADE)
```










































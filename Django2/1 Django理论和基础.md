

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



| 自增字段                           | 说明                                                         |      |
| ---------------------------------- | ------------------------------------------------------------ | ---- |
| models.BigAutoField()              |                                                              |      |
| models.AutoField()                 | 这两个都是自增的，默认从1开始                                |      |
| **二进制字段**                     |                                                              |      |
| models.BinaryField()               | 可以把图片内容读，再存到数据库...应该挺少人这么干            |      |
| **布尔型**                         |                                                              |      |
| models.BooleanField()              |                                                              |      |
| models.NullBooleanField()          |                                                              |      |
| **整型**                           |                                                              |      |
| models.PositiveSmallIntegerField() | 正小整型，5个字节，0~32767                                   |      |
| models.SmallIntegerField()         | 小整型，6个字节，-32768~32767                                |      |
| models.PositiveIntegerField()      | 正整型，10个字节，0~2147483647                               |      |
| models.IntegerField()              | 整型，11个字节-2147483648~2147483647                         |      |
| models.BigIntegerField()           | 大整型，20个字节，-9223372036854775808~9223372036854775807   |      |
| **字符串类型**                     |                                                              |      |
| models.CharField()                 | varchar，可变长字符串，超过254个字符则不建议使用，用下面那个 |      |
| model.TextField()                  | longtext，文本类型                                           |      |
| **时间日期类型**                   |                                                              |      |
| models.DateField()                 | 日期类型                                                     |      |
| models.DateTimeField()             | 时间日期类型                                                 |      |
| models.DurationField()             | int, Python timedelta实现                                    |      |
| **浮点型**                         |                                                              |      |
| models.FloatField()                |                                                              |      |
| models.DecimalField()              |                                                              |      |
| **其他字段**                       |                                                              |      |
| models.EmailField()                | 邮箱字段， 不指定max_length时，默认254                       |      |
| models.ImageField()                | 图片字段                                                     |      |
| models.FileField()                 | 文件字段                                                     |      |
| models.FilePathField()             | 文件路径字段                                                 |      |
| models.URLField()                  | url字段                                                      |      |
| models.UUIDField()                 | 唯一字段                                                     |      |



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
	
	

当跨app关联模型时
	1.  在模型前面加上app的名字
		author = models.ForeignKey("user.User",on_delete=models.CASCADE)
	
	2. 先从其他app引入模型，再关联（额...我都这么干）
```



在数据库底层都是存外键的，表面看起来是存对象🐏。





### 5.2.4 表关系



有时候真的分不清表之间的关系，怎么想怎么有道理。

但是，表关系很重要，也很好用。



表自己的关系可以自己维护，自己维护的好处就是便利，知道发生了什么

也可以给django维护，django维护的好处就是便利



#### 一对一



**场景**

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



**场景**

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



获取第一个作者的所有文章

```python
author = Author.objects.first()
articles = author.article_set.all()
```



#### 多对多



**应用场景**

```
文章和标签的关系
一篇文章有多个标签，一个标签可以分配给多个文章
```



**实现**

```python
class Article(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    
    # ManyToManyFields：多对多的字段
    tags = models.ManyToManyField("Tag",related_name="articles")
    
class Tag(models.Model):
    name = models.CharField(max_length=50)
    

说明：
	1. 使用ManyToManyField后，会自己曾经一个中间表来维护关系
    2. 
```



### related_name和related_query_name



获取作者所有的文字：

 ```
author.article_set.all()

说明：
	1. article_set：即模型对象的小写，加上_set来访问
	2. 
 ```



而这名字可以修改

```python
class Article(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    # 传递related_name参数，以后在方向引用的时候使用articles进行访问
    author = models.ForeignKey("User",on_delete=models.SET_NULL,null=True,related_name='articles')
```

访问的时候则

```
author = Author.objects.first()
author.articles.all()
```



### 5.2.5 模型的操作



#### 增加

```
book = Book(name='三国演义',desc='三国英雄！')
book.save()
或直接
Book(name='三国演义',desc='三国英雄！').save()
```

#### 数据查询



1. 查询所有数据

```
books = Book.objects.all()
```

2. 过滤

```
books = Book.objects.filter(name='ydc', ...)
```

3. 获取单个对象

```
books = Book.objects.get(name='ydc')
```

4. 排序

```
books = Book.objects.order_by('date')

说明：
	1. 排序默认是升序的
	2. 降序可以使用：books = Book.objects.order_by('-date')
```



#### 修改数据

```
books = Book.objects.get(name='ydc')
books.name = 'maker'
...
books.save()
或者
Book.objects.get(name='ydc').update(name='maker')
```



#### 删除数据

```
books = Book.objects.get(name='ydc')
books.delete()
或者
Book.objects.get(name='ydc').delete()

可以一次删除多条记录
Book.objects.filter(name='ydc').delete()
Book.objects.all().delete()
```





#### 查询操作

##### 条件查询

**exact**

```
article = Article.objects.get(id__exact=14)
article = Article.objects.get(id__exact=None)


说明：
	1. 精确查询，即“=”，name='ydc'，没有特殊字符匹配
	2. None会解释为NULL
	3. 相当于
		select ... from article where id=14;
		select ... from article where id IS NULL;
```

​	  **iexact**

```python
article = Article.objects.filter(title__iexact='hello world')

说明：
	1. 即like查询
	2. 相当于：select ... from article where title like 'hello world';
```

​	 **contains**

```python
articles = Article.objects.filter(title__contains='hello')

说明：
	1. 大小写敏感，判断某个字段是否包含了某个数据。
	2. 相当于：select ... where title like binary '%hello%';
	3. 
```

**icontains**

```python
articles = Article.objects.filter(title__icontains='hello')

说明：
	1. 大小写 不 敏感的匹配查询。
	2. 相当于：select ... where title like '%hello%';
```

**in**

```python
articles = Article.objects.filter(id__in=[1,2,3])

说明：
	1. 提取那些给定的 field 的值是否在给定的容器中。容器可以为 list 、 tuple 或者任何一个可以迭代的对象，包括 QuerySet 对象。
	2. 相当于：select ... where id in (1,3,4)
```

```python
inner_qs = Article.objects.filter(title__contains='hello')
categories = Category.objects.filter(article__in=inner_qs)

相当于：select ...from category where article.id in (select id from article where title like '%hello%');
```

**gt**

```python
articles = Article.objects.filter(id__gt=4)

说明：
	1. 某个 field 的值要大于给定的值。
	2. 相当于：select ... where id > 4;
```

除了gt，还有：

gte：大于等于

lt：小于

lte：小于等于



**startswith**

```python
articles = Article.objects.filter(title__startswith='hello')

说明：
	1. 判断某个字段的值是否是以某个值开始的，大小写敏感。
	2. 相当于：select ... where title like 'hello%'
```

**istartswith**

类似于startswith，但大小写是不敏感的。

**endswith**

```python
articles = Article.objects.filter(title__endswith='world')

说明：
	 1. 判断某个字段的值是否以某个值结束。大小写敏感。
	 2. 相当于：select ... where title like '%world';
```

**iendswith**

类似于endswith，但大小写是不敏感的。

**range**

```python
from django.utils.timezone import make_aware
from datetime import datetime
start_date = make_aware(datetime(year=2018,month=1,day=1))
end_date = make_aware(datetime(year=2018,month=3,day=29,hour=16))

articles = Article.objects.filter(pub_date__range=(start_date,end_date))

说明：
	1. 判断某个 field 的值是否在给定的区间中。
    2. 相当于：select ... from article where pub_time between '2020-01-01' and '2020-12-12'。
```

**date**

```python
articles = Article.objects.filter(pub_date__date=date(2020,8,29))


说明：
	1. 针对某些 date 或者 datetime 类型的字段。可以指定 date 的范围。并且这个时间过滤，还可以使用链式调用。
	2. 相当于：select ... WHERE DATE(CONVERT_TZ(`front_article`.`pub_date`, 'UTC', 'Asia/Shanghai')) = 2020-08-29
```

**year**

```python
articles = Article.objects.filter(pub_date__year=2020)
articles = Article.objects.filter(pub_date__year__gte=2020)

说明：
	1. 根据年份进行查找。
	2. select ... where pub_date between '2020-01-01' and '2020-12-31';
		select ... where pub_date >= '2020-01-01';
	3. 
```

**month：**同 year ，根据月份进行查找。

**day：**同 year ，根据日期进行查找。

**week_day：**同 year ，根据星期几进行查找。1表示星期天，7表示星期六， 2-6 代表的是星期一到星期五。

**time**

```python
articles = Article.objects.filter(pub_date__time=datetime.time(12,12,12));

说明：
	1. 根据时间进行查找。
	2. 
```

**isnull**

```python
articles = Article.objects.filter(pub_date__isnull=False)

说明：
	1. 根据值是否为空进行查找。
	2. 相当于：select ... where pub_date is not null;
```

**regex和iregex**

```python
articles = Article.objects.filter(title__regex=r'^hello')

说明：
	1. 大小写敏感和大小写不敏感的正则表达式。
	2. 相当于：select ... where title regexp binary '^hello';
	3. iregex 是大小写不敏感的。
```



##### 关联的表进行查询

```python
class Category(models.Model):
    """文章分类表"""
    name = models.CharField(max_length=100)
    
class Article(models.Model):
    """文章表"""
    title = models.CharField(max_length=100,null=True)
    category = models.ForeignKey("Category",on_delete=models.CASCADE)
```



获取文章标题中包含"hello"的所有的分类

```python
categories = Category.object.filter(article__title__contains("hello"))
```



#### 聚会函数



##### Avg

```python
from django.db.models import Avg
result = Book.objects.aggregate(Avg('price'))

说明：
	1. 求平均值。
```

##### Count

```python
from django.db.models import Count
result = Book.objects.aggregate(book_num=Count('id'))
result = Author.objects.aggregate(count=Count('email', distinct=True))

说明
	1. 获取指定的对象的个数。
	2. Count 类中，还有另外一个参数叫做 distinct ，默认是等于 False ，如果是等于 True ，那么将去掉那些重复的值。
```

##### Max和Min

```python
from django.db.models import Max, Min
result = Author.objects.aggregate(Max('age'), Min('age'))

说明
	1. 最大的年龄是88,最小的年龄是18。
	2. 
```

##### Sum

```python
from djang.db.models import Sum
result = Book.objects.annotate(total=Sum("bookstore__price")).values("name","total")

说明：
	1. 求指定对象的总和。
```



### aggregate和annotate的区别

1. aggregate ：返回使用聚合函数后的字段和值。
2. annotate ：在原来模型字段的基础之上添加一个使用了聚合函数的字段，并且在使用聚合函
数的时候，会使用当前这个模型的主键进行分组（group by）。



### F表达式和Q表达式



##### F

```python
employees = Employee.objects.all()
for employee in employees:
    employee.salary += 1000
    employee.save()

说明过程可以优化成下面：

from djang.db.models import F
Employee.object.update(salary=F("salary") + 1000)

说明：
	1. F表达式并不会马上从数据库中获取数据，而是在生成SQL语句的时候，动态的获取传给F表达式的值。
```

##### Q

```python
books = Book.objects.filter(price__gte=100,rating__gte=9)

说明过程可以写成：

from django.db.models import Q
books = Book.objects.filter(Q(price__lte=10) | Q(rating__lte=9))

说明：
	1. |或运算
	2. &与运算
	3. ~非运算


from django.db.models import Q
# 获取id等于3的图书
books = Book.objects.filter(Q(id=3))
# 获取id等于3，或者名字中包含文字"记"的图书
books = Book.objects.filter(Q(id=3)|Q(name__contains("记")))
# 获取价格大于100，并且书名中包含"记"的图书
books = Book.objects.filter(Q(price__gte=100)&Q(name__contains("记")))
# 获取书名包含“记”，但是id不等于3的图书
books = Book.objects.filter(Q(name__contains='记') & ~Q(id=3))
```



### 5.2.6 QuerySet API

![5 query set](D:\Me\Way\Django2\imags\5 query set.png)



1. exclude

**排除**满足条件的数据，返回一个新的 QuerySet 。

```
Article.objects.exclude(title__contains='hello')
```

2. annotate

给 QuerySet 中的每个对象都添加一个使用查询表达式（聚合函数、F表达式、Q表达式、Func表达式等）的新字段。

```
articles = Article.objects.annotate(author_name=F("author__name"))
```

3. order_by

指定将查询的结果根据某个字段进行排序。如果要倒序排序，那么可以在这个字段的前面加一个负号。

```python
# 根据创建的时间正序排序
articles = Article.objects.order_by("create_time")
# 根据创建的时间倒序排序
articles = Article.objects.order_by("-create_time")
# 根据作者的名字进行排序
articles = Article.objects.order_by("author__name")
# 首先根据创建的时间进行排序，如果时间相同，则根据作者的名字进行排序
articles = Article.objects.order_by("create_time",'author__name')


说明：
	1. 当使用多个order_by时，后面的会覆盖前面的
    2. 多个 order_by ，会把前面排序的规则给打乱，而使用后面的排序方式。
```

4. values

```python
articles = Article.objects.values("title",'content')
for article in articles:
	print(article)

说明：
	1. 用来指定在提取数据出来，需要提取哪些字段。返回字典形式的数据
	2. {'title':　'title', 'content': 'content'}
```

5. values_list

```python
和values类似，返回的是元组的形式，(('title', 'content'))
```



# 6 高级视图



## 6.1 限制请求method



常用的method

1. GET：从服务器获取数据。
2. POST：向服务器提交时间。
3. DELETE：删除服务器数据。
4. PUT：更新服务器数据。
5. ...



## 6.2 内置的限制请求装饰器



### 6.2.1 require_http_methods



这个装饰器需要传递一个允许访问的方法的列表。比如只能通过 GET 的方式访问。

```python
from django.views.decorators.http import require_http_methods

@require_http_methods(["GET"])
def my_view(request):
    pass
```



### 6.2.2 require_GET



这个装饰器相当于是 require_http_methods(['GET']) 的简写形式，只允许使用 GET 的 method 来访问视图。

```python
from django.views.decorators.http import require_GET

@require_GET
def my_view(request):
	pass
```



### 6.2.3 require_POST



这个装饰器相当于是 require_http_methods(['POST']) 的简写形式，只允许使用 POST 的 method 来访问视图。

```python
from django.views.decorators.http import require_POST

@require_POST
def my_view(request):
	pass
```



### 6.2.4 require_safe



这个装饰器相当于是 require_http_methods(['GET','HEAD']) 的简写形式，只允许使用相对安全的方式来访问视图。因为 GET 和 HEAD 不会对服务器产生增删改的行为。因此是一种相对安全的请求方式。

```python
from django.views.decorators.http import require_safe

@require_safe
def my_view(request):
	pass
```



## 6.3 页面重定向



重定向分为永久性重定向和暂时性重定向。

在页面上体现的操作就是浏览器会从一个页面自动跳转到另外一个页面。

- 永久性重定向

```
http的状态码是301，多用于旧网址被废弃了要转到一个新的网址确保用户的访问.
最经典的就是京东网站，你输入www.jingdong.com的时候，会被重定向到www.jd.com.
因为jingdong.com这个网址已经被废弃了，被改成jd.com，所以这种情况下应该用永久重定向。
```



- 暂时性重定向

```
http的状态码是302，表示页面的暂时性跳转。
比如访问一个需要权限的网址，如果当前用户没有登录，应该重定向到登录页面，这种情况下，应该用暂时性重定向。
```



### redirect



重定向是使用 redirect(to, *args, permanent=False, **kwargs) 来实现的。 

to 是一个 url ， permanent 代表的是这个重定向是否是一个永久的重定向，默认是 False 。

```python
from django.shortcuts import reverse,redirect

def profile(request):
    if request.GET.get("username"):
        return HttpResponse("%s，欢迎来到个人中心页面！")
    else:
        return redirect(reverse("user:login"))

说明：
	1. redirect(reverse("user:login")) ==》反向解析：redirect(reverse("app_name:url_name"))
```



## 6.4 HttpRequest对象



### 6.4.1 WSGIRequest对象常用属性



WSGIRequest 对象上大部分的属性都是只读的。因为这些属性是从客户端上传上来的，没必要做任何的修改。



1. **path** ：请求服务器的完整“路径”，但不包含域名和参数。

```
 http://www.baidu.com/xxx/yyy/ ，那么 path 就是 /xxx/yyy/ 。
```

2. **method** ：代表当前请求的 http 方法。比如是 GET 还是 POST 。
3. **GET** ：django.http.request.QueryDict 对象。操作起来类似于字典。

```
这个属性中包含了所有以 ?xxx=xxx 的方式上传上来的参数。
```

4. **POST** ：django.http.request.QueryDict 对象。这个属性中包含了所有以 POST 方式上传的参数。

5. **FILES** ：django.http.request.QueryDict 对象。这个属性中包含了所有上传的文件。
6. **COOKIES** ：标准的Python字典，包含所有的 cookie ，键值对都是字符串类型。
7. **session** ：类似于字典的对象。用来操作服务器的 session 。
8. **META** ：存储的客户端发送上来的所有 header 信息。
9. **CONTENT_LENGTH** ：请求的正文的长度（是一个字符串）。
10. **CONTENT_TYPE** ：请求的正文的MIME类型。
11. **HTTP_ACCEPT** ：响应可接收的Content-Type。
12. **HTTP_ACCEPT_ENCODING** ：响应可接收的编码。
13. **HTTP_ACCEPT_LANGUAGE** ： 响应可接收的语言。
14. **HTTP_HOST** ：客户端发送的HOST值。
15. **HTTP_REFERER** ：在访问这个页面上一个页面的url。
16. **QUERY_STRING** ：单个字符串形式的查询字符串（未解析过的形式）。
17. **REMOTE_ADDR** ：客户端的IP地址。

```python
如果服务器使用了 nginx 做反向代理或者负载均衡，那么这个值返回的是 127.0.0.1 
这时候可以使用 HTTP_X_FORWARDED_FOR 来获取
所以获取 ip 地址的代码片段如下：

if request.META.has_key('HTTP_X_FORWARDED_FOR'):
	ip = request.META['HTTP_X_FORWARDED_FOR']
else:
	ip = request.META['REMOTE_ADDR']
```

18. REMOTE_HOST ：客户端的主机名。
19. REQUEST_METHOD ：请求方法。一个字符串类似于 GET 或者 POST 。
20. SERVER_NAME ：服务器域名。
21. SERVER_PORT ：服务器端口号，是一个字符串类型。



### 6.4.2 WSGIRequest对象常用属性



1. **is_secure()** ：是否是采用 https 协议。
2. **is_ajax()** ：是否采用 ajax 发送的请求。原理就是判断请求头中是否存在 X-Requested-With:XMLHttpRequest 。
3. **get_host()** ：服务器的域名。如果在访问的时候还有端口号，那么会加上端口号。比如 www.baidu.com:9000 。
4. **get_full_path()** ：返回完整的path。如果有查询字符串，还会加上查询字符串。比如 /music/bands/?print=True 。
5. **get_raw_uri()** ：获取请求的完整 url 。



### 6.4.3 QueryDict对象



request.GET 和 request.POST 都是 QueryDict 对象，这个对象继承自 dict ，因此用法跟 dict 相差无几。

其中用得比较多的是 get 方法和 getlist 方法。

1. **get** 方法：用来获取指定 key 的值，如果没有这个 key ，那么会返回 None 。
2. **getlist** 方法：如果浏览器上传上来的 key 对应的值有多个，那么就需要通过这个方法获取。





## 6.5 HttpResponse对象



### 6.5.1 常用属性

1. **content**：返回的内容。
2. **status_code**：返回的HTTP响应状态码。
3. **content_type**：返回的数据的MIME类型，默认为 text/html 。

```
浏览器会根据这个属性，来显示数据。
如果是 text/html ，那么就会解析这个字符串，如果 text/plain ，那么就会显示一个纯文本。

常用的 Content-Type 如下：
    text/html（默认的，html文件）
    text/plain（纯文本）
    text/css（css文件）
    text/javascript（js文件）
    multipart/form-data（文件提交）
    application/json（json传输）
    application/xml（xml文件）
```

4. 设置请求头： response['X-Access-Token'] = 'xxxx' 。



### 6.5.2 常用方法



1. **set_cookie**：用来设置 cookie 信息。
2. **delete_cookie**：用来删除 cookie 信息。
3. **write**： HttpResponse 是一个类似于文件的对象，可以用来写入数据到数据体（content）中。



### 6.5.3 JsonResponse类



用来对象 dump 成 json 字符串，然后返回将 json 字符串封装成 Response 对象返回给浏览器。

并且他的 Content-Type 是 application/json 。

```python
from django.http import JsonResponse

def index(request):
	return JsonResponse({"username":"zhiliao","age":18})
```



默认情况下 JsonResponse 只能对字典进行 dump ，如果想要对非字典的数据进行 dump ，那么需要给 JsonResponse 传递一个 safe=False 参数。

```python
from django.http import JsonResponse

def index(request):
    persons = ['张三','李四','王五']
    return HttpResponse(persons)
```

以上代码会报错，应该在使用 HttpResponse 的时候，传入一个 **safe=False** 参数，示例代码如下：

```python
return HttpResponse(persons,safe=False)
```



## 6.6 生成CSV文件



有时候做的网站，需要将一些数据，生成有一个 CSV 文件给浏览器，并且是作为附件的形式下载下来。



### 6.6.1 生成小的CSV文件

生成小的 CSV 文件为例。用 Python 内置的 csv 模块来处理 csv 文件，并且使用 HttpResponse 来将 csv 文件返回回去。

```python
import csv
from django.http import HttpResponse

def csv_view(request):
    
    response = HttpResponse(content_type='text/csv')
    response['Content-Disposition'] = 'attachment; filename="somefilename.csv"'
    writer = csv.writer(response)
    writer.writerow(['username', 'age', 'height', 'weight'])
    writer.writerow(['zhiliao', '18', '180', '110'])
    
    return response

说明：
	1. 在初始化 HttpResponse 的时候，指定了 Content-Type 为 text/csv ，这将告诉浏览器，这是一个 csv 格式的文件而不是一个 HTML 格式的文件，如果用默认值，默认值就是 html ，那么浏览器将把 csv 格式的文件按照 html 格式输出。
    2. 在 response 中添加一个 Content-Disposition 头，用来告诉浏览器该如何处理这个文件，给这个头的值设置为attachment，浏览器将不会对这个文件进行显示，而是作为附件的形式下载，第二个 filename="somefilename.csv" 是用来指定这个 csv 文件的名字。
    3. 使用 csv 模块的 writer 方法，将相应的数据写入到 response 中。
```



### 6.6.2 将csv文件定义成模板



将 csv 格式的文件定义成模板，然后使用 Django 内置的模板系统，并给这个模板传入一个 Context 对象，这样模板系统就会根据传入的 Context 对象，生成具体的 csv 文件。



模板文件

```
{% for row in data %}
    "{{ row.0|addslashes }}", 
    "{{ row.1|addslashes }}", 
    "{{row.2|addslashes }}", 
    "{{ row.3|addslashes }}", 
    "{{ row.4|addslashes }}"
{% endfor %}
```

视图函数

```python
from django.http import HttpResponse
from django.template import loader, Context

def some_view(request):
    
    response = HttpResponse(content_type='text/csv')
    response['Content-Disposition'] = 'attachment; filename="somefilename.csv"'
    csv_data = (
        ('First row', 'Foo', 'Bar', 'Baz'),
        ('Second row', 'A', 'B', 'C', '"Testing"', "Here's a quote"),
    )
    t = loader.get_template('my_template_name.txt')
    response.write(t.render({"data": csv_data}))
    
    return response
```



### 6.6.3 生成大的CSV文件



以上例子是生成的一个小的 csv 文件，如果想生成大型的 csv 文件，以上方式将有可能会发生**超时**的情况（服务器要生成一个大型csv文件，需要的时间可能会超过浏览器默认的超时时间）。

这时候可以借助另外一个类，叫做 StreamingHttpResponse 对象，这个对象是将响应的数据作为一个流返回给客户端，而不是作为一个整体返回。

```python
class Echo:
    """
    定义一个可以执行写操作的类，以后调用csv.writer的时候，就会执行这个方法
    """
    def write(self, value):
        
    	return value
    
def large_csv(request):
    
    rows = (["Row {}".format(idx), str(idx)] for idx in range(655360))
    pseudo_buffer = Echo()
    writer = csv.writer(pseudo_buffer)
    response = StreamingHttpResponse((writer.writerow(row) for row in rows),content_type="text/csv")
    response['Content-Disposition'] = 'attachment; filename="somefilename.csv"'
    
	return response
```

这里构建了一个非常大的数据集 rows ，并且将其变成一个迭代器。

然后因为 StreamingHttpResponse 的第一个参数只能是一个生成器，因此使用圆括号 (writer.writerow(row) for row in rows) ，并且因为要写的文件是 csv 格式的文件，因此需要调用 writer.writerow 将 row 变成一个 csv 格式的字符串。

而调用 writer.writerow 又需要一个中间的容器，因此定义了一个非常简单的类 Echo ，这个类只实现一个 write 方法，以后在执行 csv.writer(pseudo_buffer) 的时候，就会调用 Echo.writer 方法。

注意： StreamingHttpResponse 会启动一个进程来和客户端保持长连接，所以会很消耗资源。所以如果不是特殊要求，尽量少用这种方法。



### 6.6.4 StreamingHttpResponse



这个类是专门用来处理流数据的。

使得在处理一些大型文件的时候，不会因为服务器处理时间过长而到时连接超时。

这个类不是继承自 HttpResponse ，并且跟 HttpResponse 对比有以下几点区别：

1. 这个类没有属性 content ，相反是 streaming_content 。

2. 这个类的 streaming_content 必须是一个可以迭代的对象。
3. 这个类没有 write 方法，如果给这个类的对象写入数据将会报错。

注意： StreamingHttpResponse 会启动一个进程来和客户端保持长连接，所以会很消耗资源。所以如果不是特殊要求，尽量少用这种方法。







## 6.7 类视图



### 6.7.1 View



django.views.generic.base.View是主要的类视图，所有的类视图都是继承自他。

如果我们写自己的类视图，也可以继承自他。

然后再根据当前请求的 method ，来实现不同的方法。

比如，实现get方法，views.py：

```python
from django.views import View

class BookDetailView(View):
    def get(self,request,*args,**kwargs):
        return render(request,'detail.html')
```

 urls.py 

```python
urlpatterns = [
	path("detail/<book_id>/",views.BookDetailView.as_view(),name='detail')
]
```

除了 get 方法， View 还支持以下方法：

 ['get','post','put','patch','delete','head','options','trace'] 。



如果用户访问了 View 中没有定义的方法。

比如你的类视图只支持 get 方法，而出现了 post 方法，那么就会把这个请求转发给 http_method_not_allowed(request,*args,**kwargs) 。

示例代码如下：

```python
class AddBookView(View):
    def post(self,request,*args,**kwargs):
    	return HttpResponse("书籍添加成功！")
    
    def http_method_not_allowed(self, request, *args, **kwargs):
        return HttpResponse("您当前采用的method是：%s，本视图只支持使用post请求！" % request.method)
```

urls.py

```python
path("addbook/",views.AddBookView.as_view(),name='add_book')
```



如果你在浏览器中访问 addbook/ ，因为浏览器访问采用的是 get 方法，而 addbook 只支持 post 方法，因此以上视图会返回您当前采用的 method 是： GET ，本视图只支持使用 post 请求！。

其实不管是 get 请求还是 post 请求，都会走 **dispatch(request,*args,**kwargs)** 方法，所以如果实现这个方法，**将能够对所有请求都处理到**。



### 6.7.2 TemplateView



django.views.generic.base.TemplateView，这个类视图是专门用来返回模版的。

在这个类中，有两个属性是经常需要用到的，一个是 template_name ，这个属性是用来存储模版的路径，TemplateView 会自动的渲染这个变量指向的模版。

另外一个是 get_context_data ，这个方法是用来返回上下文数据的，也就是在给模版传的参数的。

示例代码如下：

```python
from django.views.generic.base import TemplateView

class HomePageView(TemplateView):
    
    template_name = "home.html"
    
    def get_context_data(self, **kwargs):
        
        context = super().get_context_data(**kwargs)
        context['username'] = "ydc"
        
        return context
```

urls.py

```python
from django.urls import path
from myapp.views import HomePageView

urlpatterns = [
	path('', HomePageView.as_view(), name='home'),
]
```



如果在模版中不需要传递任何参数，那么可以直接只在 urls.py 中使用 TemplateView 来渲染模版。

示例代码如下：

```python
from django.urls import path
from django.views.generic import TemplateView

urlpatterns = [
	path('about/', TemplateView.as_view(template_name="about.html")),
]
```



### 6.7.3 ListView



在网站开发中，经常会出现需要列出某个表中的一些数据作为列表展示出来。

比如文章列表，图书列表等等。

在 Django 中可以使用 ListView 快速实现这种需求。

示例代码如下：

```python
class ArticleListView(ListView):
    
    model = Article
    template_name = 'article_list.html'
    paginate_by = 10
    context_object_name = 'articles'
    ordering = 'create_time'
    page_kwarg = 'page'
    
    def get_context_data(self, **kwargs):
        
        context = super(ArticleListView, self).get_context_data(**kwargs)
        
        return context
    
    def get_queryset(self):
        
    	return Article.objects.filter(id__lte=89)
```

1. ArticleListView 是继承自 ListView 。
2. model ：重写 model 类属性，指定这个列表是给哪个模型的。
3. template_name ：指定这个列表的模板。
4. paginate_by ：指定这个列表一页中展示多少条数据。
5. context_object_name ：指定这个列表模型在模板中的参数名称。
6. ordering ：指定这个列表的排序方式。
7. page_kwarg ：获取第几页的数据的参数名称。默认是 page 。
8. get_context_data ：获取上下文的数据。
9. get_queryset ：如果你提取数据的时候，并不是要把所有数据都返回，那么你可以重写这个方法。将一些不需要展示的数据给过滤掉。



### 6.7.4 Paginator和Page类



Paginator 和 Page 类都是用来做分页的。

他们在 Django 中的路径为 django.core.paginator.Paginator 和 django.core.paginator.Page 。

以下对这两个类的常用属性和方法做解释：



### 6.7.5 Paginator常用属性和方法



1. count ：总共有多少条数据。
2. num_pages ：总共有多少页。
3. page_range ：页面的区间。比如有三页，那么就 range(1,4) 。



### 6.7.6 Page常用属性和方法



1. has_next ：是否还有下一页。
2. has_previous ：是否还有上一页。
3. next_page_number ：下一页的页码。
4. previous_page_number ：上一页的页码。
5. number ：当前页。
6. start_index ：当前这一页的第一条数据的索引值。
7. end_index ：当前这一页的最后一条数据的索引值。



### 6.7.7 给类视图添加装饰器



在开发中，有时候需要给一些视图添加装饰器。

如果用函数视图那么非常简单，只要在函数的上面写上装饰器就可以了。

但是如果想要给 **类** 添加装饰器，那么可以通过以下两种方式来实现：



1. 装饰dispatch方法

```python
from django.utils.decorators import method_decorator


def login_required(func):
    def wrapper(request,*args,**kwargs):
        if request.GET.get("username"):
        	return func(request,*args,**kwargs)
        else:
            return redirect(reverse('index'))
    return wrapper
    
class IndexView(View):
    def get(self,request,*args,**kwargs):
    	return HttpResponse("index")
    	
    @method_decorator(login_required)
    def dispatch(self, request, *args, **kwargs):
    	super(IndexView, self).dispatch(request,*args,**kwargs)
```



### 6.7.8 直接装饰在整个类上



```python
from django.utils.decorators import method_decorator

def login_required(func):
    def wrapper(request,*args,**kwargs):
    	if request.GET.get("username"):
    		return func(request,*args,**kwargs)
    	else:
    		return redirect(reverse('login'))
    return wrapper


@method_decorator(login_required,name='dispatch')
class IndexView(View):
    def get(self,request,*args,**kwargs):
    	return HttpResponse("index")
    
    def dispatch(self, request, *args, **kwargs):
    	super(IndexView, self).dispatch(request,*args,**kwargs)
```





## 6.8 错误处理



在一些网站开发中，经常会需要捕获一些错误，然后将这些错误返回，或者是将这个错误的请求做一些日志保存。



### 6.8.1 常用的错误码



```
404 ：服务器没有指定的url。
403 ：没有权限访问相关的数据。
405 ：请求的 method 错误。
400 ：bad request ，请求的参数错误。
500 ：服务器内部错误，一般是代码出bug了。
502 ：一般部署的时候见得比较多，一般是 nginx 启动了，然后 uwsgi 有问题。
```



### 6.8.2 自定义错误模板



在碰到404 ， 500 错误的时候，想要返回自己定义的模板。

那么可以直接在 templates 文件夹下创建相应错误代码的 html 模板文件。

那么以后在发生相应错误后，会将指定的模板返回。



### 6.8.3 错误处理的解决方案



对于 404 和 500 这种自动抛出的错误。

可以直接在 templates 文件夹下新建相应错误代码的模板文件。

而对于其他的错误，可以专门定义一个 app ，用来处理这些错误。

```python
from django.shortcuts import render

def view_403(request):
	return render(request, 'errors/403.html', status=403)

def view_400(request):
	return render(request, 'errors/400.html', status=400)
```



# 7 表单



## 7.1 表单概述



### 7.1.1 HTML中的表单



单纯从前端的 html 来说，表单是用来提交数据给服务器的,不管后台的服务器用的是 Django 还是 PHP 语言还是其他语言。

只要把 input 标签放在 form 标签中，然后再添加一个提交按钮，那么以后点击提交按钮，就可以将 input 标签中对应的值提交给服务器了。



### 7.1.2 Django中的表单



Django 中的表单丰富了传统的 HTML 语言中的表单。在 Django 中的表单，主要做以下两件事：
1. 渲染表单模板。
2. 表单验证数据是否合法。



### 7.1.3 Django中表单使用流程



django中的表单和数据模型很相似。

这里以留言板为例。

首先我们在后台服务器定义一个表单类，继承自 django.forms.Form 。

示例代码如下：

```python
# forms.py
class MessageBoardForm(forms.Form):
    
    title = forms.CharField(max_length=3,label='标题',min_length=2,error_messages={"min_length":'标题字符段不符合要求！'})
    content = forms.CharField(widget=forms.Textarea,label='内容')
    email = forms.EmailField(label='邮箱')
    reply = forms.BooleanField(required=False,label='回复')
```



然后在视图中，根据是 GET 还是 POST 请求来做相应的操作。

如果是 GET 请求，那么返回一个空的表单，如果是 POST 请求，那么将提交上来的数据进行校验。

示例代码如下：

```python
# views.py
class IndexView(View):

    def get(self,request):
        
        form = MessageBoardForm()
        return render(request,'index.html',{'form':form})


    def post(self,request):
        
        form = MessageBoardForm(request.POST)
        if form.is_valid():
            title = form.cleaned_data.get('title')
            content = form.cleaned_data.get('content')
            email = form.cleaned_data.get('email')
            reply = form.cleaned_data.get('reply')
            return HttpResponse('success')
        else:
            print(form.errors)
            return HttpResponse('fail')
```



在使用 GET 请求的时候，传了一个 form 给模板，那么以后模板就可以使用 form 来生成一个表单的 html 代码。

在使用 POST 请求的时候，根据前端上传上来的数据，构建一个新的表单，这个表单是用来验证数据是否合法的，如果数据都验证通过了，那么可以通过 cleaned_data 来获取相应的数据。

在模板中渲染表单的 HTML 代码如下：

```html
<form action="" method="post">
    <table>
        <tr>
            <td></td>
            <td><input type="submit" value="提交"></td>
        </tr>
    </table>
</form>
```

在最外面给了一个 form 标签，然后在里面使用了 table 标签来进行美化，在使用 form 对象渲染的时候，使用的是 table 的方式，当然还可以使用 ul 的方式（ as_ul ），也可以使用 p 标签的方式（ as_p ），并且加上了一个提交按钮。



## 7.2 用表单验证数据



### 7.2.1 常用的Field



使用 Field 可以是对**数据验证的第一步**。你期望这个提交上来的数据是什么类型，那么就使用什么类型的 Field 。



**CharField**

```
用来接收文本。

参数：
    max_length：这个字段值的最大长度。
    min_length：这个字段值的最小长度。
    required：这个字段是否是必须的。默认是必须的。
    error_messages：在某个条件验证失败的时候，给出错误信息。
```



**EmailField**

```
用来接收邮件，会自动验证邮件是否合法。

错误信息的key： required 、 invalid 。
```



**FloatField**

```
用来接收浮点类型，并且如果验证通过后，会将这个字段的值转换为浮点类型。

参数：
    max_value：最大的值。
    min_value：最小的值。
    
错误信息的key： required 、 invalid 、 max_value 、 min_value 。
```



**IntegerField**

```
用来接收整形，并且验证通过后，会将这个字段的值转换为整形。

参数：
    max_value：最大的值。
    min_value：最小的值。
    
错误信息的key： required 、 invalid 、 max_value 、 min_value 。
```



**URLField**

```
用来接收 url 格式的字符串。

错误信息的key： required 、 invalid 。
```



### 7.2.2 常用验证器



在验证某个字段的时候，可以传递一个 validators 参数用来指定验证器，**进一步对数据进行过滤**。

验证器有很多，但是很多验证器其实已经通过这个 Field 或者一些参数就可以指定了。

比如 EmailValidator ，可以通过 EmailField 来指定，比如 MaxValueValidator ，可以通过 max_value 参数来指定。

以下是一些常用的验证器：

1. MaxValueValidator ：验证最大值。
2. MinValueValidator ：验证最小值。
3. MinLengthValidator ：验证最小长度。
4. MaxLengthValidator ：验证最大长度。
5. EmailValidator ：验证是否是邮箱格式。
6. URLValidator ：验证是否是 URL 格式。
7. RegexValidator ：如果还需要更加复杂的验证，可以通过正则表达式的验证器： RegexValidator 。
8. 比如现在要验证手机号码是否合格，可以通过以下代码实现：

```python
class MyForm(forms.Form):
    telephone = forms.CharField(validators=[validators.RegexValidator("1[345678]\d{9}",message='请输入正确格式的手机号码！')])
```



### 7.2.3 自定义验证



有时候对一个字段验证，不是一个长度，一个正则表达式能够写清楚的，还需要一些其他复杂的逻辑，那么可以对某个字段，进行自定义的验证。

比如在注册的表单验证中，想要验证手机号码是否已经被注册过了，那么这时候就需要在数据库中进行判断才知道。

对某个字段进行自定义的验证方式是，定义一个方法，这个方法的名字定义规则是： clean_fieldname 。

如果验证失败，那么就抛出一个验证错误。

比如要验证用户表中手机号码之前是否在数据库中存在，那么可以通过以下代码实现：

```python
class MyForm(forms.Form):
    
    telephone = forms.CharField(validators[validators.RegexValidator("1[345678]\d{9}", message='请输入正确格式的手机号码！')])
    
    def clean_telephone(self):
        
        # 获取通过验证后的表单数据
        telephone = self.cleaned_data.get('telephone')
        exists = User.objects.filter(telephone=telephone).exists()
        if exists:
        	raise forms.ValidationError("手机号码已经存在！")
        return telephone
```



以上是对某个字段进行验证，如果验证数据的时候，**需要针对多个字段进行验证**，那么可以重写 clean 方法。

比如要在注册的时候，要判断提交的两个密码是否相等。

那么可以使用以下代码来完成：

```python
class MyForm(forms.Form):
    
    telephone = forms.CharField(validators[validators.RegexValidator("1[345678]\d{9}", message='请输入正确格式的手机号码！')])
    pwd1 = forms.CharField(max_length=12)
    pwd2 = forms.CharField(max_length=12)
    
    def clean(self):
        
        cleaned_data = super().clean()
        pwd1 = cleaned_data.get('pwd1')
        pwd2 = cleaned_data.get('pwd2')
        if pwd1 != pwd2:
        	raise forms.ValidationError('两个密码不一致！')
```



### 7.2.4 提取错误信息



如果验证失败了，那么有一些错误信息是需要传给前端的。

这时候可以通过以下属性来获取：

1. form.errors ：这个属性获取的错误信息是一个包含了 html 标签的错误信息。

2. form.errors.get_json_data() ：这个方法获取到的是一个字典类型的错误信息。将某个**字段的名字作为 key** ，**错误信息作为value**的一个字典。

3. form.as_json() ：这个方法是将 form.get_json_data() 返回的字典 dump 成 json 格式的字符串，方便进行传输。

4. 上述方法获取的字段的错误值，都是一个比较复杂的数据。

   比如以下：

```python
{
    'username': [
        {'message': 'Enter a valid URL.', 'code': 'invalid'}, 
        {'message': 'Ensure this value has at most 4 characters (it has 22).', 'code': 'max_length'}
    ]
}
```



那么如果我只想把错误信息放在一个列表中，而不要再放在一个字典中。

这时候我们可以定义一个方法，把这个数据重新整理一份。

实例代码如下：

```python
class MyForm(forms.Form):
    
    username = forms.URLField(max_length=4)
    
    def get_errors(self):
        
        errors = self.errors.get_json_data()
        new_errors = {}
            
        for key,message_dicts in errors.items():
            messages = []
            for message in message_dicts:
            	messages.append(message['message'])
            new_errors[key] = messages
            
        return new_errors
```

这样就可以把某个字段所有的错误信息直接放在这个列表中。



## 7.3 ModelForm



在写表单的时候，会发现表单中的 Field 和模型中的 Field 基本上是一模一样的，而且表单中需要验证的数据，也就是我们模型中需要保存的。

那么这时候就可以将模型中的字段和表单中的字段进行绑定。

比如现在有个 Article 的模型。示例代码如下：

```python
from django.db import models
from django.core import validators

class Article(models.Model):
    
    title = models.CharField(max_length=10,validators[validators.MinLengthValidator(limit_value=3)])
    content = models.TextField()
    author = models.CharField(max_length=100)
    category = models.CharField(max_length=100)
    create_time = models.DateTimeField(auto_now_add=True)
```



那么在写表单的时候，就不需要把 Article 模型中所有的字段都重复写一遍了。

示例代码如下：

```python
from django import forms

class MyForm(forms.ModelForm):
    
    class Meta:
        model = Article
        fields = "__all__"
```



MyForm 是继承自 forms.ModelForm ，然后在表单中定义了一个 Meta 类，在 Meta 类中指定了 model=Article ，以及 fields="all" ，这样就可以将 Article 模型中所有的字段都复制过来，进行验证。

如果只想针对其中几个字段进行验证，那么可以给 fields 指定一个列表，将需要的字段写进去。

比如只想验证 title 和 content ，那么可以使用以下代码实现：

```python
from django import forms

class MyForm(forms.ModelForm):
    
    class Meta:
        model = Article
        fields = ['title','content']
```



如果要验证的字段比较多，只是除了少数几个字段不需要验证，那么可以使用 exclude 来代替 fields 。

比如不想验证 category ，那么示例代码如下：

```python
class MyForm(forms.ModelForm):

    class Meta:
        model = Article
        exclude = ['category']
```



### 7.3.1 自定义错误消息



使用 ModelForm ，因为字段都不是在表单中定义的，而是在模型中定义的，因此一些错误消息无法在字段中定义。

那么这时候可以在 Meta 类中，定义 error_messages ，然后把相应的错误消息写到里面去。

示例代码如下：

```python
class MyForm(forms.ModelForm):

    class Meta:
        model = Article
        exclude = ['category']
        error_messages ={
            'title':{
                'max_length': '最多不能超过10个字符！',
                'min_length': '最少不能少于3个字符！'
            },
            'content': {
            	'required': '必须输入content！',
            }
        }
```

**save方法**



ModelForm 还有 save 方法，可以在验证完成后直接调用 save 方法，就可以将**数据保存到数据库**中了。

示例代码如下：

```python
form = MyForm(request.POST)
if form.is_valid():
    form.save()
        return HttpResponse('succes')
else:
	print(form.get_errors())
	return HttpResponse('fail')
```



这个方法必须要在 clean 没有问题后才能使用，如果**在 clean 之前使用**，会抛出异常。

另外，在调用 save 方法的时候，如果传入一个 commit=False ，那么只会生成这个模型的对象，而不会把这个对象真正的插入到数据库中。



比如表单上验证的字段没有包含模型中所有的字段，这时候就可以先创建对象，再根据填充其他字段，把所有字段的值都补充完成后，再保存到数据库中。

示例代码如下：

```python
form = MyForm(request.POST)
if form.is_valid():
    article = form.save(commit=False)
    article.category = 'Python'
    article.save()
    return HttpResponse('succes')
else:
	print(form.get_errors())
	return HttpResponse('fail')
```












# 1 入门



## 1.1 安装Django

```
pip install django

说明：
	1. 如果不指定版本，则安装最新的LTS版本
	2. 还是指定一下好，pip install django==2.2
	3.
```



## 1.2 Django项目



### 1.2.1 参加一个Django项目



在控制台执行

```
django-admin startproject mysite

说明：
	1. mysite是项目名
	2. 执行命令后，会创建一个跟项目相同名字的文件夹作为容器，然后把项目放在里面
	3. 如果不想参加文件夹容器，可以自己创建，
		django-admin startproject ak .
		即在当前目录创建Django项目
```



### 1.2.2 Django项目的目录结构



```
mysqite              # 目录容器，跟Django没啥关系，可以自己取名
	mysite           # Django项目的名称，是一个Python包
		__init__.py  # 空文件，标识该文件夹是一个包
		settings.py  # 项目的配置文件
		urls.py      # 项目的url说明文件，项目的url入口文件
		wsgi.py      # 兼容WSGI的Web服务器的入口点，用于伺服项目。
	manage.py        # 项目启动脚本，项目的操作基本都基于这玩意儿
```



### 1.2.3 Django项目的配置文件



Django项目的配置文件都会放在 settings.py文件下。

当然，也可以把配置文件分开放，然后在 settings.py的最后进行import。



- TIME_ZONE

时区配置，一般都先设置成自己的时区

```
TIME_ZONE = 'Asia/Shanghai'
```



- INSTALLED_APPS

是应该列表，里面都放着Django中的app（应用）。

在Django中，应用的设计是这样的：

```
一个应用可以在多个项目中使用，而且应用可以打包，供其他项目使用。
```

```python
INSTALLED_APPS = [
    'django.contrib.admin',  # 内置的管理后台
    'django.contrib.auth',  # 身份验证系统 权限系统
    'django.contrib.contenttypes',  # 内容类型框架
    'django.contrib.sessions',  # 会话框架  缓存之类的
    'django.contrib.messages',  # 消息框架
    'django.contrib.staticfiles',  # 管理静态文件的框架
]

说明：
	1. 这些应用是默认包含的
    2. 自己参加的应用直接添加后面即可
    3. 某些应用是用到数据库的，所以需要先迁移一下数据库（Django默认带列以sqlit.db）:
    	python manage.py migtate
```



### 1.2.4 启动项目



执行以下命令，就成了。😄

```
python manage.py runserver
```

真地址栏输入：http://127.0.0.1:8000/



## 1.3 模型-视图-控制器（MVC）设计模式



MVC是一个设计理念，几乎所有的Web框架都围绕它去设计实现（我也不知道，猜的:dog:）。

- 模型（M）
  - 是数据的表述。
  - 它不是真正的数据，而是数据的接口。
  - 使用模型从数据库中获取数据时，无需知道底层数据库错综复杂的知识。
  - 模型通常还会为数据库提供一层抽象，这样同一个模型就能使用不同的数据库。



- 视图（V）
  - 是你看到的界面。
  - 它是模型的表现层。
  - 在电脑中，视图是你在浏览器中看到的 Web 应用的页面，或者是桌面应用的 UI。
  - 视图还提供了收集用户输入的接口。



- 控制器（C）
  - 控制模型和视图之间的信息流动。
  - 它通过程序逻辑判断通过模型从数据库中获取什么信息，以及把什么信息传给视图。
  - 它还通过视图从用户那里收集信息，并且实现业务逻辑：变更视图，或者通过模型修改数据，或者二者兼具。



结构如图（百度的😄）：

![6 MVC](D:\Me\Way\Django2\imags\6 MVC.jpg)



设计理念是死的，框架形式也那样了，真正难的是如何理解各层的作用，不同的框架往往会使用不同的方式实现同样的功能。



## 1.4 MTV



Django 严格遵守 MVC 模式，但是有自己的实现逻辑。

“C”部分由框架处理，多数时候，我们的工作在模型、模板和视图中，因此 Django 经常被称为 MTV 框架。



在 MTV 开发模式中:

- M 表示“模型”
  - 即数据访问层。
  - 这一层包含所有与数据相关的功能：访问数据的方式、验证数据的方式、数据的行为、数据之间的关系。
- T 表示“模板”
  - 即表现层。
  - 这一层包含表现相关的决策：在网页或其他文档类型中如何显示某个东西。
- V 表示“视图”
  - 即业务逻辑层。
  - 这一层包含访问模型和选择合适模板的逻辑。
  - 可以把视图看做模型和模板之间的桥梁。



Django 的视图更像是 MVC 中的控制器，而 MVC 中的视图是 Django 中的模板。

反正对着来就行了

```
MVC
MTV
```





# 2 视图和URL配置



## 2.1 第一个视图：静态内容



视图函数写在 views.py中（当然，后面是可以拆分的）。

url配置写在 urls.py中，如果有创建应用，则把应用的 urls.py包含到项目的 urls.py



视图view.py

```python
from django.http import HttpResponse

def hello(request):
	return HttpResponse("Hello world")
```



url.py

```python
from django.conf.urls import include, url
from django.contrib import admin
from mysite.views import hello

urlpatterns = [
    url(r'^admin/', include(admin.site.urls)),
    url(r'^hello/$', hello),  # url和视图函数绑定在一起，以对象的形式传入hello视图函数，而没有调用函数（即没有括号）。
]
```

启动项目后，服务/hello/地址就可以看到Hello world。



### 2.1.1 正则表达式



正则表达式（regular expression，简称 regexes）是指定文本模式的简洁方式。

Django 的 URL 配置允许使用任何正则表达式匹配复杂的 URL，但是实际上只会使用部分符号。



常用符号

| 符号       | 匹配内容                                                     |      |
| ---------- | ------------------------------------------------------------ | ---- |
| . （点号） | 单个字符                                                     |      |
| \d         | 单个数字                                                     |      |
| [A-Z]      | A-Z（大写）之间的单个字母                                    |      |
| [a-z]      | a-z（小写）之间的单个字母                                    |      |
| [A-Za-z]   | a-z（不区分大小写）之间的单个字母                            |      |
| +          | 一个或多个前述表达式（例如， \d+ 匹配一个或多个数字）        |      |
| [ˆ/]+      | 一个或多个字符，直到遇到斜线（不含）                         |      |
| ？         | 零个或一个前述表达式（例如， \d? 匹配零个或一个数字）        |      |
| *          | 零个或多个前述表达式（例如， \d* 匹配零个、一个或多个数字）  |      |
| {1,3}      | 介于一个到三个之间（含）的前述表达式（例如， \d{1,3} 匹配一个、两个或三个数字） |      |



### 2.1.2 关于 404 错误



在调试模式下，当访问不存在的url时，将会返回404错误响应码，并返回一下调试信息。

在生产模式下，访问不存在的url仅返回404错误信息。



### 2.1.3 关于网站根地址



访问http://127.0.0.1:8000/ 时，会404，因为没有没有配置根地址。



为网站根地址实现视图时，使用的 URL 模式是 ˆ$ ，即匹配空字符串。

```python
url(r'^$', home)
```

尽量不要这样

```python
url(r'^/$', home)
```





### 2.1.4 Django 处理请求的过程



用户访问/hello/地址，浏览器向Django服务器发送请求；

Django从settings.py文件找到ROOT_URLCONF配置，找到根URL配置；

在根URL配置中按顺序比较所有的URL模式，找到和/hello/匹配的那个；

调用对应的视图函数；视图函数处理完后，必须返回一个HttpResponse或BaseHttpResponse对象；

Django把视图函数响应的对象解析成HTTP响应，得到网页；



简单的流程就是：

1. 请求 /hello/ 。

2. Django 查看 ROOT_URLCONF 设置，找到根 URL 配置。
3. Django 比较 URL 配置中的各个 URL 模式，找到与 /hello/ 匹配的那个。
4. 如果找到匹配的模式，调用对应的视图函数。
5. 视图函数返回一个 HttpResponse 对象。
6. Django 把 HttpResponse 对象转换成正确的 HTTP 响应，得到网页。

```
说明：
	1. 找不到URL模式就报错
	2. 
```





## 2.2 第二个视图:动态内容



显示动态的内容。

views.py

```python
from django.shortcuts import HttpResponse
import datetime


def hello(request):

	return HttpResponse('Hello World')

def current_datetime(request):
	now = datetime.datetime.now()
	html = 'It is now %s.' % now
    
	return HttpResponse(html)
```

urls.py

```python
from django.contrib import admin
from django.urls import path
from django.conf.urls import url
from mysite.views import hello
from mysite.views import current_datetime


urlpatterns = [
    url(r'admin/', admin.site.urls),
    url(r'hello/', hello),
    url(r'time/', current_datetime),
]
```





## 2.3 URL 配置和松耦合



松耦合是一种软件开发方式，其价值在于让组件可以互换。

如果两部分代码之间是松耦合的，那么改动其中一部分对另一部分的影响很小，甚至没有影响。

Django 的 URL 配置就很好地运用了这个原则。在 Django Web 应用中，URL 定义与所调用的视图函数之间是松耦合的，即某个功能使用哪个 URL 与视图函数的实现本身放在两个地方。



## 2.4 第三个视图：动态url



有些url是动态组成的，比如 /articles/1/，则可以说是正则动态匹配url。

```python
url(r'^articles/\d+/$', detail)

说明：
	1. \d+表示匹配数字，5，200，100000
```

可以指定数字的位数

```python
url(r'^articles/\d{1, 2}/$', detail)

说明：
	1. \d{1, 2}表示一位或两位数字
	1. 这样仅仅是url中的匹配而已，视图函数无法获取到那个数字
```

```python
url(r'^articles/(\d{1, 2})/$', detail)

说明：
	1. 加上(),就可以把这参数传递到视图函数使用了
    2. ()就是抽取数据用的
```



将url上的变量传递到视图函数使用

views.py

```python
from django.shortcuts import HttpResponse


def url_num(request, num):

	html = 'your number is %s' % num

	return HttpResponse(html)
```

urls.py

```python
from django.conf.urls import url
from mysite.views import url_num

urlpatterns = [
    url(r'^num/(\d{1,2})/', url_num),
]
```

浏览器输入：http://127.0.0.1:8080/num/99/







## 2.5 关于Django url



1. Python 中的反斜线与正则表达式中的反斜线有冲突，因此在 Django 中定义正则表达式时最好使用原始字符串。

```python
# 在url前面加 'r' 字符，将字符串变成原始字符串，即让那些有特殊含义的字符变成普通字符
url(r'^admin/', include(admin.site.urls))
```

2.  url模式的一下不同



**url: http://127.0.0.1:8000/hello/** 



- 即想匹配的是/hello/，但url写的却是url(r'^hello/$', hello)

  原因：Django 在检查 URL 模式时会把入站 URL 前面的斜线去掉。

  所以，URL 模式中没有前导斜线。

  

- 脱字符号（^）和一个美元符号（$）

  即正则表达式的含义，^：以...开头，$：以...结尾

  原则：

  ​	（1）一般都在前面^

  ​	（2）如果确定本url是最后一级了，就加$，跟项目的urls.py那里的url最后都不加，不然匹配不到子路由。



- /hello

  如果请求的url是/hello，即没有后面的那个斜线。

  Django会这么做：

  ```
  默认情况下，如果请求的 URL 不匹配任何 URL 模式，而且末尾没有斜线，那么 Django 会把它重定向到末尾带斜线的 URL。
  ```

  

# 3 Django模板





## 3.1 模板简述



Django 模板是一些文本字符串，作用是把文档的表现与数据区分开。

模板定义一些占位符和基本的逻辑（模板标签），规定如何显示文档。通常，模板用于生成 HTML，不过 Django 模板可以生成任何基于文本的格式。



```html
<html>
    <head>
        <title>Ordering notice</title>
    </head>
    <body>
        <h1>Ordering notice</h1>
        <p>Dear {{ person_name }},</p>
        <p>Thanks for placing an order from {{ company }}. It's scheduled to ship on {{
            ship_date|date:"F j, Y" }}.</p>
        <p>Here are the items you've ordered:</p>
        <ul>
            {% for item in item_list %}
            <li>{{ item }}</li>{% endfor %}
        </ul>
        {% if ordered_warranty %}
        <p>Your warranty information will be included in the packaging.</p>
        {% else %}
        <p>
            You didn't order a warranty, so you're on your own when
            the products inevitably stop working.
        </p>
        {% endif %}
        <p>Sincerely,<br />{{ company }}</p>
    </body>
</html>
```

- {{ arg }}：双花括号包围的是变量，即不变量的值插入到html页面
- {% if %}：花括号和百分号包围的文本是模板标签，用于空中模板做事情。
-  {{ ship_date|date:"F j, Y" }}：涨这样的是过滤器，过滤器使用管道符号（ | ）传值
- 



## 3.2 模板系统



Django默认使用的模板引擎是DTL，即 Django Template Language，还有一个流行的模板引擎是Jinja2。



DTL的基本工作流程是：

- 以字符串形式提供原始的模板代码，创建 Template 对象。
- 在 Template 对象上调用 render() 方法，传入一系列变量（上下文）。
- 返回的是完全渲染模板后得到的字符串，模板中的变量和模板标签已经根据上下文求出值了。

代码流程：

```python
from django import template

# 创建模板
t = template.Template('My name is {{ name }}.')
# 创建上下文对象
c = template.Context({'name': 'Nige'})
# 渲染
print (t.render(c))
# 输出：My name is Nige.
```



### 3.2.1 创建Template对象



创建 Template 对象最简单的方式是直接实例化。 

Template 类在 django.template 模块中，构造方法接受一个参数，即原始的模板代码。

创建 Template 对象时，模板系统会把原始的模板代码编译成内部使用的优化形式，为渲染做好准备。



“块级标签”和“模板标签”是同一个事物。

遇到下述各种情况时，模板系统都会抛出 TemplateSyntaxError ：

```html
无效标签
有效标签的无效参数
无效过滤器
有效过滤器的无效参数
无效模板句法
未关闭的标签（对需要结束标签的模板标签而言）
```



### 3.2.2  渲染模板



有了 Template 对象之后，可以为其提供上下文，把数据传给它。

上下文就是一系列模板变量和相应的值。模板使用上下文填充变量，求值标签。

在 Django 中，上下文使用 django.template 模块中的 Context 类表示。

它的构造方法接受一个可选参数：一个字典，把变量名映射到值上。





## 3.3 字典和上下文



Python 字典是键和值的映射。 Context 对象类似于字典，但是它有额外的功能。

变量名必须以字母开头（A-Z 或 a-z），随后可以跟着任意多个字母、数字、下划线和点号。
























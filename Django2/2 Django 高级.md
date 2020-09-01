



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





### 3.3.1 上下文的查找



**点号**可以访问属性、字典的键、方法或对象的索引。

- 字典的访问

```
user = {'name': 'ydc'}

在模板中：
	user.name
```

- 访问属性

```
d = datetime.date(2020, 8, 20)

在模板中：
	d.year
	d.month
	d.day
```

- 访问对象的属性

```python
class User(object):
	def __init__(self, name, age):
        self.name = name
        self.age = age

user = User('ydc', 25)
        
在模板中：
	user.name
    user.age
```

- 引用对象的方法

```
string = 'ydc'

在模板中：
	sting.upper
	string.isdigit

说明：
	1. 方法调用中没有括号，不能给方法传递变量，只能调用无需参数的方法。
	2. 在Python中则是：sting.upper()
```

- 访问列表索引

```
lst = [1, 2, 3, 4, 5]

在模板中：
	lst.0
	lst.1
	lst.2

说明：
	1. 不允许使用负数索引。lst.-1 会炸，报TemplateSyntaxError错。
	2. 
```



注意：

	1. 点号查找可以嵌套多层。 {{ person.name.upper }}，相当于一个字典查找（ person['name'] ）加上一个方法调用（ upper() ）
 	2. 模板系统将使用第一个可用的类型，这是一种短路逻辑。



### 3.3.2 模板中方法调用的行为



模板中的方法调用要复杂，且需要注意：

- 在方法查找的过程中，如果方法抛出异常，异常会向上冒泡，除非异常有 silent_variable_failure 属性，而且值为 True 。如果异常确实有 silent_variable_failure 属性，使用引擎的 string_if_invalid配置选项（默认为一个空字符串）渲染变量。

- 方法不能有必须的参数。否则，模板系统向后移动到下一种查询类型（列表索引查询）。
- Django 限制了在模板中可以处理的逻辑量，因此在模板中不能给方法传递参数。数据应该在视图中计算之后再传给模板显示。
- 假如有个 BankAccount 对象

```
它有个 delete() 方法。如果模板中有 {{ account.delete }} 这样的内容，其中 account 是 BankAccount 对象，那么渲染模板时会把 account 删除。为了避免这种行为，在方法上设定函数属性 alters_data ：
	def delete(self):
        # 删除账户
        delete.alters_data = True

这样标记之后，模板系统不会执行方法。继续使用前面的例子。如果模板中有 {{ account.delete }} ，而 delete() 方法设定了 alters_data=True ，那么渲染模板时不会执行 delete() 方法，引擎会使用string_if_invalid 的值替换那个变量。

为 Django 模型对象动态生成的 delete() 和 save() 方法自动设定了 alters_data = True 。
```



### 3.3.3  如何处理无效变量



如果变量不存在，模板系统在变量处插入引擎的 string_if_invalid 配置选项。

这个选项的默认值为一个空字符串。

这一行为比抛出异常好，因为遇到人为错误时能迅速恢复。





## 3.4 基本的模板标签和过滤器



Django 的模板系统自带了一些内置的标签和过滤器。

过滤器跟Python里面的内置的函数功能几乎一样，就写法不一样。



### 3.4.1 标签



#### **if/else**



{% if  %} 计算变量的值，如果为真（即存在、不为空，不是假值），模板系统显示 {% if %} 和 {% endif %}之间的内容。

{% if %} 支持使用 and 、 or 或 not 测试多个变量，或者取反指定的变量。

```python
{% if athlete_list and not coach_list %}
	<p>There are some athletes and absolutely no coaches. </p>
{% endif %}
```



#### **for**



{% for %} 标签用于迭代序列中的各个元素。

每次迭代时，模板系统会渲染 {% for %} 和 {% endfor %} 之间的内容。



```python
<ul>
    {% for athlete in athlete_list %}
    	<li>{{ athlete.name }}</li>
    {% endfor %}
</ul>
```



在标签中添加 reversed ，反向迭代列表：

```
{% for athlete in athlete_list reversed %}
	{{ athlete }}
{% endfor %}
```



{% for %} 标签可以嵌套:

```python
{% for athlete in athlete_list %}
    <h1>{{ athlete.name }}</h1>
    <ul>
        {% for sport in athlete.sports_played %}
        	<li>{{ sport }}</li>
        {% endfor %}
    </ul>
{% endfor %}
```



如果需要迭代由列表构成的列表，可以把每个子列表中的值拆包到独立的变量中。
比如说上下文中有一个包含 (x,y) 坐标点的列表，名为 points ，可以使用下述模板输出这些坐标点：

```python
{% for x, y in points %}
	<p>There is a point at {{ x }},{{ y }}</p>
{% endfor %}
```



如果需要访问字典中的元素，也可以使用这个标签。

如果上下文中包含一个字典 data ，可以使用下述模板显示字典的键和值：

```python
{% for key, value in data.items %}
	{{ key }}: {{ value }}
{% endfor %}
```



 for 标签支持一个可选的 {% empty %} 子句，用于定义列表为空时显示的内容。

```python
{% for athlete in athlete_list %}
    <p>{{ athlete.name }}</p>
{% empty %}
    <p>There are no athletes. Only computer programmers.</p>
{% endfor %}
```



在 {% for %} 循环内部，可以访问一个名为 forloop 的模板变量。

- forloop.counter 的值是一个整数，表示循环的次数。这个属性的值从 1 开始，因此第一次循环时,forloop.counter 等于 1 。
- forloop.counter0 与 forloop.counter 类似，不过是从零开始的。第一次循环时，其值为 0 。
- forloop.revcounter 的值是一个整数，表示循环中剩余的元素数量。第一次循环时， for-loop.revcounter 的值是序列中要遍历的元素总数。最后一次循环时， forloop.revcounter 的值为 1。
- forloop.revcounter0 与 forloop.revcounter 类似，不过索引是基于零的。第一次循环时， for-
  loop.revcounter0 的值是序列中元素数量减去一。最后一次循环时， forloop.revcounter0 的值为 0。
- forloop.first 是个布尔值，第一次循环时为 True 。
- forloop.last 是个布尔值，最后一次循环时为 True 。
- 在嵌套的循环中， forloop.parentloop 引用父级循环的 forloop 对象。



#### **ifequal/ifnotequal**



Django 模板系统不是功能全面的编程语言，不允许执行任意的 Python 语句。

模板经常需要比较两个值，在相等时显示一些内容。为此，Django 提供了 {% ifequal %} 标签。

{% ifequal %} 标签比较两个值，如果相等，显示 {% ifequal %} 和 {% endifequal %} 之间的内容。

{% ifnotequal %} 的作用与 ifequal 类似，不过它测试两个参数是否不相等。

 ifnotequal 标签可以替换成 if标签和 != 运算符。



### 3.4.2 过滤器



模板过滤器是在显示变量之前调整变量值的简单方式。过滤器使用管道符号指定，管道后面的为给过滤器的传参。

过滤器可以串接，即把一个过滤器的输出传给下一个过滤器。

```python
{{ my_list|first|upper }}
```



有些过滤器可以接受参数。过滤器的参数放在冒号之后，始终放在双引号内。

```python
{{ bio|truncatewords:"30" }}
```



几个重要的过滤器

- addslashes ：在反斜线、单引号和双引号前面添加一个反斜线。可用于转义字符串。
  - 例如： {{ val-ue|addslashes }} 。
- date ：根据参数中的格式字符串格式化 date 或 datetime 对象。
  - 例如： {{ pub_date|date:"F j, Y"}} 。
- length ：返回值的长度。对列表来说，返回元素的数量。对字符串来说，返回字符的数量。如果变量
  未定义，返回 0 。





## 3.5 理念和局限



Django 的核心：

1. 表现与逻辑分离
2. 避免重复
3. 与 HTML 解耦
4. XML 不好
5. 不要求具备设计能力
6. 透明处理空格
7. 不重造一门编程语言
8. 确保安全有保障
9. 可扩展



## 3.6 在视图中使用模板







## 3.7 模板加载机制



首先要告诉框架模板的存储位置。



```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
        	# ... 一些选项 ...
        },
    },
]
```

BACKEND 的值是一个点分 Python 路径，指向实现 Django 模板后端 API 的模板引擎类。

多数引擎从文件中加载模板，所以各个引擎的顶层配置包含三个通用的设置：

- DIRS 定义一个目录列表，模板引擎按顺序在里面查找模板源文件。
-  APP_DIRS 设定是否在安装的应用中查找模板。按约定， APPS_DIRS 设为 True 时， DjangoTemplates 会在INSTALLED_APPS 中的各个应用里查找名为“templates”的子目录。这样，即使 DIRS 为空，模板引擎还能查找应用模板。
- OPTIONS 是一些针对后端的设置。





### 3.7.1 模板目录





DIRS 的默认值是一个空列表，Django会根据里面的值寻找模板。

```python
'DIRS': [
    '/home/html/example.com',
    '/home/html/default',
],
```



注意

 1. 如果没有创建app，DIRS最好留空；

 2. 如果真根目录放了公共的模板，则

    'DIRS': [os.path.join(BASE_DIR, 'templates')],

	3. 模板目录名称templates不是必须的

	4. 注意系统间的路径分隔符

    Windows：/

    Linux：\



Django选择模板，是先从根目录开始找起，根目录找不到，再按app注册的顺序，到app中查找。



## 3.8 render()



```python
from django.shortcuts import render
import datetime

def current_datetime(request):
    now = datetime.datetime.now()
    return render(request, 'current_datetime.html', {'current_date': now})
```

render() 的第一个参数是请求对象，第二个参数是模板名称，第三个单数可选，是一个字段，用于创建给模板的上下文。

如果不指定第三个参数， render() 使用一个空字典。



## 3.9 模板子目录



如果把所有模板存放在一个目录中，很快就会变得不灵便。

如果是app，则一般都会在app的templates文件下创建一个跟app名字相同的文件夹，把模板文件都放到该文件夹下，避免模板间的冲突。

```python
return render(request, 'dateapp/current_datetime.html', {'current_date': now})
```



## 3.10 include 模板标签



内置模板标签了： {% include %} 。

这个标签的作用是引入另一个模板的内容。

它的参数是要引入的模板的名称，可以是变量，也可以是硬编码的字符串（放在引号里，单双引号都行）。

只要想在多个模板中使用相同的代码，就可以考虑使用 {% include %} ，去除重复。

用法：{% include template_name %}

```Python
{% include 'includes/nav.html' %}
```





## 3.11 模板继承



模板一般都会存在大量的重复的代码，所以模板的继承是很必要的。

用法：

1. 创建一个基模板，并在对应的地方挖空，提供给子模板填内容。

```python
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN">
<html lang="en">
    <head>
        <title>{% block title %}{% endblock %}</title>
    </head>
    <body>
        <h1>My helpful timestamp site</h1>
        {% block content %}{% endblock %}
        {% block footer %}
            <hr>
            <p>Thanks for visiting my site.</p>
        {% endblock %}
    {% block js %}{% endblock %}
    </body>
</html>
```

2. 子模板继承父模板，并在对应的空填内容

```python
{% extends "base.html" %}

{% block title %}The current time{% endblock %}
{% block content %}
	<p>It is now {{ current_date }}.</p>
{% endblock %}
```



注意：

	1.  {% extends %}必须放在最顶部
 	2.  基模板中的 {% block %} 标签越多越好。子模板无需定义父模板中的全部块
 	3.  当出现过多的重复的代码时，就应该考虑在基模板挖空了
 	4.   {% block %}的名称不能同名





# 4 Django模型









































# 5 Django管理后台



## 5.1 创建超级用户



```python
python manage.py createsuperuser
```



## 5.2 管理后台运行原理



```
1. 启动服务器时，Django 运行 admin.autodiscover() 函数。要在 urls.py 文件中调用这个函数，但是现在版本的 Django 会自动运行它。

2. autodiscover()函数迭代 INSTALLED_APPS 设置，在安装的各个应用中查找一个名为 admin.py 的文件。

3. 如果应用中存在这个文件，就执行里面的代码。调用 admin.site.register() ，在管理后台中注册各个模型，就可以在后台显示了。

4. 后台管理在 django/contrib/admin 里的代码，查看它的模板、视图和 URL 配置。

5. 管理后台的处理相当复杂，理解起来挺费劲。
```



## 5.3 把模型添加到 Django 管理后台中



```python
from django.contrib import admin
from app.models import Book

admin.site.register(Book)
```





# 6 Django表单





## 6.1 关于URL的信息



视图函数的第一个参数必须为HttpRequest 。

```python
def index(request):
	return HttpResponse("Welcome to the page at %s" % request.path)
```



 HttpRequest 对象的方法和属性

| 属性/方法               | 说明                                    | 示例                                 |
| ----------------------- | --------------------------------------- | ------------------------------------ |
| request.path            | 完整的路径，不含域名，但是包含前导斜线  | /hello/                              |
| request.get_host()      | 主机名                                  | “127.0.0.1:8000”或“www.exam-ple.com” |
| request.get_full_path() | 包含查询字符串（如果有的话）的路径      | /hello/?print=true                   |
| request.is_secure()     | 通过 HTTPS 访问时为 True ，否则为 False | True 或 False                        |





## 6.2 表单验证



表单类的字段参数敢数据模型很像。



```python
from django import forms

class ContactForm(forms.Form):
    """Form类"""
    
    subject = forms.CharField(max_length=10)
    email = forms.EmailField(required=False, label='Your e-mail address')
    message = forms.CharField()
    
    def clean_message(self):
        """自定义验证字段钩子函数"""
        
        message = self.cleaned_data['message']
        num_words = len(message.split())
        if num_words < 4:
        	raise forms.ValidationError("Not enough words!")
        return message


def contact(request):
    """视图函数"""
    
    if request.method == 'POST':
    	form = ContactForm(request.POST)
        
    if form.is_valid():
        cd = form.cleaned_data
        send_mail(
            cd['subject'],
            cd['message'],
            cd.get('email', 'noreply@example.com'),['siteowner@example.com'],
        )
        return HttpResponseRedirect('/contact/thanks/')
    else:
        form = ContactForm()
    return render(request, 'contact_form.html', {'form': form})

```





# 7 高级视图和URL配置





## 7.1 简化导入



- 分开导入模型

```python
from django.conf.urls import include, url
from django.contrib import admin
from mysite.views import hello, current_datetime, hours_ahead


urlpatterns = [
    url(r'^admin/', include(admin.site.urls)),
    url(r'^hello/$', hello),
    url(r'^time/$', current_datetime),
    url(r'^time/plus/(\d{1,2})/$', hours_ahead),
]

```



- 导入整个模块（推荐）

```python
from django.conf.urls import include, url
from app import views

urlpatterns = [
    url(r'^hello/$', views.hello),
    url(r'^time/$', views.current_datetime),
    url(r'^time/plus/(d{1,2})/$', views.hours_ahead),
]
```



## 7.2 具名分组





- 普通URL

```python
from django.conf.urls import url
from app import views

urlpatterns = [
    url(r'^reviews/2003/$', views.special_case_2003),
    url(r'^reviews/([0-9]{4})/$', views.year_archive),
    url(r'^reviews/([0-9]{4})/([0-9]{2})/$', views.month_archive),
    url(r'^reviews/([0-9]{4})/([0-9]{2})/([0-9]+)/$', views.review_detail),
]
```



- 具名URL

```python
from django.conf.urls import url
from app import views

urlpatterns = [
    url(r'^reviews/$', views.page),
    url(r'^reviews/page(?P<num>[0-9]+)/$', views.page),
]


# 则视图函数
def index(request, num):
    """如果num不写，会报错"""
    pass

# 也可以设置默认值
def index(request, num=1):
    pass
```





## 7.3 引入其他 URL 配置



因为一个项目包含多个app，所以需要引入其他的URL。

```python
from django.conf.urls import include, url

urlpatterns = [
    url(r'^community/', include('app.urls')),
    url(r'^contact/', include('app.urls')),
]

```



注意，这里的正则表达式没有 $ （匹配字符串末尾的符号），但是末尾有斜线。

Django 遇到 include() 时，会把截至那一位置匹配的 URL 截断，把余下的字符串传给引入它的 URL 配置，做进一步处理。



此外，还可以使用 url() 引入额外的 URL 模式。

可以去除 URL 配置中的重复，在多处使用相同的模式前缀。

```python
from django.conf.urls import include, url
from apps.main import views as main_views
from credit import views as credit_views


extra_patterns = [
    url(r'^reports/(?P<id>[0-9]+)/$', credit_views.report),
    url(r'^charge/$', credit_views.charge),
]

urlpatterns = [
    url(r'^$', main_views.homepage),
    url(r'^help/', include('apps.help.urls')),
    # 扩展url
    url(r'^credit/', include(extra_patterns)),
]
```





引入app的urls.py，一般这样就够了。

```python
urlpatterns = [
    url(r'^app/', include(('app.urls', 'app'), namespace='app')),
]

说明：
	1. include(('app的urls.py', '应用名称'), namespace='命名空间名')
```







## 7.4  反向解析 URL



根的urls.py

```python
urlpatterns = [
    url(r'^user/', include(('user.urls', 'user'), namespace='user')),
] 
```



app的urls.py

```python
urlpatterns = [
    url(r'^login/$', view.login, name='login'),
]
```



**则使用url方向解析时**

```python
# 在视图函数
reverse('user:login')
# 传递参数可以
reverse('user:login', kwargs={'user_id':10, 'page': 2, ...})

# 在模板
{% url 'user:login' %}
# 传递参数可以
{% url 'user:login' urser_id=1 page=2 ... %}

说明：
	1. reverse('app命名空间:url命名')
    2. {% url 'app命名空间:url命名' %}
    3. 要是有多级命名空间就
    	'name1:name2:...:url_name'
        接下去就行了，但是一般的都不会太长。
```

















# 8 高级模板技术



现在前后端分离是趋势，要是你决心搞的就是前后分离，那就可以不用深入学Django模板这部分了。

不过现在一些公司内部还是使用不分离的，图的就是快速开发。

不过在学习的角度，Django的模板机制设计得很优秀的，值得学习。





模板中有模板标签和变量。

- 模板标签使用{% tag_name %}
- 模板变量使用{{ arg_name }}



## 8.1 RequestContext 和上下文处理器



模板要在上下文中渲染。

上下文是 django.template.Context 的实例，不过 Django 还提供一个子类， django.template.RequestContext ，其行为稍有不同。

RequestContext 默认为模板上下文添加很多变量。

使用 render() 快捷方式时，如果没有明确传入其他上下文，默认使用 RequestContext。



 context_processors 设置（在 settings.py 文件中）指明始终提供给 RequestContext 的上下文处理器。

```python
context_processors 的默认值如下：

'context_processors': [
    'django.template.context_processors.debug',
    'django.template.context_processors.request',
    'django.contrib.auth.context_processors.auth',
    'django.contrib.messages.context_processors.messages',
],
```



### 8.1.1 auth



django.contrib.auth.context_processors.auth

启用这个处理器后， RequestContext 中将包含下述变量：

- user ： auth.User 的实例，表示当前登录的用户（如未登录，是 AnonymousUser 实例）。
- perms ： django.contrib.auth.context_processors.PermWrapper 实例，表示当前登录用户拥有的权限。





## 8.2 模板加载内部机制



通常把模板保存在文件系统中的文件里，而不直接使用低层的 Template API。



- DIRS 选项

  在settings.py里面有如下配置

```python
TEMPLATES = [
{
    'BACKEND': 'django.template.backends.django.DjangoTemplates',
    # 这样配置，正常的就是在根目录下的templates下找模板文件，找不到就去app下面找
    'DIRS':  [os.path.join(BASE_DIR, 'templates')],
    },
]

说明：
	1. app模板的查找顺序跟app注册的顺序一至
    2. 如果前面的app有你现在这个app要用的模板，则会先用前面的，而不是本app的
    3. 所以，一遍真app的templates下再创建一个跟app同名的文件夹，把app所有的模板文件都放里面
```



理论上，模板文件可以放在任何地方，服务器找得到进行，模板文件的后缀不一定要.html，可以任意，不写也行。

但是，开发还是得根据规范来，模板都放一个文件夹，后缀都用.html。

此外还要注意：

​	Unix类系统的路径分隔符是 /

​	win系统的是\



较完整的配置长这样的：

```python
TEMPLATES = [
    {
        # 使用的模板系统
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        # 模板文件的文件夹，是列表，所以可以有多个地方
        'DIRS': [os.path.join(BASE_DIR, 'templates'), ],
        # 配置这个，如果DIRS找不到模板，就根据顺序到app下找，再找不到就报错
        'APP_DIRS': True,
        # 参数选项配置
        'OPTIONS': {
            # 上下文管理器配置，使用那些处理器在这里配置，这样模板就有了相关的对象
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



## 8.3 扩展模板系统



对模板系统的定制，基本上是自定义模板标签和（或）过滤器。





### 8.3.1 自定义模板系统的目录结构



自定义的模板标签和过滤器必须放在一个 Django 应用中。

如果与现有应用有关，可以放在现有应用中。

否则，应该专门创建一个应用存放。

应用中应该有个 templatetags 目录，与 models.py 、 views.py 等文件放在同一级，如果没有这个目录，创建一个。



添加这个模块之后，要重启服务器方能在模板中使用自定义的标签或过滤器。

自定义的标签和过滤器在 templatetags 目录里的一个模块中。

模块文件的名称是**加载标签**所用的名称，别与其他应用中的自定义标签和过滤器冲突了。



假如自定义的标签（过滤器）放在 review_extras.py 文件中，应用的布局可能是下面这样：

```python
# reviews是一个app
reviews/
    __init__.py
    models.py
    templatetags/
    	__init__.py
    	review_extras.py
	views.py
```

在模板中使用

```python
{% load review_extras %}
```



注意：

​	1. 包含自定义标签的应用必须在 INSTALLED_APPS 中列出，这样 {% load %} 标签才能起作用。

	2. 再多的例子也不如阅读源码，学习 Django 是如何定义默认的过滤器和标签的。
 	3. 过滤器和标签分别在 django/template/defaultfilters.py 和 django/template/defaulttags.py 文件中。



### 8.3.2 创建模板库



现在版本的Django创建app时已经帮我们创建好了。



不管是自定义标签还是过滤器，第一件事都是创建模板库——这是让 Django 勾住的基本要求。

创建模板库分为两步：

- 把模板库放在哪个app中，并把模板库所在的app加到INSTALLED_APPS中。
- 在 Django 应用中合适的包里创建 templatetags **包**，与 models.py 、 views.py。



标签和过滤器都通过这种方式注册，在模块顶部要插入代码：

```python
from django import template

register = template.Library()
```



### 8.3.3 自定义模板标签和过滤器



使用 Python 自定义标签和过滤器，然后使用 {% load %} 标签加载，让自定义的标签和过滤器可在模板中使用。



```python
# 传给 foo 过滤器的变量是 var ，参数是 "bar" 。
{{ var|foo:"bar" }}
```

模板语言没有提供异常处理功能，所以模板过滤器抛出的异常会以服务器错误体现出来。



#### 自定义模板过滤器

过滤器本质是一个函数，有函数所用的特性。

```python
# 定义一个过滤器
def cut(value, arg):
	return value.replace(arg, '')

def lower(value):
	return value.lower()

# 注册过滤器
from django import template
register = template.Library()
register.filter('cut', cut)
register.filter('lower', lower)

# 在模板中使用
{{ var|cut:"0" }}
{{ var|lower }}

```

另一种注册方式是装饰器的形式

```python
@register.filter(name='cut')
def cut(value, arg):
	return value.replace(arg, '')
```



#### 自定义模板标签



##### 简单标签：simple_tag



很多模板标签接受几个参数（字符串或模板变量），对输入参数和一些外部信息做些处理之后返回一个结果。



例子：

```python
import datetime
from django import template

register = template.Library()

@register.simple_tag
def current_time(format_string):
	return datetime.datetime.now().strftime(format_string)
```

如果模板标签需要访问当前上下文，注册标签时指定 takes_context 参数：

```python
# 第一个参数的名称必须是 context
@register.simple_tag(takes_context=True)
def current_time(context, format_string):
    
	timezone = context['timezone']
	return your_get_current_time_method(timezone, format_string)
```



使用 simple_tag 装饰的函数可以接受任意个位置参数和关键字参数。

```python
# 标签的定义
@register.simple_tag
def my_tag(name, age, *args, **kwargs):
    
    user = User.objects.filter(name=name).first()
    if user.age == int(age):
        return True
    return False


# 模板中使用
{% my_tag 'ydc' 20 user.address phone='110' %}

说明：
	1. 以空格分开给标签传递参数
    2. 关键字参数要写在最后面，跟Python函数一样的
```



##### 引入标签：inclusion_tag



模板标签另一种常见的用法是用于引入另一个模板。



例子：编写一个标签，生成指定Author 对象名下的图书列表。

```html
{% books_for_author author %}

得到下面

<ul>
    <li>The Cat In The Hat</li>
    <li>Hop On Pop</li>
    <li>Green Eggs And Ham</li>
</ul>
```

```python
# 定义函数.py
def books_for_author(author):
    books = Book.objects.filter(authors__id=author.id)
    return {'books': books}

# 定义模板book_snippet.html
<ul>
    {% for book in books %}
    	<li>{{ book.title }}</li>
    {% endfor %}
</ul>

@register.inclusion_tag('book_snippet.html')
def show_reviews(review):
    pass

# 在模板中使用
{% books_for_author author %}
```























# 9 Django 模型的高级用法


























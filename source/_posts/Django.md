---
title: Django
date: 2019-07-23 15:21:52
tags: python
---
# Django 框架

>> **本文基于Linux环境下操作**

## MVT框架
* M：Model **模型**&emsp;&emsp;作用：和后台数据库进行交互。
* V：View：**视图**&emsp;&emsp;&nbsp;作用：接受浏览器请求，进行处理并与模型和模板进行交互，返回应答。
* T: Template **模板**&nbsp;&nbsp;&nbsp;&emsp;作用：产生html页面。
<!--more-->
---

## 环境配置
1. **安装虚拟环境**&nbsp;(仅仅只是Python环境的复制版本，并不是另一套系统)
```
sudo pip install virtualenv  # 利用pip下载virtualenv包
```
2. **安装虚拟环境扩展包**
```
sudo pip install virtualenvwrapper
```
3. **编辑家目录下面的.bashrc文件，添加下面两行。**
```
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
```
4. **使用下面的命令使虚拟环境生效**
```
source .bashrc
```
5. **创建虚拟环境**
```
mkvirtualenv 虚拟环境名  # 此时为Python2的解释器创建
mkvirtualenv -p python3 bj11_py3
workon 虚拟环境名  # 进入虚拟环境工作
workon 空格 + 两个tab键  # 查看主机上所有的虚拟环境
deactivate  # 退出虚拟环境
rmvirtualenv 虚拟环境名  #删除虚拟环境
```
6. **安装Django环境**
```
pip install django==1.8.2  # 注意：不能使用sudo安装，sudo会安装到真是的主机环境上，而不是虚拟环境中
pip list  # 可以查看虚拟环境中安装了哪些python包
```
---

## 项目创建
```
django-admin startproject 项目名称
```
***注意：创建应用必须先进入虚拟环境。***
![1](./Django学习图片-1.jpg)

#### 各个文件的说明
* _ _init_ _.py:说明该项目是一个Python包
* settings.py:项目配置文件
* urls.py:进行url路由的配置
* wsgi.py:web服务器和Django交互的入口
* manage.py:项目的管理文件

*注：一个项目由多个应用组成，每个应用完成一个特定的功能，在Django中，每个模块使用一个Django应用来开发*

**因此，接下来将了解如何创建一个应用**

## 应用创建
```
python manage.py startapp 应用名称   # 创建应用时需先进入到项目目录
```

![1](./Django学习图片2.jpg)


#### 各个文件的说明
* _ _init__.py:说明该目录是一个Python模块
* models.py:用来写和数据库交互的功能
* views.py:定义处理函数，称为视图函数
* tests.py:用来写测试代码的文件
* admin.py:网站后台管理的相关文件

**在建立了项目与应用之后，还需要对应用进行注册，才能使该项目和该应用关联**

#### 方法：修改settings.py中的INSTALLED_APPS配置项。

![1](./Django学习图片3.jpg)

### 运行Django框架命令(在项目目录下进行)
```
python manage.py runserver
```

***完成了上述动作之后就已经构建起了基本的Django的框架，接下来就可以向此框架中添加配置了！***

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![1](./斗图1.jpg)

---

## ORM模型(对象——关系映射)

**Django中内嵌了ORM框架，ORM的作用是将类和数据表进行关联，即只需要对类和对象进行操作就可以对数据库进行更改，并且ORM可以根据设计的类自动生成表。**

**注意：在应用下的model.py文件中设计模型类时，该类必须继承models.Model类**
![1](./Django学习图片4.png)

如上图所示，bookinfo类继承了models.Model类，并且可以看出bookinfo有两种属性，分别是bittle和bpub_date,代表了图书的名称和出版日期。

## 模型类生成表

**刚刚说到ORM可以根据模型类自动生成表，那么要如何操作呢，接下来就来看看.**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![1](./斗图2.jpg)

* 生成迁移文件
```
python manage.py makemigrations
```

![1](./Django学习图片5.jpg.png)

* 执行迁移文件生成表
```
python manage.py migrate
```
**生成表的默认格式：应用名_模型类名(小写)**
***注意：先在生成的数据库是sqlite3数据库，如果你想看到图形化的数据库界面，可以安装sqliteman，使用如下命令***
```
sudo apt-get install sqliteman
sqliteman     # 即可启动sqliteman查看图形化的sqlite数据库
```
#### pip和apt-get的区别
这里简单介绍一下二者的区别，apt-get一般用于安装软件，而pip是Python管理包的工具，通常用于进行Python包和模块的安装

---


## 通过模型类操作数据表

**进入Python shell的命令**
```
python manage.py shell
```

*应当先导入刚才在应用中建立的models类*
```
from 应用名.models import 类名
```

**以下为在相互shell终端中演示的例子：**
```
1) 向booktest_bookinfo表中插入一条数据。
    b = BookInfo() #定义一个BookInfo类的对象
    b.btitle ='天龙八部' #定义b对象的属性并赋值
    b.bpub_date = date(1990,10,11) 
    b.save() #才会将数据保存进数据库

2）查询出booktest_bookinfo表中id为1的数据。
    b = BookInfo.objects.get(id=1)
     
3）在上一步的基础上改变b对应图书的出版日期。
    b.bpub_date = date(1989,10,21)
    b.save() #才会更新表格中的数据

4）紧接上一步，删除b对应的图书的数据。
    b.delete() #才会删除

5）向booktest_heroInfo表中插入一条数据。
    h = HeroInfo()
    h.hname = '郭靖'
    h.hgender = False
    h.hcomment = ‘降龙十八掌’
    b2 = BookInfo.objects.get(id=2)
    h.hbook = b2  #给关系属性赋值，英雄对象所属的图书对象
    h.save() 

6）查询图书表里面的所有内容。
    BookInfo.objects.all()
    HeroInfo.objects.all()
    
7）查询出id为2的图书中所有英雄人物的信息。
    b = BookInfo.objects.get(id=2)
    b.heroinfo_set.all() #查询出b图书中所有英雄人物的信息

```
**多类之间的关系**
![1](./Django学习图片6.jpg)

---

## 后台管理
1. 本地化

进行语言和时区的本地化

修改settings.py文件。
![1](./Django学习图片7.png)

2. 创建管理员
```
python manage.py createsuperuser
```
3. 注册模型类

在应用下的admin.py中注册模型类。
告诉djang框架根据注册的模型类来生成对应表管理页面。
![1](./Django学习图片8.png)

---

## 视图

**在Django中，通过浏览器去请求一个页面时，使用视图函数来处理这个请求的，视图函数处理之后，要给浏览器返回页面内容。**

### 视图函数的使用

* 定义视图函数

**视图函数定义在views.py中。**
例：
```
def index(request):
    #  进行处理
    return HttpResponse('hello python')
```
视图函数必须有一个参数request，进行处理之后，需要返回一个HttpResponse的类对象，hello python就是返回给浏览器显示的内容。

* 进行URL配置

url配置的目的是让建立url和视图函数的对应关系。

url配置项定义在urlpatterns的列表中，每一个配置项都调用url函数。

url函数有两个参数，第一个参数是一个正则表达式，第二个是对应的处理动作。

配置url时，有两种语法格式：
1. url(正则表达式，视图函数名)
2. url(正则表达式，include(应用中的urls文件))

**实际在配置url时，首先在项目的urls.py文件中添加配置项时，并不写具体的url和视图函数之间的对应关系，而是包含具体应用的urls.py文件，在应用的urls.py文件中写url和视图函数的对应关系。**

URL配置方式
![1](Django学习图片9.png)

### URL的匹配过程

**在项目的urls.py文件中包含具体应用的urls.py文件，应用的urls.py文件中写url和视图函数的对应关系。**

### 匹配方式和运行机制

![1](Django学习图片10.png)

当用户输入如http://127.0.0.1:8000/aindex时， **去除域名和最前面的/，剩下aindex，拿aindex字符串到项目的urls文件中进行匹配，配置成功之后，去除匹配的a字符，那剩下的index字符串继续到项目的urls文件中进行正则匹配，匹配成功之后执行视图函数index** ，index视图函数返回内容hello python给浏览器来显示。

---

## 模板

*不仅仅是一个HTML文件*

### 模板文件的使用

1. 创建模板文件夹(templates文件夹)
2. 配置模板目录

**在项目文件中修改settings文件，将其TEMPLATES下的'DIRS'改为[os.path.join(BASE_DIR, 'templates')]**

![1](./Django学习图片11.png)

其中BASE_DIR是项目的绝对路径

3. 使用模板文件

**在templates文件夹下建立与应用同名的文件夹，防止混淆**

* 加载模板文件

在模板目录下面获取HTML文件的内容，得到一个模板对象
```
from django.templates import loader
temp = loader.get_template('相对于templates文件的相对路径')
```

* 定义模板上下文
```
from django.templates import RequestContext
context = RequestContext(request, {一个字典，替代模板中变量的值})
```

向模板文件中传递数据

* 模板渲染

得到一个标准的HTML文件
```
res_html = temp.render(context)    # 渲染完成的HTML内容
return HttpResponse(res_html)
```

**以上三点在Django内部已经进行了封装，可直接调用render函数返回**
```
return render(request, '相对于templates文件的相对路径', {一个字典，替代模板中变量的值})
```

### 在模板文件中定义变量
1. 模板变量的使用 ： {{模板变量名}}
2. for循环的使用 ： 
{% raw %}{% for i in list %}{% endraw %}

{% raw %}{% empty %}{% endraw %}   # 当for循环中没有任何变量时显示的数据

{% raw %}{% endfor %}{% endraw %}

---

## 模型

### Django ORM
![1](./Django学习图片12.png)

**O：(objects)->类和对象。**<br>
**R：(Relation)->关系，关系数据库中的表格。**<br>
**M：（Mapping）->映射。**

### Django ORM 框架的功能

1. 建立模型类和表之间的对应关系，允许我们通过面向对象的方式操作数据库
2. 根据设计的模型类生成数据库的表格
3. 通过方便的配置就可以进行数据库的切换

*在此之前使用的数据库都是Django默认的sqlite数据库，接下来我们将对Django配置进行修改，改为熟悉的MySQL数据库*
### MySQL数据库配置

**修改settings.py中的DATABASES。**
![1](./Django学习图片13.png)
***注意：django框架不会自动帮我们生成mysql数据库，所以我们需要自己去创建。***

**切换mysql数据库之后不能启动服务器：**<br>
安装mysqlPython包: <br>
python2：
  pip install mysql-python<br>
python3:
安装pymysql:
  pip install pymysql<br>
在test1/__init__.py中加如下内容：<br>
```
import pymysql
pymysql.install_as_MySQLdb()
```
进行如上操作之后便完成了MySQL数据库的配置！

![1](./斗图3.jpg)

### 重定向
最后在讲解一下重定向，当浏览器点击某一功能时，仍需要返回原网址。则需使用重定向。

代码如下：
```
return HttpResponseRedirect('/当前网址')

from django.shortcuts import redirct
return redirect('/当前网址')    # 使用Django中封装好的重定向函数redirect返回
```

---

## 字段属性和选项

**属性命名限制：**
1. 不能是python的保留关键字。
2. 不允许使用连续的下划线，这是由django的查询方式决定的。
3. 定义属性时需要指定字段类型，通过字段类型的参数指定选项，语法如下：
属性名=models.字段类型(选项)

### 字段类型
**使用时需要引入django.db.models包**，字段类型如下：<br>
**AutoField**：自动增长的IntegerField，通常不用指定，不指定时Django会自动创建属性名为id的自动增长属性。<br>
**BooleanField**：布尔字段，值为True或False。<br>
**NullBooleanField**：支持Null、True、False三种值。<br>
**CharField(max_length=字符长度)**：字符串。
参数max_length表示最大字符个数。<br>
**TextField**：大文本字段，一般超过4000个字符时使用。<br>
**IntegerField**：整数。<br>
**DecimalField(max_digits=None, decimal_places=None)**：十进制浮点数。
参数max_digits表示总位数。
参数decimal_places表示小数位数。<br>
**FloatField**：浮点数。<br>
**DateField([auto_now=False, auto_now_add=False])**：日期。<br>
*参数auto_now表示每次保存对象时，自动设置该字段为当前时间，用于"最后一次修改"的时间戳，它总是使用当前日期，默认为false。*<br>
*参数auto_now_add表示当对象第一次被创建时自动设置当前时间，用于创建的时间戳，它总是使用当前日期，默认为false。*<br>
**参数auto_now_add和auto_now是相互排斥的，组合将会发生错误。**<br>
**imeField**：时间，参数同DateField。<br>
**DateTimeField**：日期时间，参数同DateField。<br>
**FileField**：上传文件字段。<br>
**ImageField**：继承于FileField，对上传的内容进行校验，确保是有效的图片。<br>

### 选项

**通过选项实现对字段的约束，选项如下**：<br>
**null**：如果为True，表示允许为空，默认值是False。<br>
**blank**：如果为True，则该字段允许为空白，默认值是False。<br>
***对比：null是数据库范畴的概念，blank是表单验证证范畴的。***<br>
**db_column**：字段的名称，如果未指定，则使用属性的名称。<br>
**db_index**：若值为True, 则在表中会为此字段创建索引，默认值是False。<br>
**default**：默认值。<br>
**primary_key**：若为True，则该字段会成为模型的主键字段，默认值是False，一般作为AutoField的选项使用。<br>
**unique**：如果为True, 这个字段在表中必须有唯一值，默认值是False。<br>

---

## 查询

### 查看MySQL的日志文件

1. sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf **68 69行 注释取消**
2. sudo service mysql restart 重启mysql服务
3. /var/log/mysql/mysql.log #mysql操作的记录文件。
4. sudo tail -f /var/log/mysql/mysql.log #实时查看mysql文件的内容。

### 查询MySQL数据库中数据的方法

**get()**:返回表中满足条件的一条且只能有一条数据。<br>
如果查到多条数据，则抛异常：MultipleObjectsReturned<br>
查询不到数据，则抛异常：
DoesNotExist<br>

**all()**:返回模型类对应表格中的所有数据。QuerySet类型，查询集<br>

**filter()**:参数写查询条件，返回满足条件的数据。QuerySet<br>
*条件格式：模型类属性名__条件名=值*

### 查询条件

1.  判等 exact。<br>
例：查询编号为1的图书。<br>
BookInfo.objects.get(id=1)<br>
BookInfo.objects.get(id__exact=1)
2.  模糊查询 contains<br>
例：查询书名包含'传'的图书。<br>
BookInfo.objects.filter(btitle__contains='传')<br>
例：查询书名以'部'结尾的图书 endswith 开头:startswith<br>
BookInfo.objects.filter(btitle__endswith='部')
3.  空查询 isnull *(select * from booktest_bookinfo where title is not null)*<br>
例：查询书名不为空的图书。<br>
BookInfo.objects.filte(btitle__isnull=False)
4.  范围查询 in *(select * from booktest_bookinfo where id in (1,3,5))*<br>
例：查询编号为1或3或5的图书。<br>
BookInfo.objects.filter(id__in = [1,3,5])
5.  比较查询 <br>
例：查询编号大于3的图书。gt(大于) lt(小于） gte(大于等于) lte(小于等于)<br>
BookInfo.objects.filter(id__gt = 3)

6.  日期查询<br>
例：查询1980年发表的图书。<br>
BookInfo.objects.filter(bpub_date__year=1980)<br>
例：查询1980年1月1日后发表的图书。<br>
from datetime import date
BookInfo.objects.filter(bpub_date__gt = date(1980,1,1))<br>
7. exclude:返回不满足条件的数据。QuerySet<br>
例：查询id不为3的图书信息。<br>
BookInfo.objects.exclude(id=3)<br>

### F对象
**作用：用于类属性之间的比较条件。**<br>
使用之前需要先导入：<br>
**from django.db.models import F**<br>
例：查询图书阅读量大于评论量图书信息。<br>
BookInfo.objects.filter(bread__gt = F('bcomment'))<br>
例：查询图书阅读量大于2倍评论量图书信息。<br>
BookInfo.objects.filter(bread__gt = F('bcomment')*2)


### Q对象
**作用：用于查询时的逻辑条件。not and or，可以对Q对象进行& | ~操作。**
使用之前需要先导入：<br>
**from django.db.models import Q**<br>
例：查询id大于3且阅读量大于30的图书的信息。<br>
BookInfo.objects.filter(id__gt=3, bread__gt=30)<br>
BookInfo.objects.filter(Q(id__gt=3)&Q(bread__gt=30))<br>
例：查询id大于3或者阅读量大于30的图书的信息。<br>
BookInfo.objects.filter(Q(id__gt=3)|Q(bread__gt=30))<br>
例：查询id不等于3图书的信息。<br>
BookInfo.objects.filter(~Q(id=3))<br>
### 排序 **order_by** QuerySet
**作用：进行查询结果进行排序。**<br>
例：查询所有图书的信息，按照id从小到大进行排序。<br>
BookInfo.objects.all().order_by('id')<br>
BookInfo.objects.order_by('id')<br>
例：查询所有图书的信息，按照id从大到小进行排序。<br>
BookInfo.objects.all().order_by(**'-id'**)<br>
例：把id大于3的图书信息按阅读量从大到小排序显示；<br>
BookInfo.objects.filter(id__gt=3).order_by('-bread')

### 聚合函数
**作用：对查询结果进行聚合操作。**<br>
**sum count max min avg**<br>
**aggregate**：调用这个函数来使用聚合。 返回值是一个字典<br>
使用前需先导入聚合类： <br>
**from django.db.models import Sum,Count,Max,Min,Avg**<br>
例：查询所有图书的数目。Count<br>
BookInfo.objects.aggregate(Count('id'))<br>
返回值类型:
{'id__count': 5}<br>
例：查询所有图书阅读量的总和。<br>
BookInfo.objects.aggregate(Sum('bread'))<br>
返回值类型：
{'bread__sum': 126}<br>
**count函数 返回值是一个数字**<br>
**作用：统计满足条件数据的数目。**<br>
例：统计所有图书的数目。<br>
BookInfo.objects.count()<br>
例：统计id大于3的所有图书的数目。<br>
BookInfo.objects.filter(id__gt=3).count()<br>


**查询相关函数返回值总结：**<br>
**get**:返回一个对象 <br>
**all**:QuerySet 返回所有数据<br>
**filter**:QuerySet 返回满足条件的数据<br>
**exclude**:QuerySet 返回不满条件的数据 <br>
**order_by**:QuerySet 对查询结果进行排序<br>
**aggregate**:字典 进行聚合操作<br>
**count**:数字 返回查询集中数据的数目<br>
get,filter,exclude参数中可以写查询条件。<br>

### 查询集
**all, filter, exclude, order_by调用这些函数会产生一个查询集，可以在查询集上继续调用这些函数。**
查询集特性：
1. 惰性查询：只有在实际使用查询集中的数据的时候才会发生对数据库的真正查询。
2. 缓存：当使用的是同一个查询集时，第一次的时候会发生实际数据库的查询，然后把结果缓存起来，之后再使用这个查询集时，使用的是缓存中的结果。
限制查询集：<br>
**可以对一个查询集进行取下标或者切片操作来限制查询集的结果。**<br>
b[0]就是取出查询集的第一条数据，b[0:1].get()也可取出查询集的第一条数据。如果b[0]不存在，会抛出IndexError异常，如果b[0:1].get()不存在，会抛出DoesNotExist异常。多条时抛MultiObjectsReturned异常
对一个查询集进行切片操作会产生一个新的查询集，下标不允许为负数。<br>
**exists**:判断一个查询集中是否有数据。(True False)

---

## 模型类关系
### 三种关系模式
1. 一对多关系<br>
例：图书类-英雄类 
models.ForeignKey() 定义在多的类中。
2. 多对多关系<br>
例：新闻类-新闻类型类 体育新闻 国际<br>
models.ManyToManyField() 定义在哪个类中都可以。
3. 一对一关系<br>
例：员工基本信息类-员工详细信息类<br>
models.OneToOneField() 定义在哪个类中都可以。
### 关联查询(一对多)
**在一对多关系中，一对应的类我们把它叫做一类，多对应的那个类我们把它叫做多类，我们把多类中定义的建立关联的类属性叫做关联属性。**<br>
例：查询图书id为1的所有英雄的信息。<br>
    book = BookInfo.objects.get(id=1)<br>
    book.heroinfo_set.all()<br>
通过模型类查询：<br>
    **HeroInfo.objects.filter(hbook_id=1)**<br>
例：查询id为1的英雄所属图书信息。<br>
    hero =HeroInfo.objects.get(id=1)<br>
    hero.hbook<br>
通过模型类查询：<br>
    **BookInfo.objects.filter(heroinfo__id=1)**<br>
    ![1](Django学习图片6.jpg)
**由一类的对象查询多类的时候：**<br>
    一类的对象.多类名小写_set.all() #查询所用数据<br>
**由多类的对象查询一类的时候：**<br>
    多类的对象.关联属性  #查询多类的对象对应的一类的对象<br>
**由多类的对象查询一类对象的id时候：**<br>
    多类的对象. 关联属性_id<br>
***通过模型类实现关联查询：***<br>
例：查询图书信息，要求图书中英雄的描述包含'八'。<br>
BookInfo.objects.filter(heroinfo__hcomment__contains='八')<br>
例：查询图书信息，要求图书中的英雄的id大于3.<br>
BookInfo.objects.filter(heroinfo__id__gt=3)<br>
例：查询书名为“天龙八部”的所有英雄。<br>
HeroInfo.objects.filter(hbook__btitle='天龙八部')<br>
**通过多类的条件查询一类的数据：**<br>
    一类名.objects.filter(多类名小写__多类属性名__条件名) <br>
**通过一类的条件查询多类的数据：**<br>
    多类名.objects.filter(关联属性__一类属性名__条件名)<br>
### 插入，更新和删除
  调用一个模型类对象的save方法的时候就可以实现对模型类对应数据表的插入和更新。<br>
调用一个模型类对象的delete方法的时候就可以实现对模型类对应数据表数据的删除。
### 自关联
**自关联是一种特殊的一对多的关系。即在一个表中建立外键。**<br>
注：mysql终端中批量执行sql语句：source areas.sql;
### 管理器
在查询语句中都会使用到**类名.objects.方法**，那么objects是什么呢？<br>
objects是Django帮我自动生成的管理器对象，通过这个管理器可以实现对数据的查询。<br>
objects是models.Manger类的一个对象。自定义管理器之后Django不再帮我们生成默认的objects管理器。

1. 自定义一个管理器类，这个类继承models.Manger类。
```
class BookInfoManager(models.Manager):
```
2. 再在具体的模型类里定义一个自定义管理器类的对象。
```
object = 自定义的管理器类()
```
自定义管理器类的应用场景：
1. 改变查询的结果集。
比如调用BookInfo.books.all()返回的是没有删除的图书的数据。
2. 添加额外的方法。
管理器类中定义一个方法帮我们创建对应的模型类对象。
使用self.model()就可以创建一个跟自定义管理器对应的模型类对象。

![1](Django学习图片14.png)

### 元选项
Django默认生成的表名：<br>
    **应用名小写_模型类名小写。**<br>
元选项：<br>
需要在模型类中定义一个元类Meta,在里面定义一个类属性db_table就可以指定表名。
```
class Meta:
    db_table = '表名'
```















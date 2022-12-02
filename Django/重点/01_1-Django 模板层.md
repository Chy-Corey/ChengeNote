## Django 的设计模式 & 模板层

讲到模板层，首先要了解以下 Django 的功能。Django 作为后端框架，不仅可以实现接收请求、访问数据库等操作，也可以完成网页界面的渲染等前端功能。

既然Django 可以完成一个软件/系统所需要的所有功能，那么我们就可以将开发出来的程序叫做”软件“。

既然是软件，那么就有软件的设计模式。传统的软件设计典范为 MVC (Model - View - Controller)，而 Django 使用了一种新的模式：MTV (Model - Template - View)。



### 一、软件设计模式

#### 1. MVC (Model - View - Controller)

MVC的全名是Model View Controller，是模型(Model)－视图(View)－控制器(Controller)的缩写，是一种软件设计典范。MVC用一种业务逻辑、数据与界面显示分离的方法来组织代码，将众多的业务逻辑聚集到一个部件里面，在需要改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑，达到减少编码的时间。增加了模块化，减少了耦合度。

在网站开发中，MVC可以理解为：

- M：模型层，主要用于对数据库层的封装
- V：视图层，用于界面的渲染
- C：控制层，包含了程序的运行逻辑，比如请求的处理、数据的获取以及结果的返回

---

以下为 **MVC 模式的一个使用案例**：

用户首先在界面(View)中进行人机交互，然后请求发送到控制器(Controller)，控制器根据请求类型和请求的指令发送到相应的模型(Model)，模型可以与数据库进行交互，进行增删改查操作，完成之后，根据业务的逻辑选择相应的视图进行显示，此时用户获得此次交互的反馈信息，用户可以进行下一步交互，如此循环。



#### 2. MTV (Model - Template - View)

MTV 的本质其实与 MVC 相同，只不过 MTV 的设计初衷就是为了后端服务的。

- M 代表模型（Model）：负责业务对象和数据库的关系映射(ORM)。

- T 代表模板 (Template)：负责如何把页面展示给用户(html)。

- V 代表视图（View）：负责业务逻辑，并在适当时候调用Model和Template。

除了以上三层之外，还需要一个URL分发器，它的作用是将一个个URL的页面请求分发给不同的View处理。可以看出，MTV模式更加针对于后端的架构。





### 二、模板

Django 中的模板是可以根据字典中的数据动态变化的 html 网页文件。

在前端中，需要在 html 文件中置入 js 代码，才能使文件具有动态效果。为了简便，Django 的模板可以直接绑定字典中的数据，实现动态效果。

#### 1. 模板配置

步骤如下：

1. 在项目下创建 `templates` 文件夹

2. 在 `settings.py` 中对 `TEMPLATES` 配置项进行配置

   - BACKEND：指定模板的引擎（暂时可以使用默认配置）
   - DIRS：模板的搜索目录（可以有多个）
   - APP_DIRS：是否要在应用中的templates文件夹中搜索模板文件（需要配置）
   - OPTIONS：模板的选项（暂时可以使用默认配置）

   初学时，暂且只需要修改DIRS，这个目录就是指向模板所在的地址，所以其代码为：

   ```python
   'DIRS': [os.path.join(BASE_DIR, 'templates')],
   ```

   `os.path.join(BASE_DIR, 'templates')` ，其实就是将根目录和 `/templates` 进行一个拼接，告诉程序 模板文件在哪个文件夹里。当我们的模板属于应用时，虽然这里的路径看起来不太对，但是我们的请求是由应用的 `urls.py` 分发的，所以 Django 会使用对应应用的模型文件。

#### 2. 模板文件编写 & 对应视图函数

在 `templates` 文件夹下，创建 html 文件，编写 html 代码。

以 `index.html` 文件为例，创建好以后，需要对应的试图函数来拉起模板文件。试图函数可以使用 `render()` 来拉起html文件：

```python
# views.py


from django.shrotcuts import render
return render(request, '模板文件名', 字典数据)
# def template_index_view(request):
#     return render(request, 'index.html')
```

可以看到， render 函数非常简洁就可以拉起 html ， 但是我们需要搞清楚其中的原理。

render 是渲染的意思，我们可以用 render 函数渲染出前端界面。render 函数本质上是将字典内的数据替换到html文件中，再将 html 文件的内容改写为json格式，最后返回给客户端。



#### 3. 模板文件携带数据

模板 html 文件可以携带视图函数中的数据，以字典的形式传递到模板。当然，字典也可以嵌套其他的数据类型，只要保证最外层的是字典即可。

```python
# views.py


def index_view(request) {
    dic = {
        "variable_1": "value1",
        "variable_2": "value2"
    }
    return render(request, 'index.html', dic)	# 以字典的形式传值
}
```

视图函数传递字典以后，在模板里直接使用 mustache 语法：`{{变量名}}` 就可以将变量渲染。



#### 4. 模板标签

[内置模板标签和过滤器 | Django 文档 | Django (djangoproject.com)](https://docs.djangoproject.com/zh-hans/4.1/ref/templates/builtins/)

所谓模板标签，其实就是模板中的逻辑运算，而这些逻辑运算符放到标签中。

标签可以写到html的所有地方，包括标签里面。

标签的语法为：

```python
{% 标签开始 %}
# 内容
{% 标签结束 %}
```



1. if 标签

   ```python
   {% if 条件表达式1 %}
   ...
   {% elif 条件表达式2 %}
   ...
   {% endif %}
   ```

   注意：if标签中括号无法提升优先级

2. for 标签

   ```python
   {% for 变量 in 可迭代对象 %}
   ...循环语句
   {% empty %}
   ... 可迭代对象无数据时执行的语句
   {% endfor %}
   ```

   for 标签还内置了很多变量，可以在该节开头的链接查阅。





### 三、模板过滤器

模板过滤器可以在变量输出时对变量的值进行处理，从而改变变量的输出显示，有一种过滤的作用。

这个和 js 的filter 有异曲同工之妙，只不过 js 的 filter 需要自己写判断函数，Django 的过滤器已经封装好了各种功能，直接调用即可。

语法：

```python
{{变量 | 过滤器1: '参数值1' | 过滤器2: '参数值2' | ...}}

# 举例：{{script | safe}}
# 这里使用了 safe 过滤器，告诉 Django 这个变量是安全的可执行代码，不需要转义成纯文本
```

---

常用过滤器：

|       过滤器       |                      作用                       |
| :----------------: | :---------------------------------------------: |
|       lower        |                 字符串全部小写                  |
|       upper        |                 字符串全部大写                  |
|        safe        |         不对变量内的字符串进行html转义          |
|      add: "n"      |               将 value 的值增加 n               |
| truncatechars: "n" | 如果字符串字符多余指定字符，则截断并以 ... 结尾 |





### 四、模板继承

模板继承类似于 java 的继承，父模板内容可以重复使用，子模板直接继承父模板的**全部内容**并且可以对定义了快标签的内容进行重写。

父模板语法：

- 定义父模板中的块标签(block)，block 在子模板中允许被修改

  ```python
  # 可以给任何的html标签打上块标签，且数量不限
  {% block mytitle %}
  <title> 主页 </title>
  {% endblock mytitle %}
  ```

- block：在父模板中定义，可以在子模板中覆盖

子模板语法：

- 在子模板的第一行写上 继承标签(extends)

  ```python
  {% extends 'base.html' %}
  ```

- 子模板重写父模板中的内容块

  ```python
  {% block block_name %}
  用于覆盖的内容
  {% endblock block_name %}
  ```

注意：**无法继承动态数据**

## Django 应用及分布式路由





### 一、 应用

应用在Django中是一个独立的业务模块，可以包含自己的路由、视图、模板和模型

#### 1. 创建应用

1. 用 `manage.py` 中的子命令 `startapp` 创建应用文件夹

   ```python
   python manage.py startapp music
   ```

2. 在 `settings.py` 的 `INSTALLED_APPS` 列表中配置、声明此应用

   注意： `settings.py` 只有根目录才拥有，即项目项目本身可以配置，应用不可配置

   ```python
   INSTALLED_APPS = [
       # ...项目自带的应用
       'user',	# 注册的应用，用户信息模块
       'music', # 注册的应用，音乐模块
   ]
   ```



#### 2. 应用目录

观察新建的 app 的目录下，有以下文件：

```
├─admin.py	# 管理员模块
├─apps.py
├─models.py # 数据库模型层
├─tests.py	# 测试模块
├─views.py	# 视图函数
├─__init__.py
├─migrations# 迁移文件
|     └__init__.py
```

当然，`urls.py` 等文件也可以手动创建。



### 二、分布式路由

Django中，主路由配置文件可以不处理用户具体的路由，主路由配置文件可以进行请求的分发，即将请求分发到每个 app，具体的请求由各自应用目录下的 `urls.py` 进行处理。

**以http ://127.0.0.1:8000/music/index 为例**

#### 1. 分布式路由配置第一步

步骤1：主路由中调用 `include` 函数

语法：`include('应用名字.url模块文件名')`

作用：将当前路由分发到对应应用的路由配置文件的 urlpatterns 进行处理。

```python
from django.urls import path, include
from . import views


urlpatterns = [
    # ... 主路由中自身的视图函数匹配
    path('music/', include('music.urls'))	# 进行了路由的分发
]
```

#### 2. 分布式路由配置第二步

步骤2：应用下配置 `urls.py`

语法：和主路由中的url配置完全相同

```python
from django.urls import path
from . import views


urlpatterns = [
    path('index', views.index_view),
    path('template/index', views.template_index_view),
]
```

配置好路由以后，就要编写对应的视图函数：

```python
from django.http import HttpResponse
from django.shortcuts import render


def index_view(request):
    return HttpResponse('Music! 基尼太没，贝贝，噢~')


def template_index_view(request):
    return render(request, 'index.html')
```

请注意：如果视图函数要拉起模板中的 html 文件，请使用 `render` 函数。关于模板的内容请查阅模板相关的笔记。

### 三、应用下的模板

应用内部也可以创建模板，步骤为：

1. 在应用文件夹下创建 `templates` 文件夹

2. 在 `settings.py` 中开启应用模板的功能

   在 `TEMPLATE` 配置项中，令`APP_DIRS` 值为 `True`

如果应用和项目目录下都存在 `templates`，Django查询模板的规则为：

1. 优先查找项目目录下的模板
2. **如果项目目录下没有，则按照 `INSTALLED_APPS` 下注册应用的顺序查找**



对于规则2，会导致不同应用的相同请求只能请求到同一应用的templates，解决方案是在templates文件夹下再建立一个文件夹，防止查询错误。

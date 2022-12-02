### Django应用及分布式路由

#### 应用

应用在Django项目中是一个独立的业务模块，可以包含自己的路由、视图、模板、模型

##### 创建应用

- 步骤1

  用manage.py 中的子命令 startapp 创建应用文件夹

  ```python
  python manage.py startapp music
  ```

- 步骤2

  在settings.py 的 INSTALLED_APPS 列表中配置安装此应用

  ```python
  # settings.py
  INSTALLED_APPS = [
      # ...
      'user', # 用户信息模块
      'music', # 音乐模块
  ]
  ```

##### 应用文件夹分析

- migrations

  主要涉及Database，即数据库相关应用

- models.py

  模型层的入口，DB相关

- tests.py

  用于测试

#### 分布式路由

Django中，主路由配置文件(urls.py)可以不处理用户具体路由，主路由配置文件可以做请求的分发(分布式请求处理)。具体的请求可以由各自的应用来进行处理。

##### 分布式路由配置

- 步骤1 - 主路由中调用include函数

  语法：`include('app名字.url模块名')`

  作用：将当前路由转到各个应用的路由配置文件的urlpatterns进行分布式处理

  ```python
  from django.urls import path, include
  
  urlpatterns = [
      # http://127.0.0.1:8000/music/index
      path('music/', include('music.urls'))
  ]
  ```

- 步骤2 - 应用下配置urls.py

  应用下手动创建urls.py文件，内容和主路由完全一致

  ```python
  from django.uls import path
  from . import views
  
  urlpatterns = [
      # http://127.0.0.1:8000/music/index
      path('index', views.index_view)
  ]
  ```

  

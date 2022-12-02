### admin管理后台

#### 什么是admin管理后台

- django提供了比较完善的后台管理数据库的接口，可供开发过程中调试和测试使用
- django会搜集所有已注册的模型类，admin为这些模型类提供数据管理界面供开发者使用

#### admin配置步骤

- 创建后台管理账号 - 该账号为管理后台最高权限账号(可以创建多个超级用户)

  `python manage.py createsuperuser`

  ```python
  $ python manage.py createsuperuser
  Username (leave blank to use '23147'): 23147	# 此处输入用户名
  Email address: 2314727037@qq.com	# 邮箱
  Password: 	#输入密码(输入时看不到输入)，密码最好复杂一些
  Password (again): 	# 重复输入密码确认
  Superuser created successfully.
  
  ```


#### 注册自定义模型类

如果要自己定义的模型类也能在`/admin`后台管理界面中显示和管理，需要将自己的类注册到后台管理界面

- 注册步骤：
  1. 在应用app的admin.py中导入注册要管理的模型models类，如：`from .models import Book`
  2. 调用 `admin.site.register`方法进行注册，如：`admin.site.register(自定义模型类)`

#### 修改自定义模型类的数据样式

- admin后台管理数据库中，对自定义的数据记录都展示为`'XXXX object'`类型的记录，不方便阅读判断

- 在用户自定义的模型类中可以重写` __str__(self)`方法来改变显示

  ```python
  calss Book(models.Model):
      ...
      def __str__(self):
          return "书名：" + self.title
  ```

- 也可以通过模型管理器类来修改

#### 模型管理器类

- 作用：作为后台管理界面添加便于操作的新功能

- 说明：后台管理器类必须继承自`django.contrib.admin`里的ModelAdmin类

- 使用方法：

  1. 在<应用app>/admin.py里定义模型管理器类

     ```python
     class XXXManager(admin.ModelAdmin):
         ...
     ```

  2. 绑定注册模型管理器和模型类

     ```python
     from django.contrib import admin
     from .models import *
     admin.site.register(XXX, XXXManager)# 绑定XXX模型类与管理器类XXXManager
     ```

- 案例：

  ```python
  # file:bookstore/admin.py
  from django.contrib import admin
  from .models import Book
  
  class BookManager(admin.ModelAdmin):
      list_display = ['id', 'title', 'price']
      
      
  admin.site.register(Book, BookManager)
  ```

- 模型管理器类中的属性：

  - list_display:列表页显示哪些字段的列
  - list_display_links:控制list_display中的字段，哪些可以链接到修改页
  - list_filter:添加过滤器，可以过滤查找
  - search_fields:添加搜索框(模糊查询)
  - list_editable:添加可在列表页编辑的字段

  [更多属性查阅官方文档](https://docs.djangoproject.com/en/4.0/ref/contrib/admin/)

#### 再看Meta类

之前通过内嵌Meta类，改变模型类的一些属性，比如表名

也可以通过Meta内嵌类，定义模型类的属性，用法如下：

```python
class Book(models.Model):
    title = CharField(...)
    class Meta:
        # 1. 该模型所用的数据表的名称(设置完成后需更新同步数据库)
        db_table = '数据表名'
        # 2. 给模型对象的一个易于理解的名称(单数)，用于显示在admin管理界面中
        verbose_name = '单数名'
        # 3. 该对象复数名，用于显示在admin管理界面中
        verbose_name_plural = 'f'
```


































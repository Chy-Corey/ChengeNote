## Admin 后台管理

在学习本章节内容时，需要先了解 app 应用注册的相关知识，可以查阅01_0节：Django 应用及分布式路由

### 1. 注册自定义模型类

因为Django默认的模型类是框架写好的，无法被修改，所以如果要自己定义的模型类也能在 admin 后台界面中显示管理，需要将自己的类注册到后台管理界面。

注册步骤：

1. 在应用 app 中的 `admin.py` 中导入注册要管理的模型 models 类，如：

   ```python
   from .models import Book
   ```

   当然，如果项目没有注册其他的app，也可以直接在项目目录下注册。

2. 调用 `admin.site.register` 方法进行注册，如：

   ```python
   admin.site.register(自定义模型类)
   # 举例
   # admin.site.register(Book)
   ```
   

注册完模型类以后，就可以在 admin 界面访问经过注册的数据。但是如果只进行注册，只能进行最基础的显示，不能自定义更加有好的显示功能。Django 也提供了可以进行友好交互的类：模型管理器类。使用这个类，可以优化交互效果。



### 2. 使用模型管理器类

模型管理器类可以为 admin 界面添加便于操作的新功能。该类继承于 `django.contrib.admin.ModelAdmin` 类。

#### 使用方法

1. 在应用的 `admin.py` 中定义模型管理器类(建议类名写为：模型类名Manager)

   ```python
   class XXXManager(admin.ModelAdmin):
       ...
   ```

2. 将模型类和模型管理器类绑定

   ```python
   admin.site.register(XXX, XXXManager)
   ```



#### 模型管理器类的语法

模型管理器类内置了许多数组以及变量，可以用这些数组和变量来改变页面样式、改善交互方式。

使用方式大概如下：

```python
class XXXManager(admin.ModelAdmin):
    list_display = ['id', 'title', 'name', 'price']
    list_display_links = ['name']
    list_filter = ['name']
    search_fields = ['title']
    list_editable = ['price']
```

这里每个列表的名字都是 Django 内置的，也就是说我们不能进行修改。列表内的元素对应模型类中的变量名(也就是表的列名)，这样就可以针对表中的不同列实现个性化的功能。

- list_display：界面上显示哪些字段
- list_display_links：可以通过哪个字段跳转到修改页
- list_filter：添加过滤器，可以通过哪个字段来过滤
- search_fields：可以通过哪个字段进行查询(模糊查询)
- list_editable：可以在列表页直接编辑哪些字段(该字段不能和 list_display_links 字段相同)

更多的选项可以查阅官网链接：[The Django admin site | Django documentation | Django (djangoproject.com)](https://docs.djangoproject.com/en/4.1/ref/contrib/admin/)
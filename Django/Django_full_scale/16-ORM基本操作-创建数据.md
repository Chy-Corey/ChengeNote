### ORM创建数据

#### 创建数据时的常见问题

- 问题1：当执行 python manage.py makemigrations 出现如下报错时

  ```
  You are trying to add a non-nullable field 'des' to book without a default; we can't do that (the database needs something to populate existing rows).
  Please select a fix:
  	1) Provide a one-off default now(will be set on all existing rows with a null value for this column)
  	2) Quit, and let me add a default in models.py
  ```

  根据报错的提示可以得到解决方法

- 问题2：数据库迁移混乱问题

  数据库中 django_migrations 表记录了migrate的全过程，项目各应用的migrate文件应该与之对应，否则migrate会报错。比如多人协同操作时，各成员的migrate文件不一样，这时migrate会报错。

  解决方案：

  1. 删除 migrations 里的所有 `000?_XXXX.py`(__init\_\_.py除外)】
  2. 删库

#### 创建数据

在ORM中进行数据库操作其实就是对管理器对象进行操作

##### 管理器对象

每个继承自models.Model 的模型类，都会有一个objects对象被同样继承下来。这个对象叫管理器对象

数据库的增删改查可以通过模型的管理器实现

```python
class MyModel(models.Model):
    ...

MyModel.objects.create(...) # objects s    
```

##### 创建数据

Django ORM 使用一种直观的方式把数据库表中的数据表示成Python对象

创建数据中每一条记录就是创建一个数据对象

- 方案1

  MyModel.objects.create(属性1=值1, 属性2=值2, 属性3=值3, ......)

  - 成功：返回创建好的实体对象
  - 失败：抛出异常

- 方案2

  创建MyModel实例对象，并调用 save() 进行保存

  ```python
  obj = MyModel(属性1=值1, 属性2=值2, ......)
  obj.属性 = 值
  obj.save()
  ```

  

​		
































































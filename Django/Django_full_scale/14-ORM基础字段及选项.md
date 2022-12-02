### ORM基础字段及选项

ORM映射图<img src="E:\Note\fromTypora\Django_full_scale\img\ORM映射.png" alt="ORM映射" style="zoom:50%;" />

#### ORM操作是什么

基本操作即为增删改查，反映到Django中就是对模型类的操作

#### 模型类创建流程

- 创建应用

- 在应用下的models.py中编写模型类

  ```python
  from django.db import models
  class 模型类名(models.Model):
      字段名 = models.字段类型(z)
  ```
  
- 迁移同步

  `makemigrations & migrate`

#### 字段类型

创建表时，我们需要知道数据库中的字段类型在Django的ORM中对应什么属性

- BooleanField()
  - 数据库类型：tinyint(1)
  - 编程语言中：使用True和False来表示值
  - 数据库中：使用1和0来表示具体的值

- CharField()
  - 数据库类型：varchar
  - 注意：必须指定maz_length参数值

- DateField()

  - 数据库类型：date

  - 作用：表示日期

  - 参数

    1. auto_now: 每次保存对象时，自动设置该字段为当前时间(取值：True/False)

    2. auto_now_add: 当对象第一次被创建时，自动设置当前时间(取值：True/False)

    3. default: 设置当前时间(取值：字符串格式时间，如'2019-6-1')

       **以上三个参数只能选一个**

- DataTimeField()

  - 数据库类型：datetime(6)
  - 作用：表示日期和时间
  - 参数和DateField相同

- FloatField()

  - 数据库类型：double
  - 编程语言和数据库中都是用小数表示值

- DecimalField()

  - 数据库类型：decimal(x, y)
  - 编程语言中，使用小鼠表示该列的值
  - 数据库中使用小数
  - 参数：
    1. max_digits: 位数总是，包括小数点后的位数，改制必须大于等于decimal_places
    2. decimal_places: 小数点后的数字数量

- EmailField()
  - 数据库类型：varchar
  - 编程语言和数据库中使用字符串
- IntegerField()
  - 数据库类型：int
  - 编程语言和数据库中使用整数

- ImageField()
  - 数据库类型：varchar(100)
  - 作用：保存图片的**路径**
  - 编程语言和数据库中使用字符串
- TextField()
  - 数据库类型：longtext
  - 作用：表示不定长的长字符串

更多的Field类型：[Model field reference | Django documentation | Django (djangoproject.com)](https://docs.djangoproject.com/en/4.0/ref/models/fields/#model-field-types)

#### 字段选项

字段选项：指定所创建列的额外的信息

- primary_key

  如果设置为True，表示该列为主键。如果存在字段定义了primary_key=True，那么数据库不会自动创建id字段

- blank

  设置为True时，字段可以为空。设置为False时，必须填写此字段

- null

  - 如果设置为True，则表示一整列都可以为空
  - 默认为False，如果此选项为False，加入default选项来设定默认值

- default

  设置所在列的默认值

- db_index

  如果设置为True，表示为该列增加索引

- unique

  如果设置为True，表示该字段在数据库中的值必须是唯一的(不可重复)

- db_column

  指定列的名称，如果不指定的话则采用属性名作为列名

- verbose_name

  设置此字段在admin界面上显示名称

##### 字段选项样例

```python
# 创建一个属性，表示用户名称，长度30字符，必须唯一，不能为空，添加索引
name = models.CharField(max_length=30, unique=True, null=False, db_i)
```

更多字段选项：[Model field reference | Django documentation | Django (djangoproject.com)](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models)






















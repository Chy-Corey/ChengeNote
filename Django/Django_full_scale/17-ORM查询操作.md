### ORM查询操作

#### 查询操作简介

其实在ORM中，这一节的查询和上一节的创建，都是对数据表内部的操作(比如查询某一行，创建一行数据)。这一类操作，在ORM中都被封装到objects对象(管理器对象)中，并可以继承下来。

- 数据库的查询需要使用管理器对象进行
- 通过`MyModel.objects`管理器方法调用数据库查询方法
- MyModel就是对应一个表，这个表里有objects对象，对象里有很多方法，用于对表进行增删改查

#### 基础查询方法

|   方法   |              说明              |
| :------: | :----------------------------: |
|  all()   | 查询全部记录，返回QuerySet对象 |
|  get()   |     查询符合条件的单一记录     |
| filter() |    查询复合体案件的多条记录    |
| exclude  |   查询符合条件之外的全部记录   |
|   ...    |              ...               |

- all()

  - 用法：MyModel.objects.all()
  - 作用：查询MyModel实体中所有的数据
  - 等同于 `select * from table`
  - 返回值：QuerySet容器对象，内部存放 MyModel 实例

  ```python
  from bookstore.models import Book
  books = Book.objects.all()
  for book in books:
      print("书名", book.title, "出版社：",)
  ```

- values('列1', '列2', ... )

  - 用法：MyModel.objects.values( ... )
  - 作用：查询部分列的数据并返回所有行的字典形式
  - 等同于 `select 列1, 列2 from xxx`
  - 返回值：QuerySet，返回查询结果容器，容器内是字典，每个字典代表一条数据，格式为：{'列1': 值1, ''列2': 值2}

- values_list('列1', '列2', ... )
  - 用法：
  - MyModel.objects.values_list( ... )
  - 作用：查询部分列的数据并返回所有行的元组形式
  - 等同于`select 列1, 列2 from xxx`
  - 返回值：QuerySet容器对象，容器内是元组。会将查询出来的数据封装到元组中，再封装到查询集合QuerySet中

- order_by()
  - 用法：MyModel.objects.order_by('-列', '列')
  - 作用：与all()方法不同，它会用SQL语句的ORDER BY 子句对查询结果根据某个字段选择性的进行排序
  - 说明：默认是按照升序排序，如果需要降序排序则在列前增加'-' 表示

#### 管理器对象的方法本质

其实管理器对象中的方法，**本质上是对QuerySet对象进行处理的方法**。只要我们是对QuerySet对象进行操作，**方法就可以一直调用下去**。比如我先查询到了部分数据(使用values方法)，接着可以再进行order_by方法对查询结果进行排序，这样对程序的编写提供了很大的灵活性。

```python
books = Book.objects.values('title').order_by('price')
```

对于QuerySet对象，还有一个操作方法`query`。对QuerySet对象使用query方法，可以获取其对应的sql语句，也就是这个QuerySet集合是通过什么sql语句得到的。

#### 条件查询

- filter(条件)

  - 语法：MyModel.objects.filter(属性1=值1, 属性2=值2)
  - 作用：返回包含此条件的全部数据集
  - 返回值：QuerySet容器对象，没不存放MyModel实例
  - 说明：多个属性在一起时为关系“与”

  ```python
  # 查询出版社为“清华大学出版社”的图书
  from bookstore.models import Book
  books = Book.objects.filter(pub="清华大学出版社")
  for book in books:
      print("书名:", book.title)
      
  # 查询Author实体中name为王老师并且age是28的
  authors = Author.objects.filter(name='王老师', age=28)
  ```

- exclude(条件)

  - 语法：MyModel.objects.exclude(条件)
  - 作用：返回不包含此条件的全部数据集
  - 实例：查询不是清华大学出版社且定价不等于50的全部图书

  ```python
  books = models.Book.objects.exclude(pub="清华大学出版社",price=50)
  for book in books:
      print(book)
  ```

- get(条件)
  - 语法：MyModel.objects.get(条件)
  - 作用：返回满足条件的唯一一条数据
  - 说明：该方法只能返回一条数据，如果查询结果多于一条或者一条都没有，那么抛出异常

#### 查询谓词/非等值查询

诠释：做更灵活的条件查询时需要使用查询谓词

说明：每一个查询谓词是一个独立的查询功能

- __exact：等值匹配

  ```python
  Author.objects.filter(id__exact=1)
  # 等同于 select * from author where id = 1
  ```

- __contains：包含指定值

  ```python
  Author.objects.filter(name__contains='w')
  # 等同于select * from author where name like '%w%'
  ```

- __startswith：以xxx开始

- __endswith：以xxx结束

- __gt：大于指定值

- __gte：大于等于

- __lt：小于

- __lte：小于等于

- __in：查找数据是否在指定范围内

  ```python
  Author.objects.filter(country__in=['cHINA', 'JAPAN', 'US'])
  ```

- __range：查找数据是否在指定的区间范围内

  ```python
  Author.objects.filter(age__range=(25, 50))
  # 查找年龄在35到50内的作者
  # 等同于 select ... where Author Between 35 and 50;
  ```

  
































































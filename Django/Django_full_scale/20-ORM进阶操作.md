## ORM进阶操作

### F对象和Q对象

#### 一、 F对象

##### 1. 基本概念

- 一个F对象代表数据库中某条记录的字段的信息

- 作用

  - 通常是对数据库中的字段值在不获取的情况下进行操作
  - 用于类属性(字段)之间的比较

- 语法：

  ```python
  from django.db.models import F
  F('列名')
  ```

- 示例1 更新Book实例中所有的零售价涨价10元

  ```python
  Book.objects.all().update(market_price=F('market_price')+10)
  # 以上为通过F对象操作，做法优于以下代码
  books = Book.objects.all()
  for book in books:
      book.market_price = book.market_price + 10
      book.save()
  ```
  
- 示例2 对数据库中两个字段的值进行比较，列出哪些书的零售价高于定价

  ```python
  from django.db.models import F
  from bookstore.models import Book
  
  # 这里如果不使用F对象不好进行比较
  books = Book.objects.filter(market_price__gt=F('price'))
  for book in books:
      print(book.title, book.price, book.market_)
  ```
  
  

##### 2. 进阶理解

F对象可以处理资源竞争问题。比如一万个人同时给一个博客点赞，这时后端处理数据时，由于同时获取到当前点赞数都是0，那么更新以后只有一个点赞数，而不是一万个。

此时我们可以对字段进行F对象包裹。这样我们不用取出这个字段值(比如点赞数)进行相加，而是对其当前值进行加1操作(即上锁)。

那么概念中的”一个F对象代表数据库中某条记录的字段的信息“，其实表示的是**先不取出字段的值，先知道字段的位置，等到操作数据库时，再上锁并进行更新操作**。

#### 二、Q对象

当再获取查询结果集要使用与或非等逻辑操作时，可以借助Q对象进行操作

如：想找出定价低于20或者清华大学出版社的书时，可以写成：

```python
Book.objects.filter(Q(price__l20)|Q(pub="清华大学出版社"))
```

 Q对象再数据包`django.db.models`中，使用需要导入，和F对象一致

- 作用：在条件中用于与或非操作(and&    or|     not~)

- 语法：

  ```python
  from django.db.models import Q
  
  Q(条件1)|Q(条件2)	# 条件1或条件2成立
  Q(条件1)&Q(条件2)	# 条件1和条件2同时成立
  Q(条件1)&~Q(条件2)  # 条件1成立且条件2不成立 
  ```

- 示例

  ```python
  from django.db.models import Q
  
  # 查找清华大学出版社的书或价格低于50的书
  Book.objects.filter(Q(market_price__lt=50) | Q(pub="清华大学出版社"))
  # 查找不是机械工业出版社的书且价格低于50
  Book.objects.filter(Q(market_price__lt=50) &~ Q(pub="机械工业出版社"))
  ```



### 聚合查询与原生数据库操作

#### 一、聚合查询

聚合为统计而生。聚合查询是指对一个数据表中的一个字段的数据进行部分或全部统计查询。查bookstore_book数据表中的全部书的平均价格，查询所有书的总个数等，都要使用聚合查询。

聚合查询分为：**整表聚合**、**分组聚合**

##### 整表聚合

不带分组的聚合就是整表聚合，指将全部数据进行集中统计查询

- 聚合函数(需要导入)

  - 导入方法：`from django.db.models import *`
  - 聚合函数：`Sum`, `Avg`, `Count`, `Max`, `Min`

- 语法

  `MyModel.objects.aggregate(结果变量名=聚合函数('列名'))`

  返回值：结果变量名和值组成的字典

  格式：{"结果变量名": 值}

##### 分组聚合

分组聚合是指通过计算查询结果中每一个对象所关联的对象集合，从而得出总计值(也可以是平均值或总和)，即为查询集的每一项生成聚合。

- 语法

  QuerySet.annotate(记过变量名 = 聚合函数('列'))

- 返回值

  QuerySet

- 步骤：
  1. 通过查询结果`MyModel.objects.values`查找查询要分组聚合的列
  
     `MyModel.objects.values('列1', '列2')`
  
     例如：
  
     ```python
     pub_set = Book.objects.values('pub')
     print(pub_set)
     ```
  
  2. 通过返回结果的`QuerySet.annotate`方法分组聚合得到分组结果
  
     `QuerySet.annotate(名=聚合函数('列'))`
  
     ```python
     pub_count_set = pub_set.annotate(myCount=Coubt('pub'))
     print(pub_count_set)
     ```
     

#### 二、 原生数据库操作

Django也可以支持直接用 sql 语句的方式通信数据库

- 查询：使用MyModel.objects.raw()进行数据库查询操作

- 语法：MyModel.objects.raw(sql语句, 拼接参数)

- 返回值：RawQuerySet 集合对象（只支持基础操作，比如循环）

  ```python
  books = models.Book.objects.raw('select * from bookstore_book')
  for book in books:
      print(book)
  ```

  

这里不做笔记了，因为用不到














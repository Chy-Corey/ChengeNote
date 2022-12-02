### 模型层及ORM

模型层是通向数据库的唯一方式，只有模型层可以管理数据，在Django框架中，如果要操作数据库，使用ORM，不要直接操作数据库！

MySQL相关命令

```mysql
# 启动服务
net start SQL服务名
# 关闭服务
net stop SQL服务名
# 查看服务是否开启
mysqladmin p

```

#### Django配置Mysql环境

[MySQL 安装 | 菜鸟教程 (runoob.com)](https://www.runoob.com/mysql/mysql-install.html)

[Windows10 MYSQL Installer 安装（mysql-installer-community-5.7.19.0.msi） | 菜鸟教程 (runoob.com)](https://www.runoob.com/w3cnote/windows10-mysql-installer.html)

- 需安装mysqlclient包

  ```python
  pip install mysqlclient
  ```

- 创建数据库

- 进入mysql数据库

  mysql –u root –p

- 执行

  - create database 数据库名 default charset utf8;
  - 通常数据库名和项目名保持一致

- settings.py 进行数据库配置

  - 修改DATABASES 配置项的内容，由sqlite3变为mysql

  ```PYTHON
  DATABASES = {
      'default': {
          'ENGINE': 'django.db.backends.mysql',
          'NAME': 'thirdsite',
          'USER': 'root',
          'PASSWORD': 'Ysy.2001.09.21',
          'HOST': '127.0.0.1',
          'PORT': '3306'
      }
  }
  ```

#### 什么是模型

- 模型是一个python类，它是由django.db.models.Model派生出的子类。
- 一个模型类代表数据库中的一张数据表
- 模型类中每一个类属性都代表数据库中的一个字段
- 模型是数据交互的接口，是表示和操作数据库的方法

#### ORM框架

- 定义：ORM(Object Relational Mapping) 即对象关系映射，可以避免通过sql语句操作数据库，直接用python语法进行操作。
- 作用：
  1. 建立模型类和表之间的对应关系，允许我们通过面向对象的方式来操作数据库。
  2. 根据设计的模型类生成数据库中的表格
  3. 通过简单的配置就可以进行数据库的切换

- 一些查看数据库的语句

  ```mysql
  use dbname;     # 使用指定的数据库
  show tables;	# 展示表
  desc tablename; # 展示所指定表中的结构
  ```

#### 模型类的创建

```python
from django.db import models
class 模型类名(models.Model):
    字段名 = models.字段类型(z)
```



#### 模型示例

此示例为添加一个bookstore_book 数据表来存放图书馆中书目信息

1. 添加一个bookstore的app

   ```python
   python manage.py startapp bookstore
   ```

2. 添加模型类并注册app

   模型类代码示例：

   ```python
   # file: bookstore/models.py
   from django.db import models
   
   class Book(models.Model):
       title = models.CharField("书名", max_length=1, default='')
       price = models.DecimalField('定价', max_digits=7, decimal_places=2, default=0.0)
   ```

3. 数据库迁移

   迁移是Django在修改模型层后，同步这些更改到数据库的方式

   - 生成迁移文件 - 执行 `python manage.py makemigrations`

     将应用下的models.py 文件生成一个中间文件，并保存在migrations文件夹中

   - 执行迁移脚本程序 - 执行`python manage.py migrate`

     执行迁移程序实现迁移。将每个应用下的migrations目录中的中间文件同步回数据库


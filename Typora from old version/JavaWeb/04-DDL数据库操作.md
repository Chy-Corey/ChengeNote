### DDL 数据库操作

#### 1. 数据库创建

- 创建数据库

  ```mysql
  create database 数据库名称;
  ```

- 创建数据库，判断数据库是否存在，且指定字符集为gbk

  ```mysql
  create database if not exists dbname character set gbk;
  ```

#### 2. 数据库查询

- 查询所有数据库名称

  ```mysql
  show databases;
  ```

- 查询某个数据库的创建语句

  ```mysql
  show create database dbname;
  ```

#### 3. 数据库修改

- 修改数据库的字符集

  ```mysql
  alter database dbname character set utf8;
  ```

#### 4. 数据库删除

- 删除数据库

  ```mysql
  drop database dbname;
  ```

- 删除数据库，判断数据库是否存在

  ```mysql
  drop database if exists dbname;
  ```

#### 5. 使用数据库

- 使用数据库

  ```mysql
  use dbname;
  ```

- 查询正在使用的数据库名称

  ```mysql
  select database();
  ```

  


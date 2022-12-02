### DDL 表操作

#### 1. 表创建

- 创建表

  ```mysql
  create table tbname(
  	列名1 数据类型1,
  	列名2 数据类型2,
    ...
  	列名n 数据类型n);
  ```

  常用的数据类型：

  1. int
  2. double:`score double(5,2)`
  3. data:日期，只包含年月日,yyyy-MM-dd
  4. datatime:日期，包含年月日时分秒 yyyy-MM-dd HH:mm:ss
  5. timestamp:时间戳类型，包含年月日时分秒 yyyy-MM-DD HH:mm:ss;如果不给此数据类型赋值，则自动设置为系统时间
  6. varchar:字符串类型`name varchar(20)`（字符数最多为20）

- 复制表

  ```mysql
  create table tbname1 like tbname2;
  ```

  

#### 2. 表查询

- 查询某一数据库中所有表的名称

  ```mysql
  show tables;  
  ```

- 查询表结构(describe)

  ```mysql
  desc tbname;
  ```

  

#### 3. 表修改

- 修改表名

  ```mysql
  alter table tbname rename to newtbname;
  ```

- 修改表的字符集

  ```mysql
  alter table tbname character set new字符集;
  ```

- 添加一列

  ```mysql
  alter table tbname add 列名 数据类型;
  ```

- 修改列名 类型

  ```mysql
  alter table tbname change 列名 新列名 新数据类型;
  ```

  ```mysql
  alter table tbname modify 列名 新数据类型;
  ```

- 删除列

  ```mysql
  alter table tbname drop listname;
  ```

#### 4. 表删除

- 删除表

  ```mysql
  drop table tbname;
  ```

- 删除表如果表存在

  ```mysql
  drop table if exists tbname;
  ```

  
### DML 数据操作

#### 1. 添加数据

```mysql
insert into tbname(listname1, listname2, ... , listnamen) values(value01, value02, ... , value0n), (value11, value12, ... , value1n), (...)
```

注意：

 	1. 列名和值要一一对应
 	2. 如果表名后不定义列名，则默认给所有列添加值
 	3. 除了数字类型，别的类型都要添加引号(如date类型：`1893-11-11`)

#### 2. 删除数据

```mysql
delete from tbname [where 条件]
```

如果不加条件，则删除全部记录，但是不建议这么使用，因为效率低，有100条记录就会执行100次delete。

可以使用以下方法：

```mysql
TRUNCATE TABLE tbname;
-- 先删除表，然后再创建一张一样的表
```

#### 3. 修改数据

```mysql
update tbname SET listname1 = value1, listname2 = value2, ... [where 条件]
```

如果不加条件，则修改所有记录


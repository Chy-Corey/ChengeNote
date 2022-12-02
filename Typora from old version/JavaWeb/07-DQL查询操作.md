### 数据查询操作                                                                                                                                

#### 1. 基础查询

语法：

```mysql
select 
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段
having
	分组之后的条件
order by
	排序
limit
	分页限定
```

- 去除重复

  ```mysql
  -- distinct
  select distinct address from student;
  ```

- 计算列

  可以进行四则运算。当null参与运算时，结果都为null，如果要替换null，可以使用`ifnull(param1, param2)`函数

  param1用于选择要判断哪个是null，param2为null的替换值

- 起别名

  ```mysql
  select address as 地址 from stutent;
  ```

#### 2. 条件查询

模糊查询like

占位符：

- _    单个任意字符
- %   多个任意字符

#### 3. 排序

语法：

```mysql
order by 排序字段1 排序方式1， 排序字段2 排序方式2...
```

如果不写排序方式，则升序排序(ASC)

降序排序为DESC

可以写多个排序字段和方式，第一个权重最高，如果第一个无法排序，则按照第二个排，以此类推

#### 4. 聚合函数

将一列数据作为整体，进行纵向计算。(比如算平均分，中位数)

1. count:计算个数
2. max:计算最大值
3. min:计算最小值
4. sum:求和
5. avg:求平均

**聚合函数会忽略NULL值**，如果不想忽略，可以使用ifnull()，或者选择主键(非空的列)

```mysql
select count(IFNULL(name, unknow)) from student;
```

#### 5. 分组查询

语法： `group by 分组字段`

注意：分组后，只能查询分组字段和聚合函数，为分组的字段查询无意义。

where和having的区别：

 	1. where在分组之前进行限定，如果不满足条件则不参与分组；having在分组后限定，如果不满足结果，则不会被查询出来
 	2. where后面不可以跟聚合函数，having可以

#### 6. 分页查询

```mysql
limit 开始的索引, m
```


























### ORM删除操作

#### 单个删除

步骤：

1. 查找查询结果对应的一个数据对象
2. 调用这个数据对象的delete()方法完成删除

```python
try:
    auth = Author.objects.get(id=1)
    auth.delete()
except:
    print(删除失败)
```

#### 批量删除

1. 查找查询结果集中满足条件的全部QuerySet查询集合对象
2. 调用查询集合对象的delete()方法实现删除

```python
# 删除全部作者中年龄大于65岁的全部信息
auths = Author.objects.filter(age__gt=65)
auths.delete()
```

#### 伪删除

- 业务中，通常不会轻易删除数据，取而代之的是做伪删除，即在表中添加一个布尔型字段(is_active)，默认为True。执行删除时，将欲删除的is_active字段置为False
- 注意：用伪删除时，确保显示数据时添加了`is_active=True`的过滤查询
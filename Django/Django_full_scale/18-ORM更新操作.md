### ORM更新操作

#### 更新单个数据

1. 查

   通过get()得到要修改的实体对象

2. 改

   通过`对象.属性`的方式修改数据

3. 保存

   通过 `对象.save()`保存数据

#### 更新批量数据

- 直接调用QuerySet的update(属性=值)实现批量修改

- 示例：

  ```python
  # 将id大于3的所有图书价格定位0
  books = Book.objects.filter(id__gt=3)
  books.update(price=0)
  # 将所有书的零售价定为100
  books = Book.objects.all()
  books.update(market_price=100)
  ```

  
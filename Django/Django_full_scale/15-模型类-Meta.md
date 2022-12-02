## 模型类-Meta

### Meta类

使用模型内部 Meta 类可以给模型赋予属性，Meta 类下有很多内建的类属性，可以对模型类做一些控制

示例：

```python
class Book(models.Model):
    title = models.CharField("书名", max_length=50, default='')
    price = models.DecimalField('定价', max_digits=7, decimal_places=2, default=0.0)
    info = models.CharField("描述", max_length=100, default='')
    
    class Meta:
        db_table = 'book'	# 改变当前模型类对应的表名
        verbose_name = '图书'	# 改变 admin 界面的列名(单数)
        verbose_name_plural = verbose_name	# 改变 admin 界面的列名(复数)
        
```


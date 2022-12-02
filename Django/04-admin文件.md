### admin.py

#### 1. admin文件的作用

有时我们想在浏览器观察项目的状态(url为admin)，那么我们可以对每个应用的admin文件进行编写，那么就可以将其展示在浏览器中

- 可以对页面进行操作
- 查看数据库操作是否成功
- else

```python
from django.contrib import admin

# Register your models here.
from firsttry.models import Project

admin.site.register(Project)
```


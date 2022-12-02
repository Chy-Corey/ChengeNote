### 生成csv文件

#### 定义

逗号分隔值文件(Comma-Separated Values, CSV)，有时也称问字符分割值文件，其文件以纯文本形式存储表格数据

说明：可被excel直接读取

#### Python中生存csv文件

Python提供了内建库 -csv；可直接通过此库操作csv文件

案例：

```python
import csv
with open('eggs.csv', 'w', newline='') as csvfile:
    writer = csv.writer(csvfile)
    writer.writerow(['a', 'b', 'c'])
    writer.writerow('fucku','shit','j')
```

#### csv文件下载

在网站中实现csv下载，需注意如下几点：

- 响应Content-Type类型需要修改为text/csv。这告诉浏览器该文档为CSV文件而不是html文件
- 响应会获得一个额外的Content-Disposition标头，其中包含CSV文件的名称。它将被浏览器用于开启"另存为..."对话框

代码实现：

```python
import csv
from django.http import HttpResponse
from .models import Book
def make_csv_view(request):
    response = HttpResponse(content_type='text/csv')
    response['Content-Disposition'] = 'attachment; filename="mybook.csv"'
    all_book = Book.objects.all()
    writer = csv.writer(response)   # 将文件写d
    writer.writerow(['id', 'title'])
    for b in all_book:
        writer.writerow([b.id, b.title])
    
    return response
```


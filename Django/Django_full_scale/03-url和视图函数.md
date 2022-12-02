### URL & 视图函数

---

#### URL

[统一资源定位符 - MBA智库百科 (mbalib.com)](https://wiki.mbalib.com/wiki/URL)

- 定义：Uniform Resource Locator，统一资源定位符
- 作用：用来表示互联网上某个资源的地址
- URL的一般语法格式为：`protocol://hostname[:port]/path[?query][#fragment]`中括号内可以不写，有默认值
  - fragment为锚点

#### 处理URL请求

当地址栏为：`http://127.0.0.1:8000/page/2003`

1. Django 从配置文件中 根据 `ROOT URLCONF` 找到主路由文件；默认情况下，该文件在项目同名目录下的urls.py
2. Django加载主路由文件中的urlpatterns变量
3. 依次匹配urlpatterns中的path，匹配到第一个合适的为止
4. 匹配成功后，调用对应的视图函数，返回响应
5. 匹配失败后，返回404

#### 视图函数

视图函数是用于接收一个浏览器请求对象(HttpRequest)并通过HttpResponse对象返回响应的函数。此函数可以接收浏览器请求并根据业务逻辑返回相应的相应内容给浏览器。浏览器收到一个url请求，就会去寻找对应的视图函数并调用。

- 语法

  ```python
  def xxx_view(request[parameters])：
  	...
  	return HttpResponseObject
  ```

- 用例

  ```python
  # file: <项目同名文件夹下> /views.py
  from django.http import HttpResponse
  def page1_view(request):
      html="..."
      return HttpResponse()
  ```

  


















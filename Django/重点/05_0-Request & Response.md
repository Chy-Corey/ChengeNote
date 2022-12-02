## Request & Response

我们在视图函数中进行响应请求时，要用到 request 的很多属性，比如用户的 IP  地址、请求类型(get post)、请求信息等待；在返回响应时，我们通常返回一串字符串，但是 Django 也提供了别的属性。比如可以设置响应头、状态码等。

我们要使用时，可以查阅官方文档来看有没有需要用的属性：[请求和响应对象 | Django 文档 | Django (djangoproject.com)](https://docs.djangoproject.com/zh-hans/4.1/ref/request-response/#module-django.http)



当然，Django 支持流式传输：`StreamingHttpResponse` 对象。

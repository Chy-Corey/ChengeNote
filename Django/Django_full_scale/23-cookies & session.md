## Cookies和Session

cookies和session都是保持会话状态的工具

### 什么是会话

- 从打开浏览器访问一个网站开始，到关闭浏览器结束此次访问，称为一次会话
- HTTP协议是无状态的，会导致会话状态难以保持
- Cookies和Session就是为了保存会话状态而诞生的两个存储技术

### Cookies

#### 什么是cookies

cookies是保存在客户端浏览器上的存储空间

Chrome 浏览器 可能通过开发者工具的 Application >> Storage >> Cookies 查看和操作Cookies值

火狐浏览器可通过开发者工具的 存储 >> Cookie 查看

#### Cookies特点

- cookies 在浏览器上以**键值对**的形式进行存储，键和值都是以ASCII字符串的形式存储(所以不能是中文字符串)
- 存储的数据有生命周期，即有过期时间(保质期)
- cookies中的数据是按域存储隔离的，不同的域之间无法访问
- cookies的内部数据会在每次访问此网址时都携带到服务器端，如果cookies太大会降低响应速度

##### Cookies的使用 - 存储和修改

可以调用对象`HttpResponse`里的方法`set_cookie`对cookie进行存储和修改

```python
HttpResponse.set_cookie(key, value='对应值', max_age=None, expires=None)
```

- key: cookie的名字

- value: cookie的值

- max_age: cookie存活时间，秒为单位

- expires: 具体过期时间

- 当不指定 max_age 和 expires 时(None)，关闭浏览器时此数据失效

- 存储示例：

  - 添加cookie

    ```python
    # 为浏览器添加键为 my_var1，值为123，过期时间为1h的cookie
    responds = HttpResponse("已添加 my_var1，值为123")
    responds.set_cookie('my_var1', 123, 3600)
    return responds
    ```

  - 修改cookie

    为浏览器添加键为 my_var1，值为123，过期时间为1h的cookie

    ```python
    # 为浏览器键为 my_var1的cookie值修改为456，过期时间为2h
    responds = HttpResponse("已修改 my_var1，值为456")
    responds.set_cookie('my_var1', 456, 7200)
    return responds
    ```

##### Cookies的使用 - 删除和获取

- 删除cookies

  ```python
  HttpResponse.delete_cookies(key)
  ```

  删除指定的Key的cookie，如果Key不存在则无事发生

- 获取Cookies

  通过request.COOKIES绑定的字典获取客户端的COOKIES数据

  ```python
  value = request.COOKIES.get('cookies名', '默认值')
  ```



### Session

#### 什么是Session

session是在服务器上开辟一段空间，用于保留浏览器和服务器交互时的重要数据。

实现方式：

- 使用session需要在浏览器客户端启动cookie，且在cookie中存储sessionID
- 每个客户端都可以在服务器端有一个独立的Session
- 注意：不同的请求者之间不会共享这个数据，与请求者一一对应

#### Session 初始配置

在`settings.py`中配置session

1. 向`INSTALLED_APPS` 列表中添加：

   ```python
   INSTALLED_APPS = [
       # 启动sessions应用
       'django.contrib.sessions'
   ]
   ```

2. 向 `MIDDLEWARE` 列表中添加：

   ```python
   MIDDLEWARE = [
       # 启动 Session 中间件
       'django.contrib.sessions.middleware.SessionMiddleware'
   ]
   ```

#### Session 的使用

session对象是一个类似于字典的SessionStore类型的对象，可以用类似于字典的方式对session进行操作

session 能够存储如字符串，整形，字典，列表等数据结构

- 保存session的值到服务器

  ```python
  request.session['KEY'] = VALUE
  ```

- 获取session的值

  ```python
  value = request.session['KEY']	# 强硬的
  value = request.session.get('KEY', 默认值)	# 温柔的
  ```

- 删除session

  ```python
  del request.session['KEY']
  ```

#### settings.py 中相关配置

1. SESSION_COOKIE_AGE

   作用：指定sessionID在cookies中的保存时常(默认两周)，秒为单位

   例如：`SESSION_COOKIE_AGE = 60 * 60 * 24 * 7 * 2`

2. SESSION_EXPIRE_AT_BROWSER_CLOSE = True

   设置浏览器关闭时session失效(默认为False)

注意：Django中的session数据存储在数据库中，所以**使用session前需要确保已经执行过migrate**

#### Django中session的问题

1. django_session表是单表设计，且该表数据量持续增持(浏览器删掉sessionID或者sessionID过期时不删除表里的对应数据)
2. 可以每晚执行 `python manage.py clearsessions` ，该命令可以删除国企的session数据



### Cookies和Session对比

Cookies数据存储在浏览器中，相对不安全

Session数据存储在服务器中，相对安全


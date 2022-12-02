## cookie & session

首先需要解释一下浏览器是如何工作的。我们在使用浏览器访问网页时，其实是在使用 HTTP 协议和厂商的服务器进行通讯。比如进行登陆时，我们在浏览器上输入账号和密码，点击登录；这时浏览器就会将我们输入的信息添加到 HTTP 请求中，发送到服务器中；服务器验证账号和密码，如果正确无误，则允许我们登录，进入下一个页面。

但是服务器并不能记住我们的 HTTP 请求，也就是说 HTTP是一种无状态的协议，它并不能携带我们是否登录的状态。换句话说，服务器记不住你，可能你每刷新一次网页，就要重新输入一次账号密码进行登录。

所以为了记住我们的状态(比如登录)，人们设计出了 cookie 和 session，每次我们进行登录，服务器都会返回一个**标签**给我们，这个标签服务器也拥有。那么我们只要在 HTTP 请求中携带这个标签，服务器就可以验证我们的状态了。这个标签就是 cookie 和 session。

用专业术语来说，cookie 和 session 的作用是可以保持会话，所谓会话就是浏览器和服务器间的通讯。

当然，cookie 和 session 有区别，它们是两种不同的标签。接下来讲解它们的具体原理。



### 一、cookie

cookie **存储在客户端浏览器的存储空间中**，具有以下特点：

1. 以键值对的形式存储，键和值都是 ASCII 字符串
2. 具有生命周期(一定时间后会失效)
3. cookie 按域隔离，不同的域之间无法访问(也就是不同的IP)
4. cookie 的内部数据在每次访问时都会携带到服务器端

具体的原理，我们可以看下一小节： **cookie 的存储**。

#### 1. cookie 的存储

Django 提供了设置 cookie 的方法：

```python
HttpResponse.set_cookie(key, value="",max_age=None,expires=None)
```

注意：存储和修改是同一个方法

- key: cookie 的名字

- value: cookie 的值

- max_age: 存活时间，秒为单位

- expires: 具体过期时间

  当不指定max_age & expires 时，关闭浏览器时该 cookie 失效

```python
# 视图函数
def set_cookies(request):
    response = HttpResponse('set cookies')
    response.set_cookie('uname', 'chenhongyu', 5000)
    return response
```

我们返回的 HttpResponse 对象携带了 cookie，服务器接收到返回值以后，就会从返回的信息中找到 cookie ，并保存下来。

我们可以看一下 HttpResponse的响应头：

> Content-Length: 11
> Content-Type: text/html; charset=utf-8
> Cross-Origin-Opener-Policy: same-origin
> Date: Mon, 28 Nov 2022 13:18:32 GMT
> Referrer-Policy: same-origin
> Server: WSGIServer/0.2 CPython/3.8.13
> **Set-Cookie**: username=chenhongyu; expires=Mon, 28 Nov 2022 14:41:52 GMT; Max-Age=5000; Path=/
> X-Content-Type-Options: nosniff
> X-Frame-Options: DENY

再看一下这个 cookie 被浏览器存到了哪里：

> username
>
> 名称: username
>
> 内容: chenhongyu
>
> 域: 127.0.0.1
>
> 路径: /
>
> 发送: 仅限相同站点的连接
>
> 可供脚本使用: 是
>
> 已创建: 2022年11月28日星期一 20:57:32
>
> 到期: 2022年11月28日星期一 22:20:52

可以看到，**浏览器通过读取响应头**中的 cookie 和请求头中主机的 IP 地址，**自动**将这个 cookie 存到了**域** 127.0.0.1 所在的位置，并且以键值对的键作为这条 cookie 的名字。

#### 2. cookie 的删除

```python
HttpResponse.delete_cookie(key)
```

以键值对的键为参数为返回对象提供删除 cookie 的方法，如果不存在这个 key，那么**不会报错**，什么也不会发生。

```python
# views.py
def delete_cookies(request):
    response = HttpResponse('delete cookies')
    response.delete_cookie('username')
    return response
```

#### 3. cookie 的获取

我们可以通过Http请求中 **request 对象** 的属性进行获取，具体来说就是通过 `request.COOKIES` 中的字典获取浏览器端的 cookie 数据

```python
value = request.COOKIES.get('cookie 名字', '默认值')
```

- 这里的 `cookie 名字` 就是键(key)，如果没有这个键，value 就是默认值

```python
# views.py
def get_cookies(request):
    value = request.COOKIES.git('username', 'null')
    return HttpResponse('value is %s'%(value))
```

#### 4. 总结

学到这里，我们已经知道了如何生成 cookie 并发给浏览器，浏览器会自动存储，并且在每次请求时携带在请求头中，这样服务器就能够读取 cookie。但是这个章节并没有讲如何用 cookie 来保持通讯，或者说如何用 cookie 来保持会话状态。这是因为我们还没有讲如何在服务器端存储 cookie，并和请求中的 cookie 进行对比。这涉及到服务器端的缓存、存储技术，一般用 `Redis` 来实现，后面会讲到。在这篇笔记中学习如何存储、读取 cookie 即可。

所以说，cookie 其实就是一串字符串，浏览器和服务器共同持有这一个字符串，从而可以达到会话保持的效果。但是这一方法存在安全隐患：如果浏览器被劫持、骇入，被获取了 cookie，那么就可以在另一个浏览器上伪造 cookie 实现无密码登录你的账号。

### 二、session

session 是基于 cookie 的另一种会话保持的技术。session 会在服务器上开辟一块空间，用于保留浏览器和服务器交互时的重要数据(比如账号密码)，这一块空间有一个独立的 session ID(就像超市储物柜的条形码)。浏览器会将这个 ID 返回给浏览器，浏览器以 cookie 的形式储存这个 ID。那么每次发送请求时，都会携带 ID，服务器得到 ID 后，就会去验证是否有这个 ID所对应的区域，从而实现状态保持。

- 使用 session 需要在客户端启动 cookie，且在 cookie 中存储 session ID
- 每个浏览器客户端都可以在服务器对应一个独立的 session，也就是说是一一对应的。

Django 中封装了 session 功能，会自动创建数据库。

#### 1. session 初始化配置

我们需要在 `settings.py` 中添加 session 配置

```python
INSTALLED_APPS = [
    # 启用 sessions 应用
    'django.contrib.sessions',
]

MIDDLEWARE = [
    # 启用 sessions 中间件
    'django.contrib.sessions.middleware.SessionMiddleware'
]
```

#### 2. session 使用

##### 1. 基本使用

我们知道，cookie 是一串字符串，那么 session 是什么结构呢？session对象是一个类似于字典的 SessionStore 类型的对象，可以用类似于操作字典的方式进行操作。

session 能够存储字符串、整型、字典、列表等数据

1. 保存 session 到服务器

   ```python
   request.session['KEY'] = VALUE
   ```

   这行代码不仅会将 session 数据存到服务器，还会生成 session ID 保存到服务器、返回到浏览器。

   ```python
   def set_session(request):
       request.session['username'] = 'chenhongyu'
       return HttpResponse('session setted')
   ```

2. 获取 session 

   ```PYTHON
   value = reqeust.session['KEY'] # 直接通过 key 来获取，如果没有对应的 key 会报错
   value = reqeust.session.get('KEY', 默认值) # get 方法获取，不会报错
   ```

   ```python
   def get_session(request):
       value = request.session['username']
       return HttpResponse('session value is %s' %(value))
   ```

3. 删除 session

   ```python
   del request.session['KEY']	# 删除对应 session
   request.sesion.clear()	# 清除所有 session
   request.session.flush()	# 删除 session 并且删除在浏览器中存储的 session_id ，一般在注销的时候用得比较多。
   ```

##### 2. 高级配置

可以给 session 配置保存时长( `settings.py` 中)

1. SESSION_COOKIE_AGE

   作用：设定 session ID 的保存时长，默认为 2 周，以秒为单位

2. SESSION_EXPIRE_AT_BROWSER_CLOSE = True

   值为 True 时，设定为浏览器关闭时，session ID 失效；默认为 False。

注意：Django 中 session 保存在数据库中，所以在使用 session 时，配置好需要 migrate。



#### 3. session 的一些问题

Django 中 session 存于数据库中，表中的 session 随着用户的访问会持续增加，不会自动删除过期的 session。所以可以每隔一段时间(比如每晚)执行 `python manage.py clearsessions` 删除过期数据。

也可以写 shell 脚本定时执行该命令。



### 三、session more

第二章所提及的 session 是存储于数据库的，但是这样会增加服务器的负担，因为要一直索引数据库。其实 Django 还支持其他的 session 存储方式，比如缓存、文件等待。

我们可以看一下 Django 的全局默认配置文件，找找默认的 session 配置。Django的默认配置路径是`from django.conf import global_settings`，我们可以打开查看默认配置，代码如下：

```python
############
# SESSIONS #
############

# Cache to store session data if using the cache session backend.
SESSION_CACHE_ALIAS = "default"
# Cookie name. This can be whatever you want.
SESSION_COOKIE_NAME = "sessionid"
# Age of cookie, in seconds (default: 2 weeks).
SESSION_COOKIE_AGE = 60 * 60 * 24 * 7 * 2
# A string like "example.com", or None for standard domain cookie.
SESSION_COOKIE_DOMAIN = None
# Whether the session cookie should be secure (https:// only).
SESSION_COOKIE_SECURE = False
# The path of the session cookie.
SESSION_COOKIE_PATH = "/"
# Whether to use the HttpOnly flag.
SESSION_COOKIE_HTTPONLY = True
# Whether to set the flag restricting cookie leaks on cross-site requests.
# This can be 'Lax', 'Strict', 'None', or False to disable the flag.
SESSION_COOKIE_SAMESITE = "Lax"
# Whether to save the session data on every request.
SESSION_SAVE_EVERY_REQUEST = False
# Whether a user's session cookie expires when the web browser is closed.
SESSION_EXPIRE_AT_BROWSER_CLOSE = False
# The module to store session data
SESSION_ENGINE = "django.contrib.sessions.backends.db"
# Directory to store session files if using the file session module. If None,
# the backend will use a sensible default.
SESSION_FILE_PATH = None
# class to serialize session data
SESSION_SERIALIZER = "django.contrib.sessions.serializers.JSONSerializer"
```

可以看到，`SESSION_ENGINE = "django.contrib.sessions.backends.db"`，也就是说 session 的引擎是数据库。Django 也提供了其他引擎，我们逐一了解。

#### 1. 数据库方式

该方式和第二章中的方法相同

```python
# 数据库方式（默认）：
SESSION_ENGINE = 'django.contrib.sessions.backends.db'   

# 数据库类型的session引擎需要开启此应用，启用 sessions 应用
INSTALLED_APPS = [
    'django.contrib.sessions',
]
```

#### 2. 缓存

使用缓存来存储`session`。想要将数据存储到缓存中，前提是你必须要在`settings.py`中配置好`CACHES`，并且是需要使用`Memcached`，而不能使用纯内存作为缓存。

关于缓存的使用可以查看缓存的笔记。

```python
SESSION_ENGINE = 'django.contrib.sessions.backends.cache'   
```

#### 3. 缓存+数据库

在存储数据的时候，会将数据先存到缓存中，再存到数据库中。这样就可以保证万一缓存系统出现问题，`session`数据也不会丢失。在获取数据的时候，会先从缓存中获取，如果缓存中没有，那么就会从数据库中获取。

```python
SESSION_ENGINE = 'django.contrib.sessions.backends.cached_db' 
```

#### 4. 文件

使用文件来存储`session`。

```python
SESSION_ENGINE = 'django.contrib.sessions.backends.file' 

# 设置文件位置， 默认是 tempfile.gettempdir()，
# linux下是：/tmp
# windows下是： C:\Users508\AppData\Local\Temp
SESSION_FILE_PATH = 'd:\session_dir'
```


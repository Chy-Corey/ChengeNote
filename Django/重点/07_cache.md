## cache

缓存是一类可以更快的读取数据的介质统称，也指其它可以加快数据读取的存储方式。一般用来存储临时数据，常用的介质是读取速度很快的内存。一般来说从数据库多次把所需要的数据提取出来，要比从内存或者硬盘等一次读出来付出的成本大很多。对于中大型网站而言，使用缓存减少对数据库的访问次数是提升网站性能的关键之一。

比如我们从数据库中请求了一大串数据，每次请求都需要三秒时间。我们可以在第一次请求时就将数据缓存下来，这样就可以加快请求的速度。

Django 中缓存的优化思想：

```python
get a URL, try finding the page corresponding to the URL in the cache
if the page is in the cache:
	return the cache page
else:
    generate the page
    save the generated page in the cache(for next time)
    return the generated page
```

如果要使用缓存，需要保证数据变动频率较低。



### 一、缓存的配置

Django中提供了多种缓存方式，如果要使用缓存，需要先在`settings.py`中进行配置，然后应用。根据缓存介质的不同，你需要设置不同的缓存引擎 Back end 。在生产环境中最常用的缓存是 Memcached 和 Redis 。在开发环境中可以使用本地内存缓存进行测试。

#### 1. 存储到数据库中

这相当于省略了数据索引、过滤的过程，降低了查询的复杂度。

```python
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.db.DatabaseCache',	# 配置引擎为数据库缓存
        'LOCATION': 'my_cache',	# 存储到哪张表里
        'TIMEOUT': 300,	# 缓存保存的时间，单位为秒，默认为300
        'OPTIONS': {
            'MAX_ENTRIES': 300,	# 缓存最大数据量(条)
            'CULL_FREQUENCY': 2	# 达到最大数据量时，删除 1/x 的缓存数据(这里是 1/2)
        }
    }
}
```

#### 2. 存储到服务器的内存

一般只有测试时才使用这种配置。

```python
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.locmem.LocMemCache',
        'LOCATION': 'unique-snowflake'	# 内存寻址算法：雪花算法
    }
}
```

#### 3. 存储到 Redis 数据库

Redis 是当今速度最快的内存型非关系型（NoSQL）型数据库。Redis不仅仅支持简单的 key-value 类型的数据，同时还提供 list ，set ，z set ，hash 等多种数据结构的存储。与 Memcached 相比，Redis 不仅支持支持缓存数据在硬盘上的持久化，还支持 master-slave 模式的数据备份，有明显的优点。

一般我们使用 Redis 缓存。

```python
CACHES = {
    'default': {
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': 'redis://your_host_ip:6379', # redis所在服务器或容器ip地址
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
             "PASSWORD": "your_pwd", # 你设置的密码
        },
    },
}
REDIS_TIMEOUT = 7*24*60*60
CUBES_REDIS_TIMEOUT = 60*60
NEVER_REDIS_TIMEOUT = 365*24*60*60
```



### 二、缓存的使用

当你做好有关缓存(Cache)的设置后，在Django项目中你可以有两种方式使用Cache。

- 整体缓存
- 局部缓存

所谓整体缓存，其实就是把视图函数的返回值全部缓存；而局部缓存表示只缓存指定的值，比如请求的数据库的值。

#### 1. 整体缓存

整体缓存可以用在**视图**函数和 **URL** 中，需要导入装饰器 `cache_page`：

```python
# views.py
# 视图函数需要导入装饰器
from django.views.decorators.cache import cache_page

@cache_page(30)	# 单位为秒，即有效时间30秒
def my_view(request):
    #...
    
    
# urls.py
from django.views.decorators.cache import cache_page

urlpatterns = [
    path('foo/', cache_page(30)(my_view))
]
```

可以看到，整体缓存是将整个视图函数的返回值缓存下来

#### 2. 局部缓存

局部缓存也是需要使用 Django 提供的方法

首先我们需要了解一下 `settings.py` 中缓存的设置：

```python
CACHES = {
    'default': {
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': 'redis://your_host_ip:6379', # redis所在服务器或容器ip地址
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
             "PASSWORD": "your_pwd", # 你设置的密码
        },
    },
    'db_cache': {
        'BACKEND': 'django.core.cache.backends.db.DatabaseCache',	# 配置引擎为数据库缓存
        'LOCATION': 'my_cache',	# 存储到哪张表里
        'TIMEOUT': 300,	# 缓存保存的时间，单位为秒，默认为300
        'OPTIONS': {
            'MAX_ENTRIES': 300,	# 缓存最大数据量(条)
            'CULL_FREQUENCY': 2	# 达到最大数据量时，删除 1/x 的缓存数据(这里是 1/2)
        }
    }
}
```

比如以上配置了两种缓存，Django 会将我们的配置写为对象，每个缓存类型对应一个对象，那么我们就可以获取这些缓存类型的对象：

```python
from django.core.cache import caches
cache1 = caches['default']
cache2 = caches['db_cache']
```

当然，也可以直接导入默认缓存项(default)：

```python
from django.core.cache import cache
# cache 就是 default 对象
```

获取到缓存对象以后，我们就可以调用 api 来存、取缓存

1. `cache.set(key, value, timeout)`
   - key: 字符串类型
   - value: 对象类型
   - timeout: 秒为单位，默认值为 `settings.py` 中的值
2. `cache.get(key)`
   - 返回值为 key 对应的值，如果没有则返回 None
3. `cache.add(key, value)`
   - 只有当 key 不存在时才能生效
   - 返回值为True 或 False
4. `cache.get_or_set(key, value, timeout)`
   - 如果不能 get ，则 set ，返回值为 value
5. `cache.set_many(dict, timeout)`
   - dict: key & value 的字典
   - 返回值为插入不成功的 key 的数组
6. `cache.get_many(key_list)`
   - key_list: key 的数组
   - 返回值：获取到的字典
7. `cache.delete(key)`
   - 返回值 None
8. `cache.delete_many(key_list)`
   - 返回值 None



### 三、浏览器缓存

不仅可以将缓存存储到服务器，还可以存储到浏览器本地。

#### 1. 强缓存

强缓存表示浏览器在拥有缓存时，不会向服务器发送请求，而是直接从缓存中读取资源，强缓存是由响应决定的。如果响应头中包含强缓存的命令，浏览器会知晓并将数据缓存到浏览器本地。

**如果我们使用装饰器，也就是整体缓存，装饰器会自动帮我们填充响应头，增加强缓存，不需要我们手动修改**。

如果我们使用的是局部缓存，那么我们要手动修改响应头。

强缓存的响应头：

1. Expires

   样例：`Expires: Tuh, 02 Apr 2030 05:14:08 GMT`

   定义：表示缓存过期的时间，为绝对时间

2. Cache-Control

   样例：`Cache-Control: max-age=120`

   定义：多少秒后缓存过期，为先对时间

浏览器优先使用 Cache-Control

#### 2. 协商缓存

强缓存中，数据一旦过期，就要重新请求，但是如果原来的数据没变，就没必要重新请求，所以当缓存到期时，可以和服务器协商一下，看一下当前缓存是否可用，如果可用就没必要再请求了。

协商缓存的指令也存在于响应头/请求头中：

1. Last-Modified 响应头 & If-Modified-Since 请求头

   1. Last-Modified 为文件最近修改时间，浏览器第一次请求静态文件时，如果返回了 Last-Modified 响应头，则表示该资源为需要协商的缓存
   2. 缓存到期后，浏览器将 Last-Modified 的值作为请求头 If-Modified-Since 的值发过去，和服务器协商。服务器返回 304 则表示缓存可用，返回 200 则表示不可用，需要使用响应体中的资源。

   这种方法通过时间来决定缓存是否到期，但是这**并不严谨**，因为时间只能精确到秒，最严谨的是计算资源的 Hash 值(唯一标识)，用这个来判断是否到期，也就是第二种请求、响应头。

2. Etag 响应头 & If-None-Match 请求头

   1. ETag 是服务器响应请求时，返回当前资源的唯一标识
   2. 缓存到期后，浏览器将 ETag 的值作为 If-None-Match 的值发送给服务器协商。服务器进行对比，如果可用则返回 304，不可用则返回 200，需要使用响应体中的资源。
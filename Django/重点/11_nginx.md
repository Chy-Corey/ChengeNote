## nginx

我们已经使用 uWSGI 启动了 Django 应用程序，但是当请求量过大时，依然会令程序崩溃。这并不是 uWSGI 的问题，是因为服务器的硬件性能总是局限的。所以一般的解决方案是：多台机器同时运行 Django 程序，同时作为服务器。

但是 uWSGI 的功能只是搭建 Web 和 Django 的桥梁，并不会把请求分发到不同的机器上。所以我们需要一个可以将请求进行分发(也就是负载均衡) 的功能，这就是 nginx 可以提供的。 



### 一、什么是 nginx

nginx 是轻量级的高性能 Web **服务器**，提供了 HTTP 代理和反向代理、负载均衡等一系列重要特性

nginx的作用：

- 负载均衡，多台服务器轮流处理请求
- 反向代理

原理：客户端请求 nginx，再由 nginx 将请求转发给由 uWSGI 运行的 Django。

#### 1. 什么是正向代理 & 反向代理

##### 正向代理

正向代理，就是一个位于客户端和原始服务器之前的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并且指定目标（原始服务器），然后代理向原始服务器转交请求并将获得的内容返回给客户端。

##### 反向代理

反向代理服务器位于用户与目标服务器之间，但是对于用户而言，反向代理服务器就相当于目标服务器，即用户直接访问反向代理服务器就可以获得目标服务器的资源。同时，用户不需要知道目标服务器的地址，也无须在用户端作任何设定。反向代理服务器通常可用来作为Web 加速，即使用反向代理作为Web服务器的前置机来**降低网络和服务器的负载**，提高访问效率。

反向代理方式是指以代理服务器来接收 Internet 网上的连接请求，然后将请求转发给内部网络上的服务器，并从服务器上得到的结果返回给 Internet 上请求连接的客户端，此时代理服务器对外就表现为一个节点服务器。
#### 2. 什么是负载均衡

负载均衡就是可以制定反向代理时转发请求的策略，比如我们有两个服务器 A 和 B ，可以使用 nginx，决定请求如何分发给 A 和 B 。



### 二、配置 nginx

修改 nginx 的配置文件 `/nginx/sites-enabled/default`

在 server 节点下添加新的 location 项，指向 uwsgi 的 IP 与端口

```nginx
server {
    ...
        location / {
        uwsgi_pass 127.0.0.1:8000; # 重定向到 uWSGI 服务的地址
        include /etc/nginx/uwsgi_params; # 将请求协议转 uwsgi 协议
    }
    ...
}
```



### 三、启动/停止

```shell
sudo /etc/init.d/nginx start|stop|restart|status
# 或者
sudo service nginx start|stop|restart|status
```

注意：只要修改了配置，就需要重启



### 四、uWSGI 的修改

我们在 HTTP 和 uWSGI 之间增加了一层 nginx，nginx 转发的是 uwsgi 协议，所以我们要修改 uWSGI。

```ini
[uwsgi]
# http=127.0.0.1:8000 将之前的 http 改为 socket
socket=127.0.0.1:8000
```

记得要关闭 uWSGI 并重新启动



### 五、nginx + uWSGI 排错

排查问题时，一般是通过**看日志**来查询问题来源

<h4>学会看日志非常重要！！！</h4>

- nginx 日志位置：

  异常信息：`/var/log/nginx/error.log`

  正常访问信息：`/var/log/nginx/access.log`

- uwsgi 日志位置：

  `uwsgi.log`

#### 1. 常见问题

1. 返回 502

   这表示 nginx 启动成功，但是 uWSGI 未启动成功

2. 返回 404

   有两种可能：

   1. 请求路由错误，即 Django 中没有该请求
   2. nginx 配置错误，未禁掉 nginx 配置中的 `try_files`



### 六、nginx 静态文件配置

当`settings.py` 中 `debug = False` 时，Django 会找不到静态文件(因为这么找影响性能，Django 放弃了测试时的方式)。所以我们需要告诉 nginx 静态文件在哪里，直接让 nginx 来处理静态文件。

#### 1. 配置步骤

1. 在项目目录下创建一个新的路径，由于存放所有的静态文件，如：`/mysite/static`

2. 在 Django `settings.py` 中添加新配置

   ```python
   STATIC_ROOT = os.path.join(BASE_DIR,'mysite/static')
   ```

3. 执行 `python manage.py collectstatic` ，导出所有静态文件到 `STATIC_ROOT`。

4. 在 nginx 的配置文件中添加新配置

   ```shell
   server{
   	...
   	location /static {
   		# root 告诉 nginx 静态文件夹的绝对路径
   		root /home/.../static;
   	}
   }
   ```

   

### 七、一些生产环境下的配置

#### 1. 自定义报错页面

在模板文件夹内添加 `404.html` 模板，当视图触发 HTTP 404 异常时显示

`404.html` 仅在发布版中(`debug = False`) 起作用，当触发 404 异常时跳转 404 界面

#### 2. 邮箱报警

当正式服务器上代码运行出错时，可以将错误追溯信息发送到后台管理者的邮箱。

```python
# settings.py
DEBUG = False

# 错误接收方
ADMINS = [('chenge', 'xxxxx@qq.com'), ('siyu', 'xxxxx@qq.com')]
# 发送邮件的账号
SERVER_EMAIL = 'xxxxx@qq.com'
```

报错邮件需要过滤敏感信息，Django 封装了这个功能，可以屏蔽局部变量和POST数据，把对应的数据变成 *

```python
from django.views.decorators.debug import sensitive_variables
from django.views.decorators.debug import sensitive_post_parameters


# 局部变量屏蔽
@sensitive_variables('user', 'pw', 'cc')
def process_info(user):
    pw = user.pass_word
    cc = user.credit_card_number
    name = user.name
    # 将 user 对象中赋值的 pw 和 cc 屏蔽
    
# 屏蔽 post 数据
@sensitive_post_parameters('password', 'username')
def index(request):
    message = request.POST['username'] + request.POST['password']
    ...
```

使用局部变量装饰器时，如果有多个装饰器，需要将其置顶，如果不传参，就会屏蔽所有局部变量。

以上的装饰器不仅对于发送邮件有用，而是在调用含有这两个装饰器的函数时，如果报错，会将报错信息中的敏感信息屏蔽。
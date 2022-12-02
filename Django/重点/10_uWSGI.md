

## uWSGI

[uWSGI的安装及配置详解 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/366440889)

在测试 Django 项目时，通常使用 `python manage.py runserver` 来启动服务，runserver 就是一个网关，内含大量的接口用来匹配各种网络协议。说白了，runserver 就像是一座桥梁，一边连着web服务器，另一边连着Python的应用程序Application(比如 Django)。

但是项目正式上线时，runserver 的性能将不足以支撑庞大的请求数量，所以需要用更强大、稳定的网关接口，也就是 uWSGI，它的稳定性远强于 runserver。所以我们要学习如何使用 uWSGI 来连接 Web 服务器和 Django应用。



### 一、WSGI 定义

WSGI(Web Server Gateway Interface)，Web 服务网关接口，是 Python 应用程序和 Web 服务器之间的一种接口。因为 Web 服务的语言和 Python 应用的语言不同，所以他们两个无法对话交流，所以设计出了 WSGI接口，用来让他们能够进行交流对话，也就是可以让 Python 应用实现 Web 服务。

我们使用的 `python manage.py runserver` 其实就是启动了一个工具，这个工具可以将 HTTP 协议和 WSGI 协议连接起来，这样 Django 就可以进行 Web 服务了。

HTTP <----> runserver <----> WSGI <----> Django

---

**uWSGI** 是 WSGI 的一种，实现了 HTTP 协议、 WSGI 协议、uwsgi 协议等等。其功能十分强大，可以用它将 Web 和 Django 连接。

uWSGI 只支持 Linux 系统，不能在 Windows 系统安装。



### 二、配置 uWSGI

#### 建立配置文件

添加 uWSGI 配置文件(`uwsgi.ini`)，文件路径在根目录的项目项目同名文件夹下(和 `settings.py` 平级)：`项目同名文件夹/uwsgi.ini`

如：`mysite/mysite/uwsgi.ini`

#### 编写配置文件

文件需要以 `[uwsgi]` 开头，然后开始写配置项：

1. 套接字方式的 IP 地址：端口号 (需要 nginx)

   ```ini
   socket = 127.0.0.1:8000
   ```

2. HTTP 方式的 IP 地址: 端口号

   ```ini
   http = 127.0.0.1:8000
   ```

3. 项目当前的工作目录

   ```ini
   chdir = .../mysite

4. 项目中 `wsgi.py` 文件的路径，是相对路径(相对于工作目录)(这个文件 Django 已经帮我们创建好了)

   ```ini
   wsgi-file=mysite/wsgi.py

5. 进程个数

   ```ini
   process=5

6. 每个进程的线程个数

   ```ini
   threads=2
   ```

7. 服务的 pid 记录文件位置

   ```ini
   pidfile=uwsgi.pid
   ```

8. 服务的日志文件位置(以后台守护进程运行，并将log日志存于temp文件夹，不将日志输出到终端)

   ```ini
   daemonize=uwsgi.log
   ```

9. 开启主进程管理模式

   ```ini
   master=true
   ```

举例：

```ini
[uwsgi]
http = 127.0.0.1:8000
chdir = .../mysite
wsgi-file=mysite/wsgi.py
process=4
threads=2
pidfile=uwsgi.pid
daemonize=uwsgi.log
master=true
```



### 三、uWSGI运行

1. 启动 uWSGI

   ```shell
   # 先要 cd 到 uWSGI 配置文件所在的目录
   uwsgi --ini uwsgi.ini
   ```

2. 关闭 uWSGI

   ```shell
   # 先要 cd 到 uWSGI 配置文件所在的目录
   uwsgi --stop uwsgi.pid
   ```

---

注意：

- 无论是启动还是关闭，都需要执行 `ps aux|grep 'uwsgi'` 确认是否符合预期
- 启动成功后，进程在后台执行，所有日志均输出在配置文件所在目录的 `uwsgi.log` 中
- Django 中代码有任何修改都需要重新启动 uWSGI



### 四、uWSGI 常见问题

1. 启动失败

   原因：端口被占用，有其他进程占用 uWSGI 的启动端口

   解决方案：执行 `sudo lsof -i: 端口号` 查明具体进程，并 kill 该进程 (`sudo kill -9 pid`)。

2. 停止失败

   原因：重复启动 uWSGI，导致 pid 文件中的进程号失准

   解决方案：ps 出 uWSGI 进程，手动 kill 掉


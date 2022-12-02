### uWGSI

uWSGI是一个Python Web服务器,它实现了WSGI协议、uwsgi、http等协议，常在部署Django或Flask开发的Python Web项目时使用，作为连接Nginx与应用程序之间的桥梁。本章总结了uWSGI服务器的作用以及在部署Python Web项目时如何安装和配置uWSGI。

**为什么需要uWSGI?**

在生产环境中部署Python Web项目时，uWSGI负责处理Nginx转发的动态请求，并与我们的Python应用程序沟通，同时将应用程序返回的响应数据传递给Nginx。

```text
客户端 <-> Nginx <-> uWSGI <-> Python应用程序(Django, Flask)
```

或许你要问了，Nginx本身就是Web服务器，我们为什么还需要uWSGI这个Web服务器呢? Django不是自带runserver服务器?Flask不是自带Werkzeug吗? 答案是Nginx处理静态文件非常优秀，却不能直接与我们的Python Web应用程序进行交互。Django和Flask本身是Web框架，并不是Web服务器，它们自带的runserver和Werkzeug也仅仅用于开发测试环境，生产环境中处理并发的能力太弱。

为了解决Web 服务器与应用程序之间的交互问题，就出现了Web 服务器与应用程序之间交互的规范。最早出现的是CGI,后来又出现了改进 CGI 性能的FasgCGI，Java 专用的 Servlet 规范。在Python领域，最知名的就是WSGI规范了。

为了解决Web 服务器与应用程序之间的交互问题，就出现了Web 服务器与应用程序之间交互的规范。最早出现的是CGI,后来又出现了改进 CGI 性能的FasgCGI，Java 专用的 Servlet 规范。在Python领域，最知名的就是WSGI规范了。

WSGI 全称是 Web Server Gateway Interface，也就是 Web 服务器网关接口，是一个web服务器（如uWSGI服务器）与web应用（如用Django或Flask框架写的程序）通信的一种规范。WSGI包含了很多自有协议，其中一个是uwsgi，它用于定义传输信息的类型。

现在你清楚uWSGI, WSGI和uwsgi的区别了吗?

\- uWSGI是Python Web服务器，实现了WSGI通信规范和uwsgi协议；

\- WSGI全名Web Server Gateway Interface，是一个Web服务器（如uWSGI服务器）与web应用（如用Django或Flask框架写的程序）通信的一种规范；

\- uwsgi是WSGI通信规范中的一种自有协议。
## Docker 基础



### 一、Docker 基本组成

Docker 的架构包括：镜像(Image) 、容器(Container) 、仓库(Repostory) 和 Dockerfile。

1. image

   镜像是一个静态文件，内部包含了**应用程序及其依赖**。可以把镜像理解为一个模板，用来告诉系统怎么运行其中的程序。也可以把镜像理解为一个 Docker 环境下的可执行文件。

2. container

   容器就是用来运行镜像程序的，Docker 会开辟一个独立的“容器”，用来运行镜像程序。每个容器就像一个独立的系统，但是它们占用的内存很低。

3. repostory

   仓库就是存放镜像的地方，有共有仓库和私有仓库，其实就像是 Git hub，只不过仓库里只存储了适用于 Docker 的可执行文件。

4. dockerfile

   Dockerfile 是一个用来构建镜像的文本文件。写程序需要源代码，那么“写”image就需要dockerfile，dockerfile就是image的源代码，docker就是"编译器"。



### 二、启动并测试 Docker

安装完 docker 以后，可以输入命令 `docker -v` 来测试是否安装成功。

在终端中运行 `docker run hello-world` ，如果成功，则说明可以运行 docker。

我们可以使用命令：

```shell
docker images
```

查看一共有哪些镜像。



### 三、run 的流程

我们在执行 `docker run hello-world` 时，会看到终端有这样的输出：

```shell
C:\Users\23147>docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete
Digest: sha256:faa03e786c97f07ef34423fccceeec2398ec8a5759259f94d99078f264e9d7af
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

第一行 `Unable to find image 'hello-world:latest' locally` 表示没有从本地的镜像中找到 `hello-world` 镜像文件

接下来就开始去 Docker Hub 上找，也就是仓库(repostory)，找到以后就开始下载这个镜像到本地，也就是拉取(pull)，下载完成后，创建容器开始运行。

所以，我们可以概括一下 `docker run` 命令的运行流程：

1. 在本机寻找该镜像，如果有，则创建容器运行；如果没有则到远程仓库下载
2. 在远程仓库下载完成后，创建容器运行该项目。
3. 如果远程仓库也没有，则报错。

#### 

### 三、Docker  基础命令

#### 1. 帮助命令

能够经常使用帮助命令对于掌握 Docker 的帮助特别大，要熟练使用它们

```shell
docker version			  # 显示 docker 的版本信息
docker info				  # 显示 docker 的系统信息，比如镜像和容器的数量等
docker (command) --help   # 显示对应命令的帮助信息
```

也可以查阅官方文档学习如何使用：[Docker Documentation | Docker Documentation](https://docs.docker.com/)

以下的所有命令都可以在文档里找到

#### 2. 镜像命令

##### 1. docker images 查看本机镜像

```shell
C:\Users\23147>docker images
REPOSITORY       TAG       IMAGE ID       CREATED         SIZE
neosmemo/memos   latest    145012dd215c   3 days ago      24.7MB
hello-world      latest    feb5d9fea6a5   14 months ago   13.3kB

# REPOSITORY 镜像的仓库源
# TAG		 镜像的标签
# IMAGE ID   镜像的 ID
# CREATED    镜像的创建时间
# SIZE       镜像的大小
```

可选项：

1. -a, -all

   列出所有镜像

2. -q, -quiet

   只显示镜像 ID

##### 2. docker search 搜索镜像

搜索镜像可以到 Docker Hub 上搜索，也可以直接在终端使用命令搜索

```shell
C:\Users\23147>docker search django
NAME                                  DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
django                                DEPRECATED; use "python" instead                1167      [OK]
alang/django                          This image can be used as a starting point t…   37                   [OK]
camandel/django-wiki                  wiki engine based on django framework           33                   [OK]
```

可选项：

1. --filter=STARS=1000

   过滤搜索出来的镜像的星星数大于1000

##### 3. docker pull 下载镜像

可以指定下载的版本：`docker pull 镜像名:tag`

```shell
# 下载最新版本就是 latest
C:\Users\23147>docker pull mysql:latest
latest: Pulling from library/mysql
996f1bba14d6: Pull complete	# 分层下载
a4355e2c82df: Pull complete
a9d7aedb7ad7: Pull complete
24ee75d8667d: Pull complete
da8c1ec8ff26: Pull complete
ea8748759282: Pull complete
e0859d5816ee: Pull complete
26e144df551b: Pull complete
9878df6a0cc3: Pull complete
b43b187428e3: Pull complete
202e454031c6: Pull complete
Digest: sha256:66efaaa129f12b1c5871508bc8481a9b28c5b388d74ac5d2a6fc314359bbef91	# 签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest	# 镜像名对应的真实地址
```

`docker pull mysql` 等价于 `docker pull docker.io/library/mysql:latest`

##### 4. docker rmi 删除镜像

rmi 就是 remove image 的意思

一般通过指定 ID 的形式删除镜像，可以用 `docker images` 查看 ID。

```shell
docker rmi -f imageID	# 删除指定容器
docker rmi -f imageID1 imageID2 imageID3	# 删除多个容器
docker rmi -f $(docker images -aq)	# 删除全部容器
```

#### 3. 容器命令

要启动容器，首先需要有镜像，所以我们可以下载一个 centos 镜像来学习容器命令。

```shell
docker pull centos
```

##### 1. docker run 新建容器并启动

```shell
docker run [parameters] imageName

# 参数说明
--name="containerName"	# 给容器命名
-d	# 后台方式运行
-it	# 使用交互式方式运行容器，直接进入容器内
-p	# 指定容器的端口
	-p 主机端口:容器端口
	-p 容器端口
	-p ip:主机端口:容器端口
	容器端口
-P	# 随机指定端口
```

启动容器测试：

```
C:\Users\23147>docker run -it --name="centos" -p 921:921 centos
[root@457836707d9a /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@457836707d9a /]# exit
exit
```

`exit` 为退出容器命令。

如果想退出但不结束容器：`CTRL+P+Q`

**可能出现的问题**：

Docker容器后台运行(-d),就必须有一个前台进程，容器运行的命令如果不是那些一直挂起的命令（比如运行top，tail），就会自动退出。
这个是 docker 的机制问题,比如你的 web 容器,我们以 nginx 和 fpm 为例,
正常情况下,我们配置启动服务只需要启动响应的 service 即可,例如

```shell
service nginx start && service php5-fpm start
```

但是,这样做, nginx 和 fpm 均为后台进程模式运行,就导致 docker 前台没有运行的应用,
这样的容器,后台启动后,会立即自杀,因为他觉得他没事可做了。

要解决这个问题，可以先交互运行，再 `CTRL+P+Q`。或者在后台启动时，添加一段命令。

##### 2. docker ps 打印容器

```shell
docker ps
	 # 打印正在运行的容器
-a	 # 打印所有容器
-n=? # 打印?个最近创建的容器
-q	 # 只显示 ID
```

##### 3. docker rm 删除容器

```shell
docker rm containerID	 # 删除指定ID容器，但是不能删除正在运行的
docker rm -f containerID # 强制删除指定ID容器，能删除正在运行的
docker rm -f $(docker ps -ap)	# 删除全部容器
```

##### 4. 容器启动和停止操作

```shell
docker start containerID	# 启动容器
docker restart containerID	# 重启容器
docker stop containerID	    # 停止容器
docker kill containerID	    # 强行停止容器
```



### 四、Docker 常用命令

#### 1. docker logs 查看日志

```shell
docker logs 
	-t	# 显示时间
	-f	# 在线更新日志
	-tail num	# 设置打印日志的条数
```

#### 2. docker top 查看容器内部进程

```shell
docker top containerID
```

#### 3. docker inspect 获取容器/镜像的元数据。

```shell
docker inspect
	-f # 用模板格式打印
	-s # 显示总的文件大小
```

#### 3. 进入当前正在运行的容器

##### 1. docker exec 方式一

docker exec 进入容器后，会开启新的进程，也就是说我们在使用该命令进入容器时，需要指定程序，不能没有应用运行

```shell
docker exec container ID
# 其参数和 docker run 类似
```

##### 2. docker attach 方式二

docker attach 进入容器后，会直接进入原来运行的程序，不会开启新的进程

```shell
docker attach containerID
```

#### 4. docker cp 从容器内拷贝文件到主机

```shell
docker cp containerID:容器内文件路径 主机路径
```

该命令可以实现容器内外的传递，学了更多的功能可以进行自动拷贝(-v 数据卷)

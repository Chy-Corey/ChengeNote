# Git学习文档（本地版）

### 1. 学习目标

---

<img src="E:\Storage\博客\Typora\image\Git版本控制\学习目标.jpg" alt="学习目标" style="zoom: 33%;" />

### 2. 版本控制

---

​	版本控制是一种记录若干文件内容变化，以便将来查阅特定版本修订情况的系统，简单讲就是备份和记录，接下来我们要了解三种不同版本控制的发展历程。

#### 2.1. 本地版本控制系统

​	人们把项目拷贝到本地磁盘上备份，然后以命名方式来区分。这种方法好处是简单，但是坏处非常多，比如容易混淆版本之间的区别，不能多人同时开发等。为了解决版本之间容易混淆的问题，人们开发了一个本地版本管理系统，结构图如下：

![本地版本控制](E:\Storage\博客\Typora\image\Git版本控制\本地版本控制系统.png)

​	系统记录文件每次的更新，可以对每个版本做一个快照，或是记录补丁文件，适合个人用，如RCS。

> 系统快照就是把系统某个状态下的各种数据记录在一个文件里，就如同人照相一样，相片显内示的是你那个时容间的一个状态。系
>
> 系统快照就是系统的“照片”，虚拟机制作了系统快照后就不用启动虚拟系统了，直接恢复快照就行了，你制作快照的时候，系统什么状态，回复后就是什么状态，包括你打开的软件的状态。

#### 2.2. 集中化版本控制系统

​	本地版本控制系统可以将不同版本的文档保存下来，并借助版本记录可以很方便地定位文件。但是，不同系统的开发者很难通过本地版本控制系统协同工作。于是，集中化的版本控制系统(Centralized Version Control System，简称CVCS)应运而生。这类系统，如CVS, Subversion以及Perforce等，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端链接到这台服务器，取出对应版本的文件或者提交更新。

 ![集中化版本控制系统](E:\Storage\博客\Typora\image\Git版本控制\集中化版本控制系统.png)

​	这样做的好处是解决了开发协同的问题，但是把所有代码提交到一台服务器上有一个明显的问题就是单点故障。如果服务器宕机了，那么所有人都不能提交和下载文件，如果服务器磁盘发生故障，数据则全部丢失。

#### 2.3. 分布式版本控制系统

​	为了解决集中化版本管理的问题，产生了分布式管理系统（Distributed Version Control System，简称DVCS）。在这类系统中，如Git， Mercurial， Bazaar以及Darcs等，客户端不只是提取出最新版的文件快照，而是把最原始的代码仓库镜像到本地。这样一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的 本地仓库恢复。因为每一次的提取操作，实际上都是一次对代码仓库的完整备份。

![分布式版本控制系统](E:\Storage\博客\Typora\image\Git版本控制\分布式版本控制系统.png)

​	如图所示，客户端也存储了所有的版本记录。

### 3. Git安装(Windows)

---

​	在Windows上使用Git，直接从Git官网下载[安装程序]([Git (git-scm.com)](https://git-scm.com/))，选择对应系统下载，然后安装即可。安装完成后，在开始菜单里找到"Git" -> "Git Bash"，显示出类似命令行的窗口，说明Git安装成功。然后可以在窗口内输入 git --version 查看git版本信息。



![Git安装](E:\Storage\博客\Typora\image\Git版本控制\Git安装.png)

> Git Bash 是命令窗口
>
> Git Gui 是图形化界面

​	在使用Git工作之前，我们需要对Git进行配置，方便后续Git能跟踪到谁做了修改。我们需要设置对应的用户名与邮箱地址：

```js
git config --global user.name "myUsername"	// 设置用户名
git config --global user.email myEmail	// 设置邮箱
git config --list	// 查看配置
```

### 4. Git文件的三种状态与工作模式

---

使用Git操作文件时，文件的状态有以下三种：

|       状态        |                             描述                             |
| :---------------: | :----------------------------------------------------------: |
| 已提交(committed) | 已提交表示数据已经安全的保存在本地数据库（本地Git仓库）中。  |
| 已修改(modified)  |         已修改表示修改了文件，但还没保存到数据库中。         |
|  已暂存(staged)   | 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。 |

针对文件的三种状态，本地Git有三个区域： 工作区、暂存区和Git仓库。

|  分类   |                             描述                             |
| :-----: | :----------------------------------------------------------: |
| 工作区  |     简单理解为在电脑里可以看到的目录，比如本地项目的目录     |
| 暂存区  | Git版本库里存了很多东西，其中最重要的就是称为stage或者index的暂存区，还有Git自动创建的第一个分支master，以及指向master的一个指针叫做HEAD。 |
| Git仓库 | 工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本仓库。 |

基本的Git工作流程如下：

- 在工作区中修改某些文件。
- 对修改后的文件进行快照，然后添加到暂存区。
- 提交更新，将保存在暂存区域的文件快照永久转储到Git仓库中。

流程图如下：

<img src="E:\Storage\博客\Typora\image\Git版本控制\Git工作流程.png" alt="Git工作流程图" style="zoom: 80%;" />

### 5. 创建版本库并提交文件

---

​	版本库又叫仓库，可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理，每个文件的修改、删除，Git都可以跟踪，以便于追踪版本变化。接下来通过一个案例来学习Git的基本操作。

<font color = "#FF7F50" size = 3>例子：将一个文本文件提交到Git仓库</font>

- 初始化Git本地仓库

  通过执行<font color=red> git init </font>命令在本地初始化一个Git仓库，执行该命令后会在本地初始化一个没有任何文件的空仓库。

  <img src="E:\Storage\博客\Typora\image\Git版本控制\初始化本地仓库.png" alt="初始化本地仓库" style="zoom: 80%;" />

  注意：可以通过dos命令修改文件目录，也可以在目标文件夹下打开Git Bash。

- 新建文件test.txt并添加到暂存区。

  通过执行<font color=red> git add </font>命令将新建的文件添加到暂存区。通过执行<font color=red> git status </font>命令检查暂存区情况。

  <font color=red> 注意： </font>可以使用 git add . 来表示上传当前目录所有文件

  - 未使用 git add 命令时使用 git status：<img src="E:\Storage\博客\Typora\image\Git版本控制\git status(未使用add).png" alt="未使用add" style="zoom:100%;" />

  - 已使用 git add 命令时使用 git status：![已使用add](E:\Storage\博客\Typora\image\Git版本控制\git status(已使用add).png)

- 将文件提交到本地仓库

  通过执行<font color=red> git commit -m </font>命令将暂存区的文件提交到仓库。（单引号里的是提交的注释）

  - 使用 git commit 提交：

    ![提交成功](E:\Storage\博客\Typora\image\Git版本控制\git commit.png)

  - 使用git commit后使用git status：

    ![git status(提交后)](E:\Storage\博客\Typora\image\Git版本控制\git status(提交成功后).png)



- 查看提交日志信息

  通过执行<font color=red> git log </font>命令查看提交日志。

  ![git log](E:\Storage\博客\Typora\image\Git版本控制\git log.png)

  

### 6. 时光穿梭机

---

​	企业中在多人开发项目的环境下，使用Git版本控制工具对项目版本进行管理时，通常会对项目不同版本的文件进行查阅。项目历史版本、未来版本的切换操作，对于一个开发人员来说，这些Git命令时必要的。

#### 6.1. 修改文件与文件提交

##### 6.1.1. 修改与提交

- 将文件修改后再使用 git status 检查暂存区：

![修改后的暂存区](E:\Storage\博客\Typora\image\Git版本控制\git status(修改文件后).png)

​		如上图所示，修改后并未上传到暂存区。也可以使用git restore命令去取消这次改变。

- 使用 git add . 命令提交，再用 git status 检查：

  ![提交后检查](E:\Storage\博客\Typora\image\Git版本控制\git status（修改后再add）.png)

- 使用 git commit -m 上传到仓库，再用 git status 检查：

  ![commit](E:\Storage\博客\Typora\image\Git版本控制\git status(修改后再add再commit).png)

- 使用git log 查看日志：

  ![log](E:\Storage\博客\Typora\image\Git版本控制\git log(修改后).png)

##### 6.1.2.  查看git本地库与工作区文件的区别

​	***此功能用于查看是否有文件修改后未上传***

- 使用<font color=red> git diff HEAD -- "fileName" </font>把工作区文件与git仓库内容进行比较。运行结果如下：

  ![检查版本区别](E:\Storage\博客\Typora\image\Git版本控制\git diff HEAD.png)

  接下来对图中信息进行说明：

  ‘---’ ：表示变动前的文件

  ‘+++’：表示变动后的文件

  ‘@@……@@’：表示文件变动的位置(用行号来表示变动位置)

  ​						 图中1,2表示变动前是第1行连续有2行；1,3表示变动后是第1行连续有3行。

  可以判断：修改后并没有提交，存在未提交到git仓库的版本(可能已经提交到暂存区)。

##### 6.1.3.暂存区文件提交与撤销

​	当发现因失误而将文件添加到暂存区时，git支持文件的撤销操作。

​	执行命令：<font color=red> git reset HEAD fileName </font>或者<font color=red> git restore --staged fileName </font> 

​	<img src="E:\Storage\博客\Typora\image\Git版本控制\git rest HEAD.png" alt="reset" style="zoom:80%;" />

 	<font color = blue>两个命令的区别：</font>

1. reset指令是将指定文件返回上一层操作，如果不小心commit了也可以使用此命令。
2. restore指令是将暂存区文件移除。	

#### 6.2. 版本退回

##### 6.2.1.版本信息阅读

​	当文件修改后被提交的次数很多时，对于版本库中存放的文件就会出现不同的版本，在多人开发的项目环境中，通常会对不同版本文件进行查看甚至退回的情况。Git提供了此功能，可以让开发者在不同版本之中进行切换。

​	对于上面的test.txt 文件已经有几个版本，可以用 git log 查看：

![git log](E:\Storage\博客\Typora\image\Git版本控制\log.png) 

​	图中列表显示的结果按提交时间倒叙排列，其中第一条中HEAD -> master 代表当前指针指向的 Git版本库中master主干分支，每次提交Git内部均会生成一个唯一的SHA算法构建的 字符串来当作版本标识。

​	如果版本数目较多，想让日志更简洁一些，可以使用<font color=red>git log --pretty-oneline</font>命令。

##### 6.2.2.版本退回命令

​	回退版本有两种命令，分别为<font color=red>git reset --hard HEAD^</font>和<font color=red>git reset --hard 版本标识符</font>

- git reset --hard HEAD命令：

  ​	此命令是从HEAD指针开始计数，退几个版本就有几个 ^。如果要退回5个版本，那就是^^^^^

  ![HEAD](E:\Storage\博客\Typora\image\Git版本控制\reset HEAD.png)

- git reset --hard 标识符

  ​	此命令是回到指定标识符对应的版本。且标识符不一定要写全，从前向后匹配。

  ![标识符](E:\Storage\博客\Typora\image\Git版本控制\reset 标识符.png)

##### 6.2.3. 回到未来版本

​	回退操作完成后，可能想要回到未来的版本怎么办呢？其实只要找到版本对应的标识符即可，使用<font color = red>git reflog</font>命令来查看所有的版本。

![git reflog](E:\Storage\博客\Typora\image\Git版本控制\git reflog.png)

找到版本号后，再使用<font color = red>git reset --hard 版本号 </font>即可

 #### 6.3. 文件删除

​	在Git中，删除文件同样是一个修改操作，即Git只关注文件是否被修改（添加、删除、内容更改）。

##### 6.3.1. 使用add 和commit提交状态删除

- 首先要知道Git仓库里有哪些文件，需要使用命令<font color=red> git ls-files</font>。此命令用于调出Git仓库内的文件名。

  ![git ls-files](E:\Storage\博客\Typora\image\Git版本控制\git ls-files.png)

- 如果工作区的文件被删除，而Git仓库内的文件并没有被删除，使用<font color = red>git checkout</font>命令，可以从仓库内调出文件到工作区。

  ![git checkout](E:\Storage\博客\Typora\image\Git版本控制\git checkout.png)

- 当工作区文件(test02)被删除后，调用git status命令：

  ![git status](E:\Storage\博客\Typora\image\Git版本控制\git status(删除工作区文件).png)

  ​	可以发现，工作区更新了一个deleted状态，与文件提交一样，我们只要把deleted这个状态<font color=red> add </font>到暂存区，再<font color = red> commit </font>到Git仓库即可。

- 使用<font color=red>git add</font>和<font color=red>git commit</font>提交删除状态：

  ![删除](E:\Storage\博客\Typora\image\Git版本控制\Git仓库内删除.png)

##### 6.3.2. 使用 git rm 直接删除

- 使用<font color = red>git rm 文件名</font>可以直接删除工作区和Git仓库内的文件。


























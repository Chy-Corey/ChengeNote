

# Git学习文档（远程版）

### 7. 远程仓库

---

​	Git是一个分布式版本控制系统，同一个仓库，可以分布到不同的机器上。对于Git，除了前面提到的本地版本库以外，Git支持远程仓库的托管服务，也就是说使用者可以将版本库中的文件托管到远程服务器进行存储。所以，只要机器可以联网，就可以通过远程的仓库地址得到一份相同的项目库文件，并且下载到本地。本地的文件版本记录与远程文件版本保持一致，并且可以很方便的实现多人联机协同开发工作。

​	对于Git远程仓库，GitHub( https://github.com/ )是比较知名的一个，国内比较知名的就是码云( https://gitee.com/ )了。当然除了这些仓库外，有的公司处于安全考虑，也会自己搭建一套Git服务，内部仓库由专门人员维护。

#### 7.1. 克隆远程项目到本地

​	在GitHub点击下载按钮，会弹出仓库的http地址，复制以后在Git Bash命令区输入**git  clone 仓库地址** 即可。当然亦可以直接点击Download ZIP。

<img src="E:\Storage\博客\Typora\image\Git版本控制\git clone.png" alt="git clone" style="zoom: 67%;" />

#### 7.2. 将本地仓库推送到远程

- 创建本地版本库并提交文件到本地库

- 使用GitHub创建远程库：

  ![建立远程仓库](E:\Storage\博客\Typora\image\Git版本控制\建立远程仓库1.png)

  ---

  <img src="E:\Storage\博客\Typora\image\Git版本控制\建立远程仓库2.png" alt="创建远程仓库" style="zoom:53%;" />

  ---

  ---

- 推送本地Git版本库文件到远程Git仓库

  ​	推送本地到远程有两种方式

  - 使用Https
  - 使用SSH

  使用Https比较简单，使用SSH是Git建议的一种推送，效率更高且更安全，这里介绍使用SSH推送

   1. 使用本地Git客户端生成SSH公钥与私钥，执行命令 **ssh-keygen -t rsa -C "GitHub账户邮箱"**

      <img src="E:\Storage\博客\Typora\image\Git版本控制\SSH生成.png" alt="SSH生成" style="zoom: 67%;" />

      建立完成后就会在对应文件夹生成相应的私钥与公钥(有pub后缀)。

   2.  GitHub公钥配置

      <img src="E:\Storage\博客\Typora\image\Git版本控制\公钥配置.png" alt="公钥配置" style="zoom:67%;" />

      ---

      <img src="E:\Storage\博客\Typora\image\Git版本控制\公钥配置2.png" alt="公钥配置2" style="zoom:67%;" />

      ---

      <img src="E:\Storage\博客\Typora\image\Git版本控制\公钥配置3.png" alt="公钥配置3" style="zoom:67%;" />

      ---

      <img src="E:\Storage\博客\Typora\image\Git版本控制\公钥配置4.png" alt="公钥配置4" style="zoom:67%;" />

  3. 检查测试链接 执行命令 ***ssh -T git@github******.com***

     <img src="E:\Storage\博客\Typora\image\Git版本控制\公钥检测.png" alt="公钥检测" style="zoom:80%;" />

     **注意：**

     可能会出现如下错误：sshkey生成后未被放入本地系统，此时需要使用命令：

     ​	**ssh-agent bash** 和 **ssh-add "文件路径"**

     然后再执行测试链接命令。

     成功以后就可以使用SSH地址下载。

     SSH和http使用的区别就是地址格式不同。

  4. 使用SSH执行远程推送操作

     ​	在Git Bash界面使用以下代码即可提交(注意要在git仓库所在的文件夹打开Git Bash)：

     <img src="E:\Storage\博客\Typora\image\Git版本控制\SSH提交.png" alt="ssh提交" style="zoom:67%;" />

     只要成功一次，以后提交到远程只要使用 git push 即可。

### 8. Git分支操作

---

​	开发企业项目中，在使用Git或其他版本控制系统对项目进行管理时，多人合作的项目在开发时通常不会直接在主干master上进行操作，而是重新开辟新的分支，在新的分支上进行开发、调试等操作，当项目调试通过时，再合并到主干中，这时在实际开发中较好的策略。Git对于分支操作提供了以下基本命令：

|                 命令                 |                             描述                             |
| :----------------------------------: | :----------------------------------------------------------: |
|         git checkout branch          |                        切换到指定分支                        |
|      git checkout -b new_branch      |                   新建分支并切换到新建分支                   |
|         git branch -d branch         |                         删除指定分支                         |
|              git branch              |            查看所有分支，并且用*标记当前所在分支             |
|           git merge branch           |                合并分支（必须先切到主干分支）                |
| git branch -m/-M oldbranch newbranch | 重命名分支，如果newbranch名字分支已经存在，则需要使用-M强制重命名。 |

#### 8.1. 本地分支的创建、合并、重命名与删除

 - 创建本地分支、查看分支

   默认Git版本库所在分支为master，通常称为项目的主干，开发中通常会在主干上创建新的分支进行本地开发。使用命令：              **git checkout -b new_branch** 创建新的分支。

   创建出来的分支，里面的内容和主干中完全一样。

- 合并分支(先切换到主干)，删除分支

#### 8.2. 分支Push与Pull操作

相关命令：

|                       命令                        |               描述               |
| :-----------------------------------------------: | :------------------------------: |
|                   git branch -a                   |        查看本地与远程分支        |
|            git push origin branch_name            |        推送本地分支到远程        |
|          git push origin :remote_branch           |  删除远程分支（本地分支仍保留）  |
| git checkout -b local_branch origin/remote_branch | 拉取远程指定分支并在本地创建分支 |

使用这些命令即可。

#### 8.3. 分支操作冲突出现与解决

​	开发中对不同分支下同一文件进行修改后执行合并时就会出现文件修改冲突情况，这里说明一种比较常见的冲突问题，以main和top01两个分支进行演示说明。

##### 8.3.1. 本地分支操作冲突

​	当主干与分支存在冲突时，如果使用git merge branch，系统会提示你存在冲突的文件，并在文件内标出冲突的位置，此时需用户手动修改。

##### 8.3.2. 多人协同操作冲突

​	拉取远程库，并在本地创建开发库，执行命令 git checkout -b 本地库名字 origin/远程库名字。修改后执行推送操作，此时冲突出现原因是另外一个用户推送的文件与当前客户端推动内容存在冲突，此时解决方式Git已有对应提示Push之前先执行Pull操作，将远程文件拉取到本地，解决完冲突后再次执行Push操作。

### 9. 标签管理

---

标签管理基本命令 **git tag**

|                命令                 |                 描述                  |
| :---------------------------------: | :-----------------------------------: |
|          git tag tag_name           | 新建标签 默认为HEAD(最后一次提交记录) |
|    git tag -a tag_name -m 'xxx'     |      添加标签并指定标签描述信息       |
|               git tag               |             查看所有标签              |
|         git tag -d tag_name         |           删除一个本地标签            |
|      git push origin tag_name       |          推送本地标签到远程           |
|       git push origin --tags        |        推送全部本地标签到远程         |
| git push origin :refs/tags/tag_name |           删除一个远程标签            |

**注意：** 在命令后加上版本的唯一标识符，可以将标签指定添加到相应版本，如果不加标识符，就是默认最后一次提交记录。

​	同大多数VCS一样，Git也可以对某一时间点上的版本打上标签，开发中在发布某个软件版本（比如v1.0的等等）的时候，通常使用版本库软件命令来对某一版本打上一个标签，以方便标识。

​	打上标签并推送到远程以后，就可以根据标签来对相应版本进行下载。






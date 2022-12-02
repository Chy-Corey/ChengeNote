## Module & Package

Module 和 Package 有助于我们更好地使用python进行模块化编程，通过模块化编程，我们能把大的工程拆分成小的子任务和子模块，



### 一、模块(module)

`python`给我们提供了十分简单的方法去创建一个模块，我们只需要写一个`python`文件即可，也就是说写一个`.py`为后缀的文件，不必用额外的语法。

举个例子，我们现在写了一个文件，命名为`mod.py`，如下：

*mod.py*

```python
s = "If Comrade Napoleon says it, it must be right."
a = [100, 200, 300]

def foo(arg):
    print(f'arg = {arg}')

class Foo:
    pass
```

可以看到，我们在这个python文件里面创建了一下几个对象：

- s，一个字符串
- a，一个列表
- foo，一个函数
- Foo，一个类

假定这个文件路径正确，我们就可以把`mod.py`作为一个模块，在别的文件中引用，举例：

```python
>>> import mod
>>> print(mod.s)
If Comrade Napoleon says it, it must be right.
>>> mod.a
[100, 200, 300]
>>> mod.foo(['quux', 'corge', 'grault'])
arg = ['quux', 'corge', 'grault']
>>> x = mod.Foo()
>>> x
<mod.Foo object at 0x03C181F0>
```

现在我们就学会了如何创建和使用模块

#### 1. 模块搜索路径

继续上面的例子，当执行这句

```python
import mod
```

的时候，解释器都干了什么？

解释器主要是在如下几个路径中搜索`mod.py`文件：

1. 首先在当前执行的文件所在的文件夹中查看是否存在，如果当前文件夹不包含`mod.py`文件，那么这个搜索就失败了；
2. 在系统设置的python环境变量下的一些文件夹里面；
3. 一些在python安装时指定的文件夹下面；

具体我们可以使用：

```python
>>> import sys
>>> sys.path
['', 'E:\\Environment\\Anaconda\\python39.zip', 'E:\\Environment\\Anaconda\\DLLs', 'E:\\Environment\\Anaconda\\lib', 'E:\\Environment\\Anaconda', 'E:\\Environment\\Anaconda\\lib\\site-packages', 'E:\\Environment\\Anaconda\\lib\\site-packages\\locket-0.2.1-py3.9.egg', 'E:\\Environment\\Anaconda\\lib\\site-packages\\win32', 'E:\\Environment\\Anaconda\\lib\\site-packages\\win32\\lib', 'E:\\Environment\\Anaconda\\lib\\site-packages\\Pythonwin']
```

来查看在路径搜索时，都有哪些路径，当然搜索的结果因为安装，版本和操作系统的原因都不会相同，每个人的结果可能都不会相同。

因此，当我们`import`一些模块出现问题的时候，我们可以查看这个模块文件是不是在搜索路径中。

当我们想要把自己写的文件作为一个模块的时候，但是又不在搜索路径中，我们可以自己添加进去，比如：

```python
>>> sys.path.append(r'C:\Users\john')
>>> sys.path
['', 'C:\\Users\\john\\Documents\\Python\\doc', 'C:\\Python36\\Lib\\idlelib',
'C:\\Python36\\python36.zip', 'C:\\Python36\\DLLs', 'C:\\Python36\\lib',
'C:\\Python36', 'C:\\Python36\\lib\\site-packages', 'C:\\Users\\john']
```

#### 2. 让python文件即可以被当作模块引用，也能被当作脚本执行

比如前面的`mod.py`文件，

```python
s = "If Comrade Napoleon says it, it must be right."
a = [100, 200, 300]

def foo(arg):
    print(f'arg = {arg}')

class Foo:
    pass
```

当我们单独执行者文件的时候，是没有结果的输出的。

```python
s = "If Comrade Napoleon says it, it must be right."
a = [100, 200, 300]

def foo(arg):
    print(f'arg = {arg}')

class Foo:
    pass

print(s)
print(a)
foo('quux')
x = Foo()
print(x)
```

当执行这个文件时，有结果输出。但是当我们把这个文件当作一个模块，在其他文件中引用时，同样的结果会输出出来，这让不是我们需要的结果。

正确的做法是：

```python
s = "If Comrade Napoleon says it, it must be right."
a = [100, 200, 300]

def foo(arg):
    print(f'arg = {arg}')

class Foo:
    pass

if (__name__ == '__main__'):
    print('Executing as standalone script')
    print(s)
    print(a)
    foo('quux')
    x = Foo()
    print(x)
```

加上了`if (__name__ == '__main__'):`，这样python解释器就知道这个文件时单独在执行，还是被作为模块引用。在单独执行时，有输出，而在作为模块引用时，没有输出。



### 二、包(package)

简单来说，包就是多个模块的集合。当项目较大，模块较多时，我们就可以把模块放在包中，便于管理。

我们在包中经常能见到`__init__.py`文件。在python3.3版本之前，初始化一个包必须包含`__init__.py`文件，之后这就不必备的文件了，但是一般都会包含，不过需要配置，我们就在这个文件中写入一些指令，如果不需要的话，空文件也可以。在引用包中的模块时，使用`.`操作符即可，同样也要注意是不是在搜索路径中。

引用包时，需要使用 `from 包 import 模块` 或者 `import 包 ` 的语法：

```python
>>> from test import func
>>> func.test()
test package
>>>
```

一个package 被导入，不管在什么时候`__init__.py`的代码都只会被执行一次。

由于 package 被导入时 `__init__.py` 中的可执行代码会被执行，所以小心在 package 中放置你的代码，尽可能消除它们产生的副作用，比如把代码尽可能的进行封装成函数或类。

#### 1. 包的导入顺序

`__init__.py` 永远被优先导入和索引，当我尝试导入

```python
from package import something
```

`import`语句会首先检查`something`是不是`__init__.py`的变量，然后检查是不是`subpackage`，再检查是不是`module`，最后抛出`ImportError`。

所以检查顺序如下：

1. `__init__.py` 文件内变量
2. 是不是package内的`subpackage`
3. 是不是package内的`module`

#### 2. 如何导入包内的模块

当我们使用 `import 包` 语法而不使用 `from 包 import 模块` 时，如果 package 内有一个 `a.py` 文件，我们直接使用 `包.a` 是会**报错**的，python 无法找到这个模块a。因为 python 为了节省内存，**在导入包时并不会加载所有的模块**。

当然了，如果你希望使用`import 包`后自动加载模块，这里有两种办法。

第一种就是在`__init__.py`内导入`a`或者`b`模块，然后保存再激活`python`的交互环境

```python
#__init__.py
import a
import b
```

当你再次尝试`import 包`后，就可以包中的模块 `a` & `b` 了。

第二办法就是手动导入，当你想使用模块`a`中的`bar()`函数时，需要手动导入

```python
import package.a as a
# 或者
from package import a
```

然后就可以使用模块 `a` 里的变量、函数或者类等变量了。



### 3. \_\_init\_\_.py里的 \_\_all\_\_


在 \_\_all\_\_中可以声明定义允许用户可以调用的方法，入示例代码所示，只允许调用Test的A()方法

```python
# __init__.py
__all__ = ['A']

# python 程序
from package import *
B()	# 报错
```


如果强行调用没有放在 \_\_all\_\_里的方法会报错，总而言之，可以在 \_\_all\_\_中限定用户调用的范围。




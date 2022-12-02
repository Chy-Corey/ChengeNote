## Traceback

我们使用各种语言编写程序时，如果出错就能在终端看到报错信息，这其实是编程语言为我们封装好的函数，用来告诉程序员哪里出错了。Python 中返回报错信息的一个模块就是 Traceback，我们可以学习该模块，在使用 try exception 获取更详细的异常信息，或者自定义报错信息。



`traceback` 模块有两个常用的函数：

1. `traceback.print_exc()`
2. `traceback.format_exc()`

`traceback.print_exc()` 会直接打印出异常信息， `traceback.format_exc()` 会返回异常信息的字符串。

以下为使用举例：

```python
# 导入trackback模块
import traceback
class SelfException(Exception): pass
def main():
    firstMethod()
def firstMethod():
    secondMethod()
def secondMethod():
    thirdMethod()
def thirdMethod():
    raise SelfException("自定义异常信息")
try:
    main()
except:
    # 捕捉异常，并将异常传播信息输出控制台
    traceback.print_exc()
    # 捕捉异常，并将异常传播信息输出指定文件中
    traceback.print_exc(file=open('log.txt', 'a'))
```


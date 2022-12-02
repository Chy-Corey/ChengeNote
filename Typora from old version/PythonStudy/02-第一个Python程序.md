## 第一个Python程序

Python源文件以`.py`结尾

```python
print('hello world');print('hello')
fruits = ["apple", "banana", "cherry", 'watermelon']
for x in fruits:
    print(x)

i = 1
while i < 7:
    print(i)
    i += 1
```

Python对缩进的要求很高，很容易出现缩进错误，python规定缩进为4。

### 2.1. 注释

单行注释以`#`开头，并且＃后要附带一个空格符。如果要在代码的正后方添加注释，那么 # 要距离代码至少两个空格符

多行注释以 `"""` 开头且以 ` """` 结尾。
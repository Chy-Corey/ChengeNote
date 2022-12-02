## Global

### 1. 特点

全局对象，global中封装的方法不需要创建对象就可以直接调用

### 2. 方法

- encodeURI()：url编码
- decodeURI()：url解码
- encodeURIComponent()：url编码，编码的字符更多
- decodeURIComponent()：url解码

- parseInt()：将字符串转为数字
  - 注意判断每一个字符是否为数字，直到不是数字为止，将前边数字部分转为number
- isNaN()：判断一个值是否NaN
- eval()：将字符串作为脚本代码执行
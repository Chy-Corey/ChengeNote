## Bom

- 定义：

  Browser Object Model：浏览器对象模型。将浏览器的各个组成部分封装为对象

- 组成：

  Window：窗口对象

  Navigator：浏览器对象

  Screen：显示器屏幕对象

  History：历史记录对象

  Location：地址栏对象

### 1. Window

#### 1.1. 方法

1. 与弹出框有关的方法：
   - alert()
   - confirm()    显示带有一段消息和一个确认按钮和取消按钮的对话框
     - 如果点击确定，返回true
     - 如果点击取消，返回false
   - prompt()    显示可提示用户输入的对话框
     - 返回值：用户的输入值

2. 与打开关闭有关的方法：
   - close()    关闭浏览器窗口
   - open()    打开一个新的浏览器窗口
     - 返回新的window对象
3. 与定时器有关的方法
   - setTimeout()    指定的毫秒数后调用参数
     - param1：js代码或者方法对象
     - param2：毫秒值
     - 返回值：唯一标识，用于取消定时器
   - clearTimeout()    取消由setTimeout()方法设置的timeout
   - setInterval()    循环定时器，按照指定的周期来调用函数
   - clearInterval()    取消由setInterval设置的timeout

#### 1.2. 属性

1. 获取其他Bom对象
   - history	location	navigator	screen
2. 获取DOM对象
   - document

### 2. Location

1. 创建

   1. window.location
   2. location

2. 方法

   reload()    刷新

3. 属性

   href 设置或返回完整的url

### 3. History

1. 创建

   - window.history
   - history

2. 方法：

   - back()	返回上一个url
   - forward()    加载history列表中下一个url
   - go(param)    加载history列表中某个具体页面
     - 参数为正数时，前进
     - 参数为负数时，后退

3. 属性

   length：history中url数量





























